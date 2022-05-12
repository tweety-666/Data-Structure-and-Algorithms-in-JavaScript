# 資料結構與演算法 - intersection problem
###### tags: `資料結構與演算法`,`Algorithms`,`Data Structure`

## 問題
求兩個陣列的交集？
```javascript=
function intersection(arr1,arr2){
    //...想想該怎麼做
}
let A = [1,2,3,4]
let B = [2,4,6,8]
intersection(A,B) // 答案為：[2,4]
```
## 思考
最暴力的方式為：對A跟B跑迴圈，一個個問是不是一樣的，但雙層迴圈光是用想的就覺得很耗成本，複雜度為O(n^2)。
想要降低複雜度，那如果改為一個迴圈呢？
把AB陣列組合起來，成為新的陣列，跑一次迴圈，去看看哪些數字出現次數是兩個以上，這樣複雜度為O(n)。

## 實作

```javascript=
let counter = {} // 計分板 =>  {數字:出現次數}

function intersection(arr1=[],arr2=[]){
    let arr3 = arr1.concat(arr2)
    let result = []
    if(arr3.length == 0){
        return result;
    }
    
    for (let i = 0; i< arr3.length; i++ ){
        if(!counter[arr3[i]]){
            counter[arr3[i]] = 1
        }else{
            counter[arr3[i]] += 1
        }
    }
    
    for(let item in counter){
        if(counter[item]>=2){
            result.push(item)
        }
    }
    return result
}
let arrA = [1,2,3,4]
let arrB = [2,4,6,8]
intersection(arrA,arrB) 
```