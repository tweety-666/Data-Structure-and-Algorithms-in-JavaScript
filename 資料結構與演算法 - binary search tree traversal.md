# 資料結構與演算法 - binary search tree traversal
###### tags: `資料結構與演算法`,`Algorithms`,`Data Structure`

## Binary Tree
一個root，每個node下面只有最多兩個node。

## Binary Search Tree
一個root，每個node下面只有最多兩個node。大小順序為：`left child node < node < right child node`。

## Breadth-first Tree Traversal
從root出發，以水平方向由左到右訪問，將同detph的兄弟node訪問完之後，再訪問下一層的node。


## Depth-first Tree Traversal
從root出發，選擇某一subtree並以垂直方向從上到下訪問，將child node訪問完後，再選擇另一subtree重複該過程。

## Order
* in-order：由小到大依序迭代
* pre-order：由上往下，由左至右
* post-order：由下往上，由左至右


### 實作
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
        this.path = ''; // 紀錄遍歷到tree的哪裡
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
    
    preOrder(node) {
        if (node != null) {
          this.path += node.value + " ";
          this.preOrder(node.left);
          this.preOrder(node.right);
        }
    }

    inOrder(node) {
        if (node != null) {
          this.inOrder(node.left);
          this.path += node.value + " ";
          this.inOrder(node.right);
        }
    }

    postOrder(node) {
        if (node != null) {
          this.postOrder(node.left);
          this.postOrder(node.right);
          this.path += node.value + " ";
        }
    }
    
    // 廣度優先
    breadthFirstTreeTraversal(node){
        let list =[]
        if (node != null) {
            list.push(node);
            for (let i = 0; i < list.length; i++) {
                let currentNode = list[i];
                if (currentNode != null) {
                    if (currentNode.left != null) {
                        list.push(currentNode.left);
                      }
                    if (currentNode.right != null) {
                        list.push(currentNode.right);
                    }
                }
            }
        }
        console.log('廣度優先',list)
        return list;
    }
    
    // 深度優先
    depthFirstTreeTraversal(node){
        let list = [];
        function traverse(node) {
            list.push(node);
            if (node.left) traverse(node.left);
            if (node.right) traverse(node.right);
        }
        traverse(node)
        console.log('深度優先',list)
        return list;
    }
}

const myBST = new BinarySearchTree();
myBST.insert(15)
myBST.insert(6)
myBST.insert(5)
myBST.insert(1)
myBST.insert(13)
myBST.insert(-7)
myBST.insert(3)

myBST.preOrder(myBST.root)
console.log(myBST.path) // 15 6 5 1 -7 3 13 

myBST.inOrder(myBST.root)
console.log(myBST.path) // -7 1 3 5 6 13 15

myBST.postOrder(myBST.root)
console.log(myBST.path) // -7 3 1 5 13 6 15 

myBST.breadthFirstTreeTraversal(myBST.root)

myBST.depthFirstTreeTraversal(myBST.root)
```