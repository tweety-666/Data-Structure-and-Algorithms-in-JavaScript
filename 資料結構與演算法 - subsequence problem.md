# 資料結構與演算法 - subsequence problem
###### tags: `資料結構與演算法`,`Algorithms`,`Data Structure`

## 問題
subsequence，字串的子序列性，是指某個字串的組成，是透過另一個字串，拿掉一些字母（或是沒拿掉任何字母）而形成的。

創建一個函式有兩個參數都是字串，判斷兩個字串是否有「子序列性」，該兩個參數順序隨機。例如：`book`和`brooklyn`，同樣的字母有同樣的順序性。

## 思考
將兩個字串排隊排好，比較短的那個隊伍（簡稱隊伍B），取第一個字母去跟比較長的隊伍（簡稱隊伍A）比較是否有一樣的字母。如果比對到一樣的字母（位置簡稱為index），將該字從隊伍B刪除，並從隊伍B的下一個字母成員，從剛剛比對到的位置（index）接續比對。如果最後隊伍B還有成員，代表不符合子序列性。
如果兩個隊伍一樣長，直接比較是不是同個字。
但這樣寫，恐怕需要迴圈兩次。複雜度會為O(n^2)。

直接問短字串中每個字母，是否存在於長字串中，並且列出index。
1. index不能重複。
2. 短字串中每個字母的index，必須是由小到大的。
3. 如果有index為-1，代表該短字串字母不存在在長字串中。直接回傳為false。

上方的想法，會導致如果有同樣的字，會出現一樣的index。
必須把已經比對過且一致的字母，從該位置到陣列開頭都刪除掉。
例如：
`book`跟`brooklyn`
從`book`的`b`開始跟`brooklyn`比對，比對到`b`就拔掉。
`ook`的`o`，跟`rooklyn`比對，比對到`o`，把開頭到`o`（也就是`ro`）拿掉。
`ok`的`o`，跟`oklyn`比對，以此類推。
比對到最後`book`整個字串都比完了，有一致的就拔掉，變成空字串`''`

## 實作
```javascript=
function isSubsequence(str1,str2){
    const len1 = str1.length
    const len2 = str2.length
    // 一樣長度字串 直接比較是否一樣的字
    if(str1==''||str2=='') return true
    if(len1==len2){
        if(str1==str2) return true
    }else{
         let shortStr = (len1>len2)? str2:str1
         let count = 0
         let longStr = (len1>len2)? str1:str2
         for(let i=0;i<shortStr.length;i++){
             let latter = shortStr[i]
             console.log('要比較的latter',latter)
             // 該字 不存在於長字串中
             let idx = longStr.indexOf(latter)
             if( idx == -1){
                console.log('NO!!!')
                return false
             }else{
               // 該字 存在於長字串中
                longStr = longStr.slice(idx+1,longStr.length)
                count ++ // 成功劃掉一次
                console.log('比完了 長字串剩下',longStr)
             }
         }
      if(count == shortStr.length) return true
    }
	
    return false
}

isSubsequence('bookll','brooklyn') // false
isSubsequence('book','brooklyn') // true
isSubsequence('','hello brooklyn') // true
```

## 問題2
創建一個函式，判斷兩個字串，前者字串是否為後者字串的子序列。
例如：`book`和`brooklyn`，同樣的字母有同樣的順序性。

## 思考2
用兩兩比較的方式，比到的時候，在該字母上擺放「箭頭」。
例如：比較`book`跟`brooklyn`，箭頭指到第一個字母。
```
book
^
brooklyn
^
```
一樣的話，`book`跟`brooklyn`箭頭往後移動，`brooklyn`箭頭也往後移動，往後繼續比較。直到book的箭頭指到空空的地方。
比較到不一樣的話，只有`brooklyn`箭頭往後移動，
```
book
    ^
brooklyn
     ^
```

## 實作2
```javascript=
function isSubsequence(str1, str2) {
  if (str1.length == 0) {
    return true;
  }

  let pointer1 = 0;
  let pointer2 = 0;

  while (pointer2 < str2.length) {
    if (str1[pointer1] == str2[pointer2]) {
      pointer1++;
    }
    if (pointer1 >= str1.length) {
      return true;
    }
    pointer2++;
  }
    
  return false;
}

isSubsequence("hello", "hello Dear"); // true
isSubsequence("book", "brooklyn"); // true
isSubsequence("", "abc"); // true
```
