# 資料結構與演算法 - unique letter string
###### tags: `資料結構與演算法`,`Algorithms`,`Data Structure`

## 問題
寫一個函式，參數為字串，會回傳所有字母都不同且長度最長的子字串。
例如：`uniqueLetterString('thisishowwedoit')
`，會回傳`wedoit`。

## 思考
使用slide window的概念，利用迴圈去一個個元素。
```
// 從頭開始跑 記住't'
thisishowwedoit
^
^

// 從頭開始跑 記住't' 新元素'h'不和't'重複 記住'th' 繼續移動end箭頭 以此類推
thisishowwedoit
^
 ^
```
```
// 從頭開始跑 當前記得'this' 繼續移動end箭頭到下個新元素i 重複了
thisishowwedoit
^
    ^
// 於是移動start箭頭 以此類推 直到當前值沒有和新元素重複
thisishowwedoit
 ^
    ^
```

## 實作
```javascript=
function uniqueLetterString(str){
    let start = 0
    let end = 0
    let currentWord = '' // 當前紀錄的字串
    let maxLenWord = '' // 紀錄最長的字串
    const strLen = str.length
    
    while(start<strLen && end<strLen){
        let newWord = str[end]
        console.log('start',start,'/ end',end)
      	if(currentWord.indexOf(newWord) !== -1){
            console.log('重複了 currentWord=>',currentWord,'newWord',newWord)
          	if(currentWord.length>maxLenWord.length){
            	maxLenWord = currentWord
              	console.log('maxLenWord',maxLenWord)
            }
			currentWord= currentWord.slice(1,currentWord.length)
          	start++
        }else{
          	currentWord+=newWord
			console.log('currentWord',currentWord)
          	end++
        }
    }
    return maxLenWord
}

uniqueLetterString('thisishowwedoit') // wedoit
```
## 優化
上述的做法算到`thisi`重複了`i`的時候，移動start的箭頭，下次運算為`hisi`，這樣依然是重複的。一旦有重複的字母出現，start的箭頭就該移動到重複字母的下一位開始計算，例如：

```
// 從頭開始跑 當前記得'this' 繼續移動end箭頭到下個新元素i 重複了
thisishowwedoit
^
    ^
// 於是移動start箭頭 得出字串'hisi'
thisishowwedoit
 ^
    ^
    
// 改為移動start箭頭到s（重複的i的下一個位置）
thisishowwedoit
   ^
    ^

// 如果題目改為
thisssshowwedoit
^
^
// 算到s重複了
thisssshowwedoit
^
    ^

// 也是從重複的index+1 開始繼續算
thisssshowwedoit
    ^
    ^
    
// 如果題目改為 也是從重複的index+1 開始繼續算
thisabcdeshowwedoit
    ^
         ^
```

```javascript=
    function uniqueLetterString(str){
    let start = 0
    let end = 0
    let currentWord = '' // 當前紀錄的字串
    let maxLenWord = '' // 紀錄最長的字串
    const strLen = str.length
    
    while(start<strLen && end<strLen){
        let newWord = str[end]
        console.log('start',start,'/ end',end)
      	if(currentWord.indexOf(newWord) !== -1){
            console.log('重複了 currentWord=>',currentWord,'newWord',newWord)
          	if(currentWord.length>maxLenWord.length){
            	maxLenWord = currentWord
              	console.log('maxLenWord',maxLenWord)
            }
          	start=str.indexOf(newWord)+1
          	currentWord= currentWord.slice(start,end+1)
          	console.log('新的',currentWord)
        }else{
          	currentWord+=newWord
			console.log('currentWord',currentWord)
          	end++
        }
    }
    return maxLenWord
}

uniqueLetterString('thisaishowwedoit') // wedoit

```