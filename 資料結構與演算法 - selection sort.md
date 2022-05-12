# 資料結構與演算法 - selection sort
###### tags: `資料結構與演算法`,`Algorithms`,`Data Structure`

## 選擇排序法
在未排序的陣列中，一直去找尋最小的值，再去跟陣列第一個互換。

## 思考
要先在陣列中依序找出最小的值。因為是兩兩一組比較，前一個元素index最多只會到陣列長度的-2，後一個元素index最多只會到陣列長度的-1。

怎麼找出最小值，一開始先將第一個元素(index為0)當成最小值，下一個元素要是比它小就交換。

```
[A,B,C,D,E]
 ^ 一開始A當成最小
   ^ A跟下一個元素B比較
   
[A,B,C,D,E]
   ^ A最小 固定在最左邊 沒他的事情了 開始換B當「還沒排序的陣列中的最小值」
     ^ B跟下一個元素C比較 以此類推
```

## 實作
```javascript=
let unsortedArray = [14, -4, 17, 6, 22, 1, -5,35,8,-12];

function selectionSort(arr) {
  for (let i = 0; i <= arr.length - 2; i++) {
    let minIndex = i;
    for (let j = i; j <= arr.length - 1; j++) {
      if (arr[j] < arr[minIndex]) {
        minIndex = j;
      }
    }
    // 交換
    let temp = arr[minIndex];
    arr[minIndex] = arr[i];
    arr[i] = temp;
    console.log(arr);
  }

  console.log(arr);
  return arr;
}

selectionSort(unsortedArray);
```

## 時間複雜度
* 最差：O(n^2)
* 最佳：O(n^2)
* 平均：O(n^2)
