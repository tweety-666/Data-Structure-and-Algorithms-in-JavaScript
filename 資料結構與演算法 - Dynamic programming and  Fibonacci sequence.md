# 資料結構與演算法 - Dynamic programming and  Fibonacci sequence
###### tags: `資料結構與演算法`,`Algorithms`,`Data Structure`

## 動態規劃(Dynamic programming)
dynamic programming是divide and conquer的延伸。當遞迴分割出來的子問題(sub-problems)，一而再、再而三出現，就用記憶法儲存這些問題的答案，避免重複求解，以空間換取時間。

跟greedy method相比，比較全面。greedy method只考量當下的狀況做決定。也因為比較全面，可以解決子問題，也不會重工。

## 重疊子問題(Overlapping Subproblem)
當一個大問題能被分解成小問題，而小問題再分解下去的小問題有重複的情況時，表示這個問題有重疊子問題(Overlapping Subproblem)。例如：費氏數列。
那該怎麼處理重疊子問題？

## Top-down and Bottom-up
*  Top-down:
    它會把一個大問題拆分成許多小問題，通常搭配記憶法，把處理過的小問題記憶起來。
*  Bottom-up:
    從小問題開始解決，直到解決大問題，通常使用製表法，把小問題的值放入表格，要計算大問題的值，就把前面已計算過的小問題拿來用。

## 費氏數列(Fibonacci sequence)
### 遞迴(Recursion
費氏數列：
```
F(0)=0
F(1)=1
F(n)=F(n-1)+F(n-2)
```
用Recursion的方式的確可以運算出來，但非常沒有效率，子問題會一直被計算，好比說計算到`F(5)=F(4)+F(3)`，就要再去計算`F(4)=F(3)+F(2)`跟`F(3)=F(2)+F(1)`，該時間複雜度為O(2^n)。

### 記憶法(Memoization)
把計算過的數值，先記憶起來，利用空間換取時間。時間複雜度減少為O(n)。
```javascript=
function fibonacciCalculator(cache, n) {
    switch (n) {
        case 0:
            return 0;
        case 1:
            return 1;
        default:
            if (!cache[n]) {
                cache[n] =fibonacciCalculator(cache, n - 1) +fibonacciCalculator(cache, n - 2);
            }
 
            return cache[n];
    }
}
 
function fibonacci(n) {
    const cache = new Array(n + 1).fill(null); //當作倉庫 存放已經算過的數值
    return fibonacciCalculator(cache, n);
}

```

## 製表法(Tabulation)
把小問題的值放入表格，要計算大問題的值，就把前面已計算過的小問題拿來用。時間複雜度減少為O(n)。
```javascript=
function fibonacci(n) {
    const cache = Array(n + 1);
 
    cache[0] = 0;
    cache[1] = 1;
 
    for (let i = 2;i <= n;i++) {
        cache[i] = cache[i - 1] + cache[i - 2];
    }
 
    return cache[n];
}
```