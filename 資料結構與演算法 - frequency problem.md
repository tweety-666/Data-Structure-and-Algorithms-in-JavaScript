# 資料結構與演算法 - frequency problem
###### tags: `資料結構與演算法`,`Algorithms`,`Data Structure`

## 問題
找出兩個字串中，每個字母是不是有一樣的出現頻率，結果只能為「是」跟「不是」。
例如：
1. abba 跟 abab => 是
2. abbc 跟 acab => 不是

## 思考
使用計分板，把每個數字出現的次數記著，然後去比較是不是有一樣的字母，有不一樣的字母直接顯示結果為「不是」，字母都一樣的話，就去比對字母出現的次數，頻率都一樣才能回傳「是」。
但如果先排序字母，從a到z，然後比較兩個字母是不是完全一樣，比較快也比較簡單。

## 實作
```javascript=
function sameLatterFrequency (str1,str2){
    const strFisrt = Array.from(str1).sort().join('')
    const strSecond = Array.from(str2).sort().join('')
    
    if(strFisrt === strSecond){
        return true
    }
    return false
}

sameLatterFrequency('abab','bbaa') // true
sameLatterFrequency('abac','bbcc') // false
sameLatterFrequency('ababc','bbaa') // false
```