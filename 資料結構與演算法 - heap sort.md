# 資料結構與演算法 - heap sort
###### tags: `資料結構與演算法`,`Algorithms`,`Data Structure`

## 問題
請將一個任意的complete binary tree，轉換成max heap，再轉成一個array。
例如：
`let array = [3,12,14,7,1,13,10,5,9,4,11,8,15,2,6]`
得出max heap：
![](https://i.imgur.com/sjvxFCI.png)

max heap轉為陣列：
`let maxHeapArray = [15,14,12,13,11,7,10,8,4,9,1,5,3,6,2]`

## 思考
假設有個max heap轉array為`[A,B,C,D,E,F,G]`，將這個heap sort過的陣列打亂為`[A,F,C,B,E,G,D]`:

```
heapSortArray = [A,B,C,D,E,F,G] // 假設剛好是由大到小排列
unsortedArray = [A,F,D,B,E,G,C]
```
排列成二元素：
```
   A,
  F,D,
B,E,G,C  
```
可以看出每個subtree，進行比大小。
```
   A,   // A最大 A可以當root
  F,D 
```
```
   F,   // F跟B比 B比較大 
  B,E 
 
   B,   // E跟B比 B比較大   B可以當root
  F,E 
```
```
  D,   // D跟G比較 D比較大 
 G,C

  C,   // D跟C比較 C比較大 C可以當root
 G,D
```
每個二元樹都比較過了，都是max heap，那將這些max heap組合在一起，組合在一起時，又要比較大小，成為新的max heap：
```
   A,   
  F,D  // CD要比較 D輸了下去 
  F C  
B,E G,D   

   A,   
  F,C
B,E,G,D
```

### 從array得出subtree（subtree會是binary tree）
1. 從上圖，用肉眼就可以列出陣列：
`[15,14,12,13,11,7,10,8,4,9,1,5,3,2,6]`
2. 開始思考上面的陣列，怎麼排列成binary tree？可以發現每三個元素是一組subtree，`[15,14,12]`是第一組，第一個當root，剩下依序是left node跟right node。`[14,13,11]`一組、`[12,7,10]`一組、`[13,8,4]`，這邊有個規律是：

    > 當第i個元素是root，第2i+1個元素跟第2i+2個元素是他的child node。

3. 反推回去找該root的parent node，舉例：`[14,13,11]`，15是root 14的
parent node；`[12,7,10]`，15是root 12的parent node；`[13,8,4]`，14是root 13的parent node。得出規律是：
    > 當第i個元素是root，第(i-1)/2個元素是他的parent node。

### max heapify
陣列當然不會像上述範例這麼剛好已經排序好了，換成題目的`[3,12,14,7,1,13,10,5,9,4,11,8,15,2,6]` 試試看。
依照規律：**當第i個元素是root，第2i+1個元素跟第2i+2個元素是他的child node。**
i為0，找出第一組subtree：`[3,12,14]`

那第i個元素的parent node為：
**當第i個元素是root，第(i-1)/2個是該元素的parent node。**

* 針對一組subtree進行max heapify：
`[3,12,14]`代表：root是3，left node是12，right node是14。
root先跟left node比較大小，決定誰是root，變為`[12,3,14]`，root改為12，再來跟right node比較，變為`[14,3,12]`。
在二元樹中root依序跟左右節點比大小決定誰要當root，這樣的過程就是max heapify。

### build max heap
要從一組二元樹中進行max heapify還算簡單。陣列內的確可以列出二元樹們。但這些二元素們就算已經max heapify後組合在一起，要組在一起成為新的二元樹後，又要再次max heapify。如上述思考的最後一個步驟，這個步驟是最難的。

從上述得知結論：
**1. 當第i個元素是root，第(i-1)/2個是該元素的parent node。**
**2. 當第i個元素是root，第2i+1個元素跟第2i+2個元素是他的child node。**

既然得知一個node的parent node跟child node怎麼找了，就可以從題目的陣列中決定誰要先當parent來執行max heapify：

```javascript=
let arr = [3,12,14,7,1,13,10,5,9,4,11,8,15,2,6]
function buildMaxHeap() {
  heapSize = arr.length - 1;
  for (let i = Math.floor(heapSize / 2); i >= 0; i--) {
    maxHeapify(i); // 從陣列中抓取一個node當parent node
  }
}

// 給予一個parent node當root 跟child node比較 得出誰是root
function maxHeapify(i) {
  let largest; // 這是root的位置
  let l = i * 2 + 1; // 左節點
  let r = i * 2 + 2; // 右節點
    // parent先跟左邊比
  if (l <= heapSize && arr[l] > arr[i]) {
    largest = l;
  } else {
    largest = i;
  }
    // 贏家再來跟右邊比
  if (r <= heapSize && arr[r] > arr[largest]) {
    largest = r;
  }
    // 如果帶進來的node不是最大 就交換位置
    // 如果是最大的 就是root 保持原樣 不用交換
  if (largest != i) {
    let temp = arr[i];
    arr[i] = arr[largest];
    arr[largest] = temp;
      
     // 這組subtree得出max heap了 
     // 該組root 也會是別人的parent node 再拿去跟child比較
    maxHeapify(largest); 
  }
}
```
### max heap轉為array
這邊有個max heap：
![](https://i.imgur.com/sjvxFCI.png)
用肉眼轉成陣列很簡單，但如果透過原本題目的陣列，該怎麼轉換呢？
很明顯可以知道的是，max heap的第一個元素是最大的。
將該陣列的最後一個元素，跟該陣列第一個元素交換，把該元素當成最大的root拿去進行maxHeapify，就沒該元素的事了，該元素退場，讓heap size減一。陣列中剩下其他元素要繼續進行一樣的步驟。
```javascript=
for (let i = arr.length - 1; i >= 0; i--) {
    // 把 arr[0] 跟arr[i] 交換
    let temp = arr[0];
    arr[0] = arr[i];
    arr[i] = temp;
   
    heapSize -= 1; // 沒該元素的事了
    maxHeapify(0);  // 交換後 當成root 去進行maxHeapify 
  }
```

## 實作
```javascript=
let heapSize;
let arr = [15, 3, 17, 18, 20, 2, 1, 666];
heapSort();
console.log(arr);

function buildMaxHeap() {
  heapSize = arr.length - 1;
  for (let i = Math.floor(heapSize / 2); i >= 0; i--) {
    maxHeapify(i);
  }
}

function maxHeapify(i) {
  let largest;
  let l = i * 2 + 1;
  let r = i * 2 + 2;
  if (l <= heapSize && arr[l] > arr[i]) {
    largest = l;
  } else {
    largest = i;
  }

  if (r <= heapSize && arr[r] > arr[largest]) {
    largest = r;
  }

  if (largest != i) {
    let temp = arr[i];
    arr[i] = arr[largest];
    arr[largest] = temp;
    maxHeapify(largest);
  }
}

function heapSort() {
  buildMaxHeap(); // 組成max heap
    
    // max heap要轉為陣列
  for (let i = arr.length - 1; i >= 0; i--) {
    let temp = arr[0];
    arr[0] = arr[i];
    arr[i] = temp;
    heapSize -= 1;
    maxHeapify(0);
  }

  return arr;
}
```

## 時間複雜度
1. buildMaxHeap：
    從函式可以看出迴圈跑了(n-1)/2次執行maxHeapify。從下方得知：貞烈長度n時，maxHeapify要算log2 n次。

2. maxHeapify：
    二元樹中：
    第0層layer，深度0，2^0=1，有1個node。
    第1層layer，深度1，2^1=2，有2個node。
    第2層layer，深度2，2^2=4，有4個node。
    第3層layer，深度3，2^3=4，有8個node。
    也就是說，n個node會有log2 n層要去做運算。
    陣列長度n時，要算log2 n次，複雜度是O(logn)。
    
3. heapSort：
    heapSort執行了buildMaxHeap，跟自己跑迴圈n-1次的maxHeapify。
    上兩個組起來，複雜度為O(n*logn)。

* 最差：O(n*logn)
* 最佳：O(n*logn)
* 平均：O(n*logn)
    