# 資料結構與演算法 - palindrome
###### tags: `資料結構與演算法`,`Algorithms`,`Data Structure`

## 問題
創建一個函式，判斷一個字串是不是palindrome，意思是該字串正著寫跟反過來寫是一樣的。
例如：
```
tacocat -> 是
surprise -> 不是
```
## 思考
想像在摺紙，把字串對折去比較字母一樣與否。

## 實作
這樣做萬一剛好都是match的字母，迴圈就會跑n次。對折的想法應該能改善這種情況。
```javascript=
function isPalindrome(str){
    let result = true;
    const len = str.length
    for (let i = 0; i < len; i++) {
      if(str[i] !== str[len-1-i]){
        result=false
        break;
      }
    }
    return result
}

isPalindrome('tacocat')
isPalindrome('surprise')
```
### 其他寫法1
從中心點開始對折。往左右兩邊去比對字母是否一樣。
```javascript=
function isPalindrome(str){
    let result = true
    const len = str.length
    const isSingleNum = (len%2==1)
    const mid = isSingleNum? Math.floor(len/2):len/2
    let leftIdx= mid-1
    let rightIdx= isSingleNum?mid+1:mid
    // console.log('isSingleNum???',isSingleNum)
    while(leftIdx>=0 && rightIdx<len){
        if(str[leftIdx] !== str[rightIdx]){
            result = false
            break
        }
        leftIdx--
        rightIdx++
    }
    return result
}

isPalindrome('tacocat')
isPalindrome('surprise')
```
### 其他寫法2
不過，有需要像上方寫法去區分長度是奇數偶數嗎？抓頭抓尾兩兩一組呢？
```javascript=
function isPalindrome(str){
    let result = true
    let left = 0
    let right = str.length-1
    while(left<=right){
        if(str[left]!==str[right]){
            result = false
            break
        }else{
            left++
            right--
        }
    }
    return result
}

isPalindrome('tacocat')
isPalindrome('surprise')
```