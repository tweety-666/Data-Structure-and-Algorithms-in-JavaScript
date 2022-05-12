# 資料結構與演算法 - linear time sort and radix sort
###### tags: `資料結構與演算法`,`Algorithms`,`Data Structure`

## radix sort
radix sort又稱“桶子排序”，從個位數開始一直往更多位數比較大小，將要排序的元素分配至某些“桶子”中。

## 思考
1. 建立代表0~9的桶子。
2. 比較陣列中的每個數字的個位數。
3. 依序把數字放入桶子中。
4. 這個位數比完了，從桶子中拿出來，排序到陣列內。
5. 比較下一個位數，也就是十位數。依序把數字放入桶子中。
6. 再一次，這個位數比完了，從桶子中拿出來，排序到陣列內。
7. 依此類推直到比完。

## 實作
```javascript=
const testArr = [10, 200, 13, 12, 7, 88, 91, 24]

function radixSort(arr) {
  const max = Math.max(...arr); // 確認最多有幾位數要問
  const buckets = Array.from({length:10},()=>[]) // 0~9的桶子

  let m = 1; // 從幾位數開始比較大小
  while (m < max) {
    // 對陣列跑迴圈放入桶
    arr.forEach(number => {
      const digit = Math.floor((number % (m * 10)) / m); //取第某位數的值
      buckets[digit].push(number);
    });

    
     // 這個位數比完了 從桶子中拿出來排序到陣列
      let index = 0
    buckets.forEach(bucket => {
      while (bucket.length > 0) {
        arr[index++] = bucket.shift(); // 桶子先進先出
      }
    });

    m *= 10; // 往下個位數前進
  }
    console.log(arr)
}

radixSort(testArr)

```
