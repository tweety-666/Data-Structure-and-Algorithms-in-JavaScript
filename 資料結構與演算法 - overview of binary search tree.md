# 資料結構與演算法 - overview of binary search tree
###### tags: `資料結構與演算法`,`Algorithms`,`Data Structure`

## binary search tree
### 時間複雜度
* 最差：O(n)
    如果binary search tree剛好長成陣列的樣貌，也就是每個node都只有left child node，如果有n個node，那麼遍歷的時候，最差的情況下就得走n次。
* 最佳：O(1)
    如果binary search tree剛好只有一個或沒有node，或是一串binary search tree中第一個node就是要找的node。最佳情況下，只要找一次。
* 平均：O(logn)
    如果binary search tree是binary tree，也就是balanced的情況下，時間複雜度會因為sorting而呈現O(logn)。

### overview of code
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
    
    // 不同order
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
```



