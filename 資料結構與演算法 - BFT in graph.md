# 資料結構與演算法 - BFT in graph
###### tags: `資料結構與演算法`,`Algorithms`,`Data Structure`

## graph
圖形（graph）是由多個節點(node)與連接節點的邊(edge)所組成，跟tree相比，graph中是沒有root的，也沒有parent and child的關係。tree是graph的一種。
![](https://i.imgur.com/WbdCJhK.png)

## 廣度優先
從graph的起始node開始，從該node開始訪問，往鄰居節點繼續訪問，該節點的鄰居都問完了，才訪問鄰居的鄰居節點。

## 思考

## 實作
```javascript=
 class Node {
  constructor(value) {
    this.value = value;
    this.neighbors = [];
    this.visited = false;
  }

  addNeighbor(n) {
    this.neighbors.push(n);
  }
}

let A = new Node("A");
let B = new Node("B");
let C = new Node("C");
let D = new Node("D");
let E = new Node("E");
let F = new Node("F");
let G = new Node("G");
let H = new Node("H");
let I = new Node("I");
let J = new Node("J");
let K = new Node("K");
let L = new Node("L");
let M = new Node("M");

A.addNeighbor(E);
A.addNeighbor(F);
B.addNeighbor(D);
C.addNeighbor(D);
D.addNeighbor(B);
D.addNeighbor(C);
D.addNeighbor(E);
D.addNeighbor(I);
E.addNeighbor(A);
E.addNeighbor(D);
E.addNeighbor(F);
E.addNeighbor(I);
F.addNeighbor(A);
F.addNeighbor(E);
F.addNeighbor(I);
G.addNeighbor(H);
G.addNeighbor(I);
H.addNeighbor(G);
H.addNeighbor(I);
H.addNeighbor(L);
I.addNeighbor(D);
I.addNeighbor(E);
I.addNeighbor(F);
I.addNeighbor(G);
I.addNeighbor(H);
I.addNeighbor(J);
I.addNeighbor(K);
I.addNeighbor(M);
J.addNeighbor(I);
J.addNeighbor(M);
K.addNeighbor(I);
K.addNeighbor(L);
K.addNeighbor(M);
L.addNeighbor(H);
L.addNeighbor(K);
M.addNeighbor(I);
M.addNeighbor(J);
M.addNeighbor(K);

// ---以上是建立graph的關係---

let result = [];

function BFT(starter) {
  let queue = []; // 用來確認該節點是不是訪問過的地方 當作是檢查的隊伍
  queue.push(starter); 
    // 有節點在排隊 就檢查
  while (queue.length != 0) {
    let firstNode = queue.shift(); // 第一個節點 先檢查是否訪問過
    if (!firstNode.visited) {
      firstNode.visited = true;
      result.push(firstNode.value); // 確定訪問過 就放到回傳結果的隊伍
      // 訪問該節點後 詢問他的所有鄰居
      firstNode.neighbors.forEach((neighbor) => {
        if (!neighbor.visited) {
          queue.push(neighbor);
        }
      });
    }
  }
  return result;
}

console.log(BFT(A));
```