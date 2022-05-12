# 資料結構與演算法 - bubble sort
###### tags: `資料結構與演算法`,`Algorithms`,`Data Structure`

## 泡沫排序法
泡沫排序為在一個陣列中比較相鄰的元素，如果位置是錯（大的在小的前面）的就要互換。

## 思考
陣列內兩兩一組去比較大小，排序錯誤就交換。

## 實作
```javascript=
function bubbleSort(array) {
  const length = array.length;
  for (let i = 0; i < length; i++) {
    for (let j = 0; j < length; j++) { 
      if(array[j] > array[j+1]) {
        //交換數字
        let temp = array[j]
        array[j] = array[j+1];
        array[j+1] = temp;
      }
    }        
  }
    console.log(array)
}


let test = [];
for (let i = 0; i < 100; i++) {
  test.push(Math.floor(Math.random() * 100));
}

bubbleSort(test);

```

## 時間複雜度
* 最差：O(n^2)
* 最佳：O(n)
* 平均：O(n^2)