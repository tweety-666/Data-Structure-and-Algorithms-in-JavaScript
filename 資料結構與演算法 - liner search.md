# 資料結構與演算法 - liner search
###### tags: `資料結構與演算法`,`Algorithms`,`Data Structure`

## 線性搜尋
線性搜尋是一個在序列中找尋目標的方法。基本上就是地毯式搜尋，一個個找尋目標。
看起來爆炸單純，但效能也不太好，尤其在input很大的時候特別明顯。要比喻的話，可以想成是王子挨家挨戶敲了整個村莊的門，去找腳剛好可以穿玻璃鞋的人。


## 實作
最單純的實作方式：迴圈
```javascript=
let numbers = [
  33,
  99,
  97,
  28,
  87,
  72,
  48,
  72,
  18,
  89,
  18,
  45,
  85,
  13,
  70,
  80,
  10,
  88,
  92,
  65,
  23,
  73,
  88,
  55,
  1,
  79,
  95,
  69,
  30,
  31,
  88,
  13,
  32,
  86,
  15,
  51,
  69,
  29,
  11,
  26,
  62,
  0,
  45,
  32,
  21,
  4,
  73,
  10,
  88,
  23,
  93,
  34,
  91,
  68,
  8,
  36,
  66,
  19,
  45,
  12,
  15,
  29,
  68,
  59,
  53,
  76,
  42,
  81,
  27,
  30,
  69,
  15,
  18,
  0,
  12,
  2,
  28,
  79,
  49,
  15,
  70,
  4,
  34,
  48,
  40,
  28,
  55,
  73,
  18,
  37,
  10,
  65,
  95,
  11,
  49,
  7,
  22,
  24,
  19,
  33,
];

// 從陣列中找到目標的的Index
function linearSearch(arr, n) {
  for (let i = 0; i < arr.length; i++) {
    if (arr[i] === n) {
      console.log("Found number " + n + " at index " + i);
      return i;
    }
  }
  console.log("Cannot find " + n);
  return -1;
}

linearSearch(numbers, 24);
```

## 該演算法的表現（performance)
* Worst performance  : 
    最糟的情況下，必須找n次。也就是王子敲門敲到最後一戶人家才找到灰姑娘。複雜度為O(n)。
* Best performance:
    最好的情況下，只花了1次，也就是王子第一次敲一戶人家的門就找到灰姑娘。真是天選之敲。複雜度為O(1)。
    
* Average performance：
    最佳跟最糟狀況的平均，複雜度為O(n/2)。
