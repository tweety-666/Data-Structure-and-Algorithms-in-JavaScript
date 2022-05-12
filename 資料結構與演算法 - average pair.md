# 資料結構與演算法 - average pair
###### tags: `資料結構與演算法`,`Algorithms`,`Data Structure`

## 問題
在一串數字陣列中，算出該陣列的平均值（簡稱為A），並找出所有兩兩一組的數字，該組數字的平均必須為A。
```javascript=
let arrA = [1,2,3,4,5]
let arrB =  [0,1,2,3,4,5]
function averagePair(arr){
    //...思考
}

averagePair(arrA) // 平均為3 組合有[1,5]、[2,4]
averagePair(arrB) // 平均為3 組合有[0,3]、[1,5]、[2,4]
```

## 思考
先算出平均，然後針對該陣列跑一次迴圈，問每個數字如果需要達到平均，你要跟誰組隊，假設平均為4，問到數字0，知道該數字必須跟數字4組隊，去問該陣列有沒有數字4存在，沒有的話就跳過，如果剛好有數字4，那後面數字4也不用問了，可以被跳過，把0跟4都從該陣列中拿出來組隊。

## 實作
複雜度為O(n)
```javascript=
function getAverage(arr){
    const reducer = (previousValue, currentValue) => previousValue + currentValue;
    return arr.reduce(reducer)/arr.length

}
function getPairNum(average,num){
    return average*2-num
}

function averagePair(arr){
    let copyArr = arr
    let result = []
    const average = getAverage(arr)
    console.log('average is',average)
    for(let i =0; i< copyArr.length; i++){
      	let item = copyArr[i];
        let pairNum = getPairNum(average, item)
        if( copyArr.indexOf(pairNum) !== -1 && copyArr.indexOf(pairNum)!== i){
            let team =[]
            team.push(item)
            team.push(pairNum)
          	result.push(team)
            copyArr.splice(copyArr.indexOf(pairNum), 1);
        }
    }
  	console.log('result is ',result)
    return result;
    
}

let arrA = [1,2,3,4,5]
let arrB =  [0,1,2,3,4,5]
averagePair(arrA) // 平均為3 組合有[1,5]、[2,4]
averagePair(arrB) // 平均為2.5 組合有[0,5]、[1,4]、[2,3]

```

## 問題2
創建出一個函式，第一個參數為一串已經排序好大小的數字陣列，第二個參數為一個數字。請讓該函式可以找出參數陣列中兩兩一組平均起來為第二個參數的組合。
```javascript=
function averagePair(arr,average){
    //...思考
}

averagePair([-11,0,2,3,9,14,17,21],1.5)
```

## 思考2
先求出要達成平均的隊伍的總和（簡稱A）。
跑一次迴圈，問第一個參數陣列中，每個數字為了要達成A，必須跟誰組隊。該數字不存在就跳過。存在就先從陣列中排除並加入隊伍。

## 實作2
複雜度為O(n)
```javascript=
function getPairNum(average,num){
    return average*2-num
}

function averagePair(arr,average){
    const averageTotal = average * 2
    let result = []
    for(let i =0; i< arr.length; i++){
      	let item = arr[i];
        let pairNum = getPairNum(average, item)
        if( arr.indexOf(pairNum) !== -1 && arr.indexOf(pairNum)!== i){
            let team =[]
            team.push(item,pairNum)
            // team.push(pairNum)
          	result.push(team)
            arr.splice(arr.indexOf(pairNum), 1);
        }
    }
  	console.log('result is ',result)
    return result;
    
}
averagePair([-11,0,2,3,9,14,17,21],1.5)
// 指定的平均為：1.5
// 組合為：[-11,14]、[0,3]
```
