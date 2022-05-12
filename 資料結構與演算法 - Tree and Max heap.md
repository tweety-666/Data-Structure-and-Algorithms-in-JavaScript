# 資料結構與演算法 - Tree and Max heap
###### tags: `資料結構與演算法`,`Algorithms`,`Data Structure`

## 資料結構
### Tree的定義
Tree的結構長得很像技能樹或族譜。
一組tree只會有一個root為根節點，每個節點（node）下方可能還有子節點（child node）。
節點跟子節點合起來就是一個tree。
tree內如果還有其他tree，那可以稱呼其他tree為subtree。

如下圖：
* 2為root，包含下面所有子孫節點，這樣是一組tree。
* 2也是7跟5的child node，而對7跟5來說，2是他們的parent node，這樣就是一組tree，也是整個tree的subtree。

* 9是5的child node。5是9的parent node，這樣也是一組tree，也是整個tree的subtree。

![](https://i.imgur.com/W1XU4WY.png)

### Tree的種類
1. binary tree：
    一個node下面只有left child node跟right child node。 
2. full binary tree
    在一組binary tree內，每層child node的深度（detph）是一致的。如下圖的Max heap。
3. complete binary tree
   在一組接近full binary tree的tree中，少幾個最深層的child node。下圖的Max heap，假設拿掉最右下的3、2、6，就會是complete binary tree的圖。

### Max heap
Max heap會是一組complete binary tree，每個node最多只有left child node跟right child node，最少會沒有child node。
最大差異在於，每個subtree的root，都會是最大值。
如下圖：
最左下角的一組subtree內，root是13，child node是4跟8。
最右下角的一組subtree內，root是10，child node是2跟6。
整個tree的root是15，是整個tree中的最大值。
![](https://i.imgur.com/FwMeKpN.png)

