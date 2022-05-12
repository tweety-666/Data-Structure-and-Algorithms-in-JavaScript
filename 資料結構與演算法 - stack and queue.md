# 資料結構與演算法 - stack and queue
###### tags: `資料結構與演算法`,`Algorithms`,`Data Structure`

## stack
stack像是個柱狀管子，可以放element進去，要拿element出來也只能從最上面開始拿。
### 特色
1. last in,first out.
2. element沒有index
3. 只能從最上方移除跟加入element

### 實作
```javascript=
class Queue {
  constructor(){
    this.queue = []
  }
  enqueue(value){
    this.queue.unshift(value)
  }
  dequeue(){
    return this.queue.pop()
  }
}

```

---

## queue
就是排隊。
### 特色
1. first in,first out.
2. 只能從最前方移除跟最後方加入element
3. element沒有index

### 實作
```javascript=
class Stack {
  constructor(){
    this.stack = [];
  }
  push(value){
    this.stack.push(value)
  }
  pop(){
   return this.stack.pop()
  }
}
```