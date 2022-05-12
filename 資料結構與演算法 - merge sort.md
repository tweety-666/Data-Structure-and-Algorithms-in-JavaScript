# 資料結構與演算法 - merge sort
###### tags: `資料結構與演算法`,`Algorithms`,`Data Structure`

## 合併排序
還沒排序好的大陣列分成好幾個小小陣列，小小陣列各自比較大小後，排序好成一個小陣列，小陣列跟小陣列比較後，合併成一個排序好的大陣列。

## divide and conquer
> 分割 -> 比較 -> 合併

divide and conquer的概念類似分組排名賽。
假設一個陣列是8人比賽隊伍，要依照比賽成績去排名，8人分成兩邊各4人；4人再去分成兩邊各2人；2人再去分成兩邊各1人，從1人數隊伍開始較勁，得到成績後去做比賽排名。

## 邏輯推理、舉例
1. 一個隊伍八個成員：`[-1,16,-2,3,-5,7,8,4]`
2. 全部拆成自己一組：`[-1], [16], [-2], [3], [-5], [7], [8], [4]`
3. 兩兩一組去比賽並且排序成新隊伍：
```
[-1] vs [16] => [-1,16]
[-2] vs [3] => [-2,3]
[-5] vs [7] => [-5,7]
[8] vs [4] => [4,8]
```
4. 依此類推，直到變成唯一的組：
```
newArray=[] 該陣列代表排序後的新隊伍組合 初始為空陣列
兩邊第一個元素比大小
[-1,16] vs [-2,3]
 ^           ^

newArray新增-2 輸的隊伍換下一個元素去比
[-1,16] vs [-2,3]
 ^             ^
 
newArray新增-1 輸的隊伍換下一個元素去比
[-1,16] vs [-2,3]
    ^          ^
    
newArray新增3 右邊隊伍比到超出範圍了 剩下16還沒進去
[-1,16] vs [-2,3]
    ^            ^
    
只要有其中一邊比到超出範圍了 而另一邊還沒 就把剩下的成員都加入newArray
newArray = [-2,-1,3,16]

以此類推 [-5,7] vs [4,8] => [-5,4,7,8]
以此類推 [-2,-1,3,16] vs [-5,4,7,8] => [-5, -1, -2, 3, 4, 7, 8, 16]
```

## 時間複雜度
假設8人比賽，1人自己一個隊伍(1+1+1+1+1+1+1+1)，比大小後合併(2+2+2+2)，再次比賽合併(4+4)，再次比賽合併(8)。
隊伍數量（陣列數量）變化為：`8 -> 4 -> 2 -> 1`
也就是說，每次合併都除以2，n個元素做好幾次合併成為1，那n要做幾次合併才會變成1？
每次合併都要除以2，假設合併k次，數學寫為：
```
n/(2^k)=1
-> 2^k=n
-> log2 n = k
```
單一陣列要先比較大小排序，再合併，直到成為唯一的陣列。

依照上述的例子，假設8人比賽，每次排序要比較大小8次，合併總共3次。該例子複雜度為`8*3=24`。
```
(1+1+1+1+1+1+1+1) => 自己1人一組 互相比8次 比完合併成2人一組
(2+2+2+2) => 還是比8次 比完合併成4人一組
(4+4) => 還是比8次 比完合併成8人一組
(8) =>只剩下這個陣列了
```

那麼n個元素比大小，就要比較n次，比較完之後合併，要合併k次，k為log 2 n，複雜度為`n*log2 n`。

* 最佳：O(nlogn)
* 最差：O(nlogn)
* 最平均：O(nlogn)

## 實作
```javascript=
// 兩組已經排序過的陣列 比大小 組成新的排序陣列
function mergeTwoTeam(sortedArr1,sortedArr2){
    let result = []
    let left = 0 // 左邊隊伍的index
    let right = 0 // 右邊隊伍的index
    const leftLen = sortedArr1.length
    const rightLen = sortedArr2.length
    
    while(left < leftLen && right < rightLen){
        if(sortedArr1[left] > sortedArr2[right]){
            result.push(sortedArr2[right])
            right++
        }else if(sortedArr1[left] < sortedArr2[right]){
             result.push(sortedArr1[left])
             left++
        }else{
            // 兩邊一樣大
            result.push(sortedArr1[left])
            result.push(sortedArr2[right])
            left++
            right++
        }
    }
    
    // 一組比完後 有可能有還有隊伍會有剩下的成員 全部推上去
    // ex: [-1,-2,3,4] vs [-5,7,8,16]
    while(left>=leftLen && right < rightLen ){
        result.push(sortedArr2[right])
        right++
    }
    while(right>=rightLen && left < leftLen ){
        result.push(sortedArr1[left])
        left++
    }
    console.log(result)
    return result
}
// mergeTwoTeam([-1,-2,3,4],[-5,7,8,16]) // [-5, -1, -2, 3, 4, 7, 8, 16]

// 把單一元素拆成兩兩組別去較勁 直到沒成員可以再去拆分
function mergeSort(arr){
    if (arr.length === 1) {
        return arr;
  } else {
        let middle = Math.floor(arr.length / 2); // 拆的中心
        let left = arr.slice(0, middle);
        let right = arr.slice(middle, arr.length); 
        return merge(mergeSort(right), mergeSort(left)); // 利用到遞迴的概念
  }
}

const unsortedArray = [15,6,7,33,2,-12,10,-3,29,-21]
mergeSort(unsortedArray)
```