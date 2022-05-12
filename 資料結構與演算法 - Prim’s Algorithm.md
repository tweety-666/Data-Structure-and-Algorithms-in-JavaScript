# 資料結構與演算法 - Prim's Algorithm
###### tags: `資料結構與演算法`,`Algorithms`,`Data Structure`

## minimal spainning tree
一個有loop的graph，消去一個以上的edge（edge具有值），使其成為tree（沒有loop）。這樣留下來的tree就是spainning tree。每個node都要算進去，不可以被消去。

一個有loop的graph，可能有著很多spainning tree；minimal spainning tree是指該tree的edge所具有的值，加總起來最小。

## Prim's Algorithm
### 步驟
1. 從任何一個node開始訪問，當作初始node。
2. 訪問過的node，用visited標記為已經訪問過。
3. 從初始node開始計算所有與該node相連的node的edge值。
4. 選擇值最小的那個edge。
5. 重複上述操作，直到所有node都被訪問過。

![](https://i.imgur.com/aVOxW95.png)
假設從上圖節點A開始，將節點A改為已訪問過，節點A有邊10跟8可以選，選擇比較小的8，走到節點C。
將節點C改為已訪問過，節點C有著邊5跟8可以選，走5，訪問節點D。
將節點D改為已訪問過，節點D有著邊3、4跟6，走3來到節點F。
將節點F改為已訪問過，節點F有邊1、3、8可以選，但只能走還沒訪問的節點邊，走1，訪問到節點E。
將節點E改為已訪問過，因為只能走還沒訪問的節點邊，走7，訪問到節點B。
全部節點都訪問過了。

也就是說，每次決定要訪問下家的時候，要分成兩個集合：「沒有被訪問過的節點」，跟「被訪問過的節點」，將兩個集合有邊的話，進行連線。

### 實作
```javascript=
// 節點
class Node {
  constructor(label) {
    this.label = label;
    this.visited = false;
    this.edges = [];
  }

  addEdge(edge) {
    this.edges.push(edge);
  }
}

let A = new Node("A");
let B = new Node("B");
let C = new Node("C");
let D = new Node("D");
let E = new Node("E");
let F = new Node("F");
let allNodes = [A, B, C, D, E, F];

// 邊
class Edge {
  constructor(node1, node2, distance) {
    this.node1 = node1;
    this.node2 = node2;
    this.distance = distance;
  }
}

// 先把graph的節點跟邊實例建立出來
let e1 = new Edge(A, B, 10);
A.addEdge(e1);
B.addEdge(e1);
let e2 = new Edge(A, C, 8);
A.addEdge(e2);
C.addEdge(e2);
let e3 = new Edge(B, D, 6);
B.addEdge(e3);
D.addEdge(e3);
let e4 = new Edge(C, D, 5);
C.addEdge(e4);
D.addEdge(e4);
let e5 = new Edge(B, E, 7);
B.addEdge(e5);
E.addEdge(e5);
let e6 = new Edge(D, E, 4);
D.addEdge(e6);
E.addEdge(e6);
let e7 = new Edge(D, F, 3);
D.addEdge(e7);
F.addEdge(e7);
let e8 = new Edge(E, F, 1);
E.addEdge(e8);
F.addEdge(e8);
let e9 = new Edge(C, F, 8);
C.addEdge(e9);
F.addEdge(e9);

//-------以下是求出minimal spainning tree-------

let tempBucket = [] // 要被運算的邊 都先放在這裡

function getMSTByPrim(startNode) {
  let mstEdges = []; // 紀錄形成mst的邊
    
  // 把初始節點的邊都加進來
  for (let i = 0; i < startNode.edges.length; i++) {
    tempBucket.push(startNode.edges[i]);
  }

  let bestEdge = getBestEdge(); // 求出最小且節點沒有都被訪問過的邊

  while (bestEdge != null) {
    let n1 = bestEdge.node1;
    let n2 = bestEdge.node2;
    n1.visited = true;
    n2.visited = true;
    mstEdges.push(bestEdge); // 把邊記錄起來 節點也記錄成被訪問過

    tempBucket = []; // reset要被運算的邊 因為要重新比較邊的大小
    
    // 有拜訪過的節點 如果它的邊還沒被放去比較 要把這些邊放進去比較 就一直比較直到沒得比
    allNodes.forEach((node) => {
      if (node.visited) {
        node.edges.forEach((edge) => {
          if (!mstEdges.includes(edge)) {
            tempBucket.push(edge);
          }
        });
      }
    });

    bestEdge = getBestEdge();
  }

  return mstEdges;
}

// 求出最小且節點沒有都被訪問過的邊
function getBestEdge() {
  let bestEdge = null;
  while (bestEdge == null && tempBucket.length != 0) {
    // 假設第一個邊是最佳
    bestEdge = tempBucket[0];
    let index = 0;
    // 開始比較誰是真正最小的邊 找出該index
    tempBucket.forEach((edge, i) => {
      if (edge.distance < bestEdge.distance) {
        bestEdge = edge;
        index = i;
      }
    });
    // 找出最佳的邊 兩個節點都訪問過 淘汰掉 
    // 因為若兩邊節點都被訪問 代表會形成loop 所以要淘汰
    if (bestEdge.node1.visited && bestEdge.node2.visited) {
      tempBucket.splice(index, 1);
      bestEdge = null; // 被淘汰了 重新找
    }
  }

  return bestEdge;
}

console.log(getMSTByPrim(A));
```
