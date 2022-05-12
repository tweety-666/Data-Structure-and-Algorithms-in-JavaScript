# 資料結構與演算法 - insertion in binary search tree
###### tags: `資料結構與演算法`,`Algorithms`,`Data Structure`

## Graph概略
Graph是由一串node（也稱point或vertex）的有限集合，node之間也許有line（也稱link或edge）將node連在一起。如果line有方向性箭頭，稱為directed graph。

![](https://i.imgur.com/AWBamPo.png)
![](https://i.imgur.com/FATaPqq.png)

## Tree
1. Tree是一種graph，只是沒有loop。
2. Tree必定有一個唯一的root。

## Binary Tree
一個root，每個node下面只有最多兩個node。

## Binary Search Tree
一個root，每個node下面只有最多兩個node。大小順序為：`left child node < node < right child node`。

### insertion in BST

```javascript=
class Node {
  constructor(value) {
    this.value = value;
    this.left = null;
    this.right = null;
  }
}

class BinarySearchTree {
    constructor() {
        this.root = null;
    }

    // 插入node
    insert(value){
        let newNode = new Node(value);
        //無root
        if (this.root === null) {
            this.root = newNode;
            return this;
        }
        //有root 判斷大小 決定往左還是往右
        let current = this.root;
        while(true){
            if (value < current.value) {
                if (current.left === null) {
                    current.left=newNode
                    return this;
                }
                current = current.left
            }
        
            else if(value > current.value) {
                if (current.right === null) {
                    current.right=newNode
                    return this;
                }
                current = current.right
            }
            //插入重複節點
            else {
                console.log("插入重複節點");
                return false;
            }
        }    
    }
}
```