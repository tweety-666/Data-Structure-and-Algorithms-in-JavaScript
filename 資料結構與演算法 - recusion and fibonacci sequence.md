# 資料結構與演算法 - recusion and fibonacci sequence
###### tags: `資料結構與演算法`,`Algorithms`,`Data Structure`

## 什麼是遞迴？
在自己函式內部執行自己。

在JS中，如果執行某函式內一個的函式，就是處於call stack。
call stack的資料結構是個堆疊(Stack)，這種資料結構具有後進先出(LIFO, Last in First out)的特性。而在每一次呼叫函式時，會把這個函式添加到堆疊的最上方，執行完後才將函式從上方抽離。

在數學當中，也有遞迴的運用。
例如：A(n)=A(n-1)+15

## 用程式描述遞迴數列
```javascript=
recusion(n){ // n必須為正整數
 if(n==1){
     return 10
 }else{
     return recusion(n-1)+15
 }
}
```

## 斐波那契數列
n必須為正整數:
F(0)=0
F(1)=1
F(n)=F(n-1)+F(n-2)

```javascript=
fs(n){ // n必須為正整數
 if(n==0){
     return 0
 }else if(n==1){
     return 0
 }else{
     return fs(n-1)+fs(n-2)
 }
}
```