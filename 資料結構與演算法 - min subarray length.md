# 資料結構與演算法 - min subarray length
###### tags: `資料結構與演算法`,`Algorithms`,`Data Structure`

## 問題
寫出一個有兩個參數的函式，第一個參數為內容都是正整數的陣列，第二個參數為正整數。從第一個參數中找出「子陣列」，其總和大於等於第二個參數，求出最短的子陣列長度。無法求得就回傳0。

子陣列為：一個子陣列，是從另一個陣列拿去零個以上連續的元素。例如：
```
const arr = [1,2,3,4,5,6] 
[1,2,3]  // 為arr的子陣列
[2,3,4] // 為arr的子陣列
[2,5,6] // 不為arr的子陣列
[1,2,3,4,7] // 不為arr的子陣列
```

## 思考
對該陣列跑迴圈，初始總和為0，當迴圈跑到該元素加入總和，沒有大於等於第二個參數，就持續往該元素下一個累加，直到超過，無法求得就回傳0。
```javascript=
//sum要求要10
// 箭頭都從第一個元素開始指向
// total目前為1

[1,2,3,4,5,6] 
 ^start
 ^end

// 總和沒超過10 就移動end箭頭 
// 總和為3 以此類推

[1,2,3,4,5,6] 
 ^start
   ^end
   
// 總和大於等於10 紀錄當前陣列的長度
// 改為移動start箭頭

[1,2,3,4,5,6] 
   ^start
       ^end
```
不用迴圈一個個跑陣列，而用**箭頭**移動的方式，是因為陣列會跑到並累加重複的元素。例如：
```
[1,2,3,4,5,6] 

[1,2,3,4] // 總和是10
[2,3,4,5] // 總和是14
// 兩組都符合目標，但重複計算了三個元素
```

## 實作
```javascript=
function minSubArray(arr, sum) {
  let start = 0 // 指向子陣列第一個元素的箭頭
  let end = 0 // 指向子陣列最後一個元素的箭頭
  let total = 0 // 當前總和
  let minLength = Infinity // 要求的最小陣列長度
  const arrLen = arr.length
  
  while (start < arrLen) {
    if (total < sum && end < arrLen) {
      total += arr[end];
      end++;
    } else if (total >= sum) {
      let currentLength = end - start;
      if (minLength > currentLength) {
        minLength = currentLength;
      }
      total -= arr[start];
      start++;
    } else if (end >= arrLen) {
      break;
    }
  }

  if (minLength === Infinity) {
    return 0;
  } else {
    console.log(minLength);
    return minLength;
  }
}
const testArr = [8, 1, 6, 15, 3, 16, 5, 7, 14, 30, 12]
const testArr = 70
minSubArray(testArr, testSum);
```