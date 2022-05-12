# 資料結構與演算法 - binary search
###### tags: `資料結構與演算法`,`Algorithms`,`Data Structure`

## 二元搜尋
二元搜尋是在已經排序好的陣列中，找尋目標的方法。

有點類似猜數字遊戲，你向遊戲關主提出一個數字，關主會回答更高、更低或是正確，一來一往給予範圍，排除猜錯的另一半，直到找出答案。

每猜一次，搜尋範圍就縮小一半，因此稱為二元搜尋。不過只能在已經排序好的陣列中這麼做。

## 實作
```javascript=
let numbers = [
  9,
  12,
  15,
  18,
  19,
  20,
  22,
  25,
  26,
  26,
  33,
  37,
  38,
  41,
  47,
  47,
  50,
  55,
  57,
  60,
  68,
  80,
  87,
  90,
  98,
  100,
  103,
  108,
  109,
  109,
  116,
  120,
  120,
  124,
  127,
  128,
  131,
  135,
  135,
  139,
  143,
  145,
  151,
  155,
  156,
  158,
  163,
  164,
  165,
  169,
  169,
  173,
  174,
  176,
  177,
  178,
  181,
  182,
  182,
  183,
  184,
  189,
  192,
  195,
  200,
  201,
  203,
  204,
  207,
  213,
  217,
  222,
  222,
  222,
  227,
  228,
  233,
  235,
  237,
  239,
  239,
  243,
  248,
  251,
  252,
  257,
  260,
  260,
  263,
  268,
  270,
  271,
  271,
  276,
  281,
  284,
  285,
  295,
  297,
  298,
];

function binarySearch(arr, n) {
  let minIndex = 0;
  let maxIndex = arr.length - 1;

  while (min <= max) {
    let middleIndex = Math.floor((maxIndex + minIndex) / 2);
    if (n > arr[middleIndex]) {
      minIndex = middleIndex + 1;
    } else if (n < arr[middleIndex]) {
      maxIndex = middleIndex - 1;
    } else if (n === arr[middleIndex]) {
      return middleIndex;
    }
  }
  return -1;
}

binarySearch(numbers, 213);
```

## 該演算法的表現（performance)
* Worst performance: O(logn)
* Best performance: O(1)。
* Average performance：O(logn)