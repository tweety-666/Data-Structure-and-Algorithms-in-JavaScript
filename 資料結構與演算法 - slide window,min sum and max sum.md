# 資料結構與演算法 - slide window,min sum and max sum
###### tags: `資料結構與演算法`,`Algorithms`,`Data Structure`

## 什麼是slide window
假設一串陣列是`[A,B,C,D,E,F,G]`，4個元素為一組，每個元素依序當開頭，直到slide到最後一個元素：
```
[A,B,C,D]
  [B,C,D,E]
    [C,D,E,F]
      [D,E,F,G]
```
## 問題
寫出`minSum()`跟`maxSum()`，第一個參數為一串數字陣列，第二個參數代表幾個元素為一組slide，利用slide window的特性，求出每組slide的總和，分別取得最小跟最大的總和。前提為slide的size不超過陣列長度。例如：
```javascript=
const arr = [1,2,3,4,5,6]
const count = 3 // 3個一組為slide
maxSum(arr,count) // 15
// [1,2,3] total => 6
//   [2,3,4] total => 9
//     [3,4,5] total => 12
//       [4,5,6] total => 15
minSum(arr,count) // 6
```

## 思考
假設4個一組，陣列長度為6，從最後一個元素往前推(4-1=3)個，組為一組:
```
// [1,2,3,4] total => 10
//   [2,3,4,5] total => 14
//     [3,4,5,6] total => 18
```
假設3個一組，從最後一個元素往前推(3-1=2)個，組為一組:
```
// [1,2,3] total => 6
//   [2,3,4] total => 9
//     [3,4,5] total => 12
//       [4,5,6] total => 15
```

如果從開頭往後算，第一個元素index是0，往後推算(m-1)個元素，從0到m-1組成一組；第二個元素index是1，往後推算(m-1)個元素，從1到m組成一組；第三個元素index是2，往後推算(m-1)個元素，從2到m+1組成一組。以使類推，得知：
從第某個元素index是i，往後推算(m-1)個元素，算到第i+m-1個為一組，i+m-1不能超過陣列最後一個元素的index：
```
i+m-1 <= arr.length-1
i+m <= arr.length
```

## 實作
```javascript=
const reducer = (previousValue, currentValue) => previousValue + currentValue;
function minSum(arr,size){
    let min = null;
    const len = arr.length
    for(let i=0;i<len;i++){
        let team = []
        for (j=0;j<size;j++){
            if(i+j<=len-1){team.push(arr[i+j])}
        }
        let total  = team.reduce(reducer)
        if(!min){
            min = total
        }else{
            if(min > total){
                min = total
            }
        }
    }
    return min
}

function maxSum(arr,size){
    let max = null;
    const len = arr.length
    for(let i=0;i<len;i++){
        let team = []
        for (j=0;j<size;j++){
            if(i+j<=len-1){team.push(arr[i+j])}
        }
        let total  = team.reduce(reducer)
        if(!max){
            max = total
        }else{
            if(max < total){
                max = total
            }
        }
    }
    return max
}

const testArr = [2,7,3,0,6,1,-5,-12,-11]
console.log(minSum(testArr,3)) // -28
console.log(maxSum(testArr,3)) // 12
```

## 除錯
上述的做法有些問題，這段：
```javascript=
 for (j=0;j<size;j++){
     if(i+j<=len-1){team.push(arr[i+j])}
 }
```
當迴圈跑到陣列`[2,7,3,0,6,1,-5,-12,-11]`中的`-12`時，`team`陣列會把`-12`跟`-11`加進來，但這樣其實不滿三個成員。雖然不影響結果，但其實成員不滿三個，就沒有必要再算下去。
所以只要`team`長度不滿足`size`就不需要加總，直接回傳結果：
```javascript=
const reducer = (previousValue, currentValue) => previousValue + currentValue;
function minSum(arr,size){
    let min = null;
    const len = arr.length
    for(let i=0;i<len;i++){
        let team = []
        for (j=0;j<size;j++){
            if(i+j<=len-1){team.push(arr[i+j])}
        }
        if(team.length !== size) return min //到這邊就可以不用再算下去了
        let total  = team.reduce(reducer)
        if(!min){
            min = total
        }else{
            if(min > total){
                min = total
            }
        }
    }
    return min
}

function maxSum(arr,size){
    let max = null;
    const len = arr.length
    for(let i=0;i<len;i++){
        let team = []
        for (j=0;j<size;j++){
            if(i+j<=len-1){team.push(arr[i+j])}
        }
        if(team.length !== size) return //到這邊就可以不用再算下去了
        let total  = team.reduce(reducer)
        if(!max){
            max = total
        }else{
            if(max < total){
                max = total
            }
        }
    }
    return max
}

const testArr = [2,7,3,0,6,1,-5,-12,-11]
console.log(minSum(testArr,3)) // -28
console.log(maxSum(testArr,3)) // 12
```

## 優化
想要盡量縮減迴圈跑的次數。
上述的第一層迴圈，根據先前的思考：從第某個元素index是i，往後推算(m-1)個元素，算到第i+m-1個為一組，i+m-1不能超過陣列最後一個元素的index。
```
i+m-1<=len-1
i+m <= len
i<=len-m
```
也就是說，第一層迴圈，最多跑到len-m這個index。
第二層迴圈則是不需要從0開始，從i開始，雖然這樣跑的次數一樣是size。
不需要用到陣列去算總和，從0開始加數字就好。
```javascript=
function minSum(arr,size){
    let min = null;
    const len = arr.length
    for(let i=0;i<=len-size;i++){
       let total = 0
        for (j=i;j<i+size;j++){
            total+=arr[j]
        }
        if(!min){
            min = total
        }else{
            if(min > total){
                min = total
            }
        }
    }
    return min
}


function maxSum(arr,size){
    let max = null;
    const len = arr.length
    for(let i=0;i<=len-size;i++){
        let total = 0
        for (j=i;j<i+size;j++){
            total+= arr[j]
        }
        if(!max){
            max = total
        }else{
            if(max < total){
                max = total
            }
        }
    }
    return max
}

const testArr = [2,7,3,0,6,1,-5,-12,-11]
console.log(maxSum(testArr,3)) // 12
console.log(minSum(testArr,3)) // -28
```

## 再次優化
會發現每次再計算slide內的總額的時候，會有size-1個是重複的，例如：
```
// [1,2,3,4] total => 10
//   [2,3,4,5] total => 14
```
上述的兩個slide，2、3、4重複，算總額的時候，可以把前者總額減去前者的第1個元素，加上後者的最後一個元素。有比當前紀錄到的總額大（或小），就更新值。
```javascript=
function maxSum(arr, size) {
  let max = 0
  // 第一次算總和
  for (let i = 0; i < size; i++) {
    max += arr[i];
  }

  let temp = max;
    // 有總和之後 加上下一個元素 減去上個slide中的第一個元素
  for (let j = size; j < arr.length; j++) {
    temp = temp + arr[j] - arr[j - size];
      // 更新值
    if (temp > max) {
      max = temp;
    }
  }

  console.log(max);
  return max;
}

function minSum(arr, size) {
  let min = 0
  // 第一次算總和
  for (let i = 0; i < size; i++) {
    min += arr[i];
  }

  let temp = min;
    // 有總和之後 加上下一個元素 減去上個slide中的第一個元素
  for (let j = size; j < arr.length; j++) {
    temp = temp + arr[j] - arr[j - size];
      // 更新值
    if (temp < min) {
      min = temp;
    }
  }

  console.log(min);
  return min;
}
const testArr = [2,7,3,0,6,1,-5,-12,-11]
maxSum(testArr,3)
minSum(testArr,3)

```