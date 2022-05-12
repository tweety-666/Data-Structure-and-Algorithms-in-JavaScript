# 資料結構與演算法 - DFT in graph
###### tags: `資料結構與演算法`,`Algorithms`,`Data Structure`

## graph
圖形（graph）是由多個節點(node)與連接節點的邊(edge)所組成，跟tree相比，graph中是沒有root的，也沒有parent and child的關係。tree是graph的一種。
![](https://i.imgur.com/WbdCJhK.png)

## 深度優先
從graph的起始node開始，從該node開始訪問，往鄰居節點繼續訪問，每次訪問都會盡量挑距離最遠的節點。

## 思考
用上圖舉例，假設從初始節點A開始，用loop去訪問還沒訪問過的鄰居節點E、F，此時在迴圈內，先訪問到E，又用loop去訪問E的還沒訪問過的鄰居節點D、F、I，先訪問到D，又用loop去訪問E的還沒訪問過的鄰居節點B、C，先訪問到B，已經沒有鄰居節點可以訪問了，該次loop結束，訪問順序為：`A E D B`，回到上一個loop，也就是D訪問鄰居的loop，下一個是C，以此類推。
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

function DFT(starter) {
  starter.visited = true;
  result.push(starter.value);
  starter.neighbors.forEach((neighbor) => {
    if (!neighbor.visited) {
      DFT(neighbor);
    }
  });
  return result;
}
```