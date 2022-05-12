# 資料結構與演算法 - search in binary search tree
###### tags: `資料結構與演算法`,`Algorithms`,`Data Structure`

## recursively search
先從一個節點開始尋找，沒找到就再次提供該節點的左子節點跟右子節點，透過遞迴的方式繼續找。

## interatively search
利用`while`的特性，沒有找到該節點就繼續找，直到比較的節點是null（代表沒有拿來比較大小的節點了）就停止。


## 實作
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
    
    // 遞迴尋找節點
    searchRecursively(x, value) {
        if (x == null || value == x.value) {
          return x;
        } else if (value < x.value) {
          return this.searchRecursively(x.left, value);
        } else {
          return this.searchRecursively(x.right, value);
        }
    }
    
    // 持續尋找節點
    searchIteratively(x, value) {
        while (x !== null && value !== x.value) {
          if (value < x.value) {
            x = x.left;
          } else {
            x = x.right;
          }
        }

        if (x == null) {
          console.log("Node not found");
        } else {
          console.log("Node found.");
        }
        return x;
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

let result1 = myBST.searchRecursively(myBST.root,3)
let result2 = myBST.searchIteratively(myBST.root,-1) // null

console.log(result1,result2)
```



