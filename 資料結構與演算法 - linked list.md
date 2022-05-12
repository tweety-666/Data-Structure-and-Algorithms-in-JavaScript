# 資料結構與演算法 - linked list
###### tags: `資料結構與演算法`,`Algorithms`,`Data Structure`

## linked list
1. 有順序性的線性集合，順序性不是指在記憶體位置上是連續的，而是每個元素會指向下一個，尾端會指向null。
3. linked list有property: head 跟 length。就像是個火車。
4. linked list內的每個node，像是火車車廂，有自己的value，且指向下一個或是null。

## 優勢
1. 元素可以無限制地被安插到linked list，如果是array就不行，因為array會被填滿。
2. insert或delete的效率都比array來得好。因為array每個元素有index，linked list只要改變指向就好。

## 缺點
1. 比array佔用更多記憶體空間，因為會用到pointer（指向的property）。
2. 一定要從head開始遍歷才能取到特定的node。不像array可以用index找到元素。
3. 在電腦記憶體內的儲存並不是連續性的，所以要跳到指定儲存位置需要比較久，但array是連續性的。
4. 要倒過來遍歷所有元素是很難的，因為只能從head開始遍歷。


## 實作
```javascript=
class Node {
  constructor(value) {
    this.value = value;
    this.next = null;
  }
}

class LinkedList {
  constructor() {
    this.head = null;
    this.length = 0;
  }

  push(value) {
    let newNode = new Node(value);
    if (this.head === null) {
      this.head = newNode;
    } else {
      let currentNode = this.head;
      while (currentNode.next !== null) {
        currentNode = currentNode.next;
      }
      currentNode.next = newNode;
    }
    this.length++;
  }

  pop() {
    if (!this.head) {
      return null;
    } else if (this.length === 1) {
      let temp = this.head;
      this.head = null;
      this.length = 0;
      return temp;
    } else {
      let currentNode = this.head;
      for (let i = 1; i <= this.length - 2; i++) {
        currentNode = currentNode.next;
      }
      let temp = currentNode.next;
      currentNode.next = null;
      this.length--;
      return temp;
    }
  }

  shift() {
    if (!this.head) {
      return null;
    } else if (this.length === 1) {
      let temp = this.head;
      this.head = null;
      this.length = 0;
      return temp;
    } else {
      let temp = this.head;
      this.head = this.head.next;
      this.length--;
      return temp;
    }
  }

  unshift(value) {
    if (!this.head) {
      this.head = new Node(value);
    } else {
      let temp = this.head;
      let newNode = new Node(value);
      this.head = newNode;
      this.head.next = temp;
    }
    this.length++;
  }

  insertAt(index, value) {
    if (index > this.length || index < 0) {
      return null;
    } else if (index === 0) {
      this.unshift(value);
      return;
    } else if (index === this.length) {
      this.push(value);
      return;
    }

    let currentNode = this.head;
    let newNode = new Node(value);
    // for loop index - 1 steps
    for (let i = 1; i <= index - 1; i++) {
      currentNode = currentNode.next;
    }
    newNode.next = currentNode.next;
    currentNode.next = newNode;
    this.length++;
    return;
  }

  removeAt(index) {
    if (index > this.length || index < 0) {
      return null;
    } else if (index === 0) {
      let result = this.shift();
      return result;
    } else if (index === this.length) {
      let result = this.pop();
      return result;
    }

    let currentNode = this.head;
    for (let i = 1; i <= index - 1; i++) {
      currentNode = currentNode.next;
    }
    let temp = currentNode.next;
    currentNode.next = currentNode.next.next;
    this.length--;
    return temp;
  }

  get(index) {
    if (index >= this.length || index < 0) {
      return null;
    }
    let currentNode = this.head;
    for (let i = 0; i < index; i++) {
      currentNode = currentNode.next;
    }
    return currentNode.value;
  }
    
  printAll() {
    if (this.length === 0) {
      console.log("Nothing in this linked list.");
      return;
    }
    let currentNode = this.head;
    while (currentNode !== null) {
      console.log(currentNode.value);
      currentNode = currentNode.next;
    }
  }
}

let myLinkedList = new LinkedList();
console.log(myLinkedList.length);
```
## singly and doubly linked list
1. singly
    像火車一樣，有個head，每個node就是車廂，指向next，最後一個node指向null。
2. doubly
    除了head還有tail。每個node除了有next以外，還多了before，指向前一個node。

## doubly linked list 的優劣
* 優點
    比較好找到指定的節點，效率比singly linked list好一倍。因為可以從tail開始找node，最多走一半。但singly linked list只能從head開始找。
* 缺點
    因為node多了一個pointer叫做before，記憶體就會消耗更多去紀錄這些pointer。


