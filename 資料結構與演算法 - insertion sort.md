# 資料結構與演算法 - insertion sort
###### tags: `資料結構與演算法`,`Algorithms`,`Data Structure`

## 插入排序法
一個沒有排序過的陣列，將元素用「插入正確位置」的方式，使其排列正確。
很類似玩樸克牌或拉密時，整理手牌的時候會做的事情。

## 思考
就像在打樸克牌，拿起手牌，不斷往前找到適合的位置，等找到的時候就可以直接插入。陣列中迴圈拿元素跟前一個元素比大小，前一個比較大，就往前插入。

## 實作
```javascript=
let unsortedArray = [14, -4, 37, -6, 22, 16, 55, 0];

function insertionSort(arr) {
  for (let j = 1; j <= arr.length - 1; j++) {
    let key = arr[j];
    i = j - 1;
    while (i >= 0 && arr[i] > key) {
      arr[i + 1] = arr[i];
      i -= 1;
    }
    arr[i + 1] = key;
  }

  console.log(arr);
  return arr;
}

insertionSort(unsortedArray)
```

## 時間複雜度
效率上會比泡沫排序法還好。在泡沫排序法中，如果元素排列順序錯誤，兩個元素就要交換位置；而插入排序法只需要將一個元素插入到正確位置。
* 最差：O(n^2)
* 最佳：O(n)
* 平均：O(n^2)

