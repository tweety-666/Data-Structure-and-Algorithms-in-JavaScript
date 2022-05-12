# 資料結構與演算法 - priority queue and max heap insertion
###### tags: `資料結構與演算法`,`Algorithms`,`Data Structure`

## priority queue
priority queue內的每個元素有個叫做priority的屬性，該數值代表優先程度。越大的數值越優先。

但這只是個概念，實際上該用那個資料格式來呈現？
1. linked list
2. queue
3. array
4. max heap

答案是：max heap。max heap是parent node數值大於child node的資料結構，node的數值剛好可以代表priority的值。

## 時間複雜度比較
* max heap：
    enqueue: O(logn)
    dequeue: O(logn)
* array
    enqueue: O(n)
    dequeue: O(n)
* linked list
    enqueue: O(n)
    dequeue: O(1)

## max heap insertion
在max heap內放入一個新node，它具有priority，那麼該node應該放在max heap的哪裡呢？

### max heap 的節點關係
對parent node來說，下面兩個child node，他們之間index的關係：
```
x => 2x+1 and 2x+2
```
對child node來說，parent node的index為：
```
x => Math.floor((x-1)/2)
```

### 實作
```javascript=
class Node {
  constructor(value, priority) {
    this.value = value;
    this.priority = priority;
  }
}

class PriorityQueue {
  constructor() {
    this.list = [];
  }

  // 新節點加入queue
  enqueue(value, priority) {
    let newNode = new Node(value, priority);
    // 空的 直接加進去
    if (this.list.length === 0) {
      this.list.push(newNode);
      return true;
    }

    this.list.push(newNode);
    let newIndex = this.list.length - 1; // 新加入的節點的位置 在最後面
    let parentIndex = Math.floor((newIndex - 1) / 2); // 找出新加入節點的parent node位置 去進行比大小
    while (
      parentIndex >= 0 &&
      this.list[newIndex].priority > this.list[parentIndex].priority
    ) {
      // 比parent node大 就要交換
      let result = this.list[parentIndex];
      this.list[parentIndex] = this.list[newIndex];
      this.list[newIndex] = result;
      // 更新新節點位置 持續比較
      newIndex = parentIndex;
      parentIndex = Math.floor((newIndex - 1) / 2);
    }
  }

}
```

## max heap deletion
把第一個node從max heap中刪除，重新排序。

### 實作
```javascript=
class Node {
  constructor(value, priority) {
    this.value = value;
    this.priority = priority;
  }
}

class PriorityQueue {
  constructor() {
    this.list = [];
  }

  // 新節點加入queue
  enqueue(value, priority) {
    let newNode = new Node(value, priority);
    // 空的 直接加進去
    if (this.list.length === 0) {
      this.list.push(newNode);
      return true;
    }

    this.list.push(newNode);
    let newIndex = this.list.length - 1; // 新加入的節點的位置 在最後面
    let parentIndex = Math.floor((newIndex - 1) / 2); // 找出新加入節點的parent node位置 去進行比大小
    while (
      parentIndex >= 0 &&
      this.list[newIndex].priority > this.list[parentIndex].priority
    ) {
      // 比parent node大 就要交換
      let result = this.list[parentIndex];
      this.list[parentIndex] = this.list[newIndex];
      this.list[newIndex] = result;
      // 更新新節點位置 持續比較
      newIndex = parentIndex;
      parentIndex = Math.floor((newIndex - 1) / 2);
    }
  }
    
  // 從max heap刪除首個節點
  dequeue() {
    if (this.list.length === 0) {
      return null;
    }
    if (this.list.length === 1) {
      let removedNode = this.list.pop();
      return removedNode;
    }
    // 把第一個跟最後一個node互換
    let temp = this.list.pop();
    this.list.push(this.list[0]);  
    this.list[0] = temp;
    let removedNode = this.list.pop();
    this.maxHeapify(0); // 要重新排序
    return removedNode;
  }
  maxHeapify(i){
    let largest; // 最大值的index
    let l = i * 2 + 1; // 左邊子節點index
    let r = i * 2 + 2; // 右邊子節點index
      
     // 先跟左邊比
    if (
      l <= this.list.length - 1 &&
      this.list[l].priority > this.list[i].priority
    ) {
      largest = l;
    } else {
      largest = i;
    }
    
      // 再跟右邊比
    if (
      r <= this.list.length - 1 &&
      this.list[r].priority > this.list[largest].priority
    ) {
      largest = r;
    }
    
     // 最大值的王座要換人坐
    if (largest != i) {
      let temp = this.list[i];
      this.list[i] = this.list[largest];
      this.list[largest] = temp;
      this.maxHeapify(largest); // 交換後 持續進行比較
    }
  }
}

let vipQueue = new PriorityQueue();
vipQueue.enqueue("tom", 5);
vipQueue.enqueue("amy", 2);
vipQueue.enqueue("ruby", 7);
vipQueue.enqueue("joe", 8);
vipQueue.enqueue("jake", 12);
vipQueue.enqueue("bob", 15);

// 先進先出
while (vipQueue.list.length >= 1) {
  let task = vipQueue.dequeue();
  console.log(task.value + ", " + task.priority);
}
```
