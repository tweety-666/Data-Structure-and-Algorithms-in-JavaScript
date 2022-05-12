# 資料結構與演算法 - linear time sort and counting sort
###### tags: `資料結構與演算法`,`Algorithms`,`Data Structure`

## linear time sort
線性排序的算法，不是基於比較的排序算法，都不涉及元素之間的比較。

## counting sort
1. 只能對整數做排序
2. 有n個元素要排列，元素的範圍是0~k

### 問題
陣列中有10個元素，範圍從0到5，依照大小排序。

### 解法
1. 列出計分板：
```
const counter= {
    0:0, // 元素範圍數字: 出現次數
    1:0,
    2:0,
    3:0,
    4:0,
    5:0,
}
```
2. 對陣列跑迴圈，計算每種元素出現的次數。
3. 回傳排序好的陣列。
```javascript=
const testArr = [0,0,1,2,5,3,3,1,4,2]
const counter= {
    0:0, // 元素範圍數字: 出現次數
    1:0,
    2:0,
    3:0,
    4:0,
    5:0,
}

function countingSort(arr){
    let result =[]
    for(let i=0;i<arr.length;i++){
        counter[arr[i]]+=1
    }
    
    for (let key in counter) {
        const times = counter[key] // 要推送的次數
        for(let i=0;i<times;i++){
            result.push(key)
        }
    }
    console.log(result)
    return result
}
countingSort(testArr)
```
優化一下：
```javascript=
const testArr = [0,0,1,2,5,3,3,1,4,2]

// 根據元素範圍 產生計分板
function getCounter(k){
    const counter={}
    for(let i=0;i<=k;i++){
        counter[i] = 0
    }
    console.log('counter',counter)
    return counter
}

function countingSort(arr,k){
    const counter = getCounter(k)
    let result =[]
    // 對陣列跑迴圈n次
    for(let i=0;i<arr.length;i++){
        counter[arr[i]]+=1
    }
    
    for (let key in counter) { // 從計分板推送的元素種類 跑k+1次
        const times = counter[key] // 最多推送n次
        if(times>0){
            for(let i=0;i<times;i++){
                result.push(key)
            } 
        }
    }
    
    console.log(result)
    return result
}
countingSort(testArr,5)

```

## 再優化
如果範圍不是0~k，而是兩個數（min和max）中間，假設min和max差異為k：
```javascript=
const testArr = [3,6,7,3,4,8,8,5,4,7] 
function countingSort(arr, min, max) {
    let j = 0;
    let storeArray = []; // 計分板改為用陣列 不用物件
    
    for (let i = min; i <= max; i++) {
        storeArray[i] = 0;
    }
    
    for (let i=0; i < arr.length; i++) {
        storeArray[arr[i]] += 1;
    }
       
    for (let i = min; i <= max; i++) {
        while (storeArray[i] > 0) {
            arr[j++] = i;
            storeArray[i] -= 1;
        }
    }
    return arr;
}
countingSort(testArr,3,8)
```
### 時間複雜度
從計分板推送的元素種類迴圈，跑k次；對陣列內記元素出現次數跑了n次。時間複雜度為O(n+k)。
