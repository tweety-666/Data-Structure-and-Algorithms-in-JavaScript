# 資料結構與演算法 - hash table
###### tags: `資料結構與演算法`,`Algorithms`,`Data Structure`

## Hash Table
Hash Table本身是一個array，裡面的每一個元素都是帶有key跟value的object，叫做Buckets。
![](https://i.imgur.com/bwJbkeT.png)


## Hash Table要解決的問題
1. 在陣列中要找到特定value的資料是很慢的。時間複雜度是O(n)。
2. 找指定key的陣列內元素是快的，但會耗費很多的記憶體空間。
3. 空間和時間，終究有一個會被浪費掉。

> Hash Table更能節省空間跟時間。
> 插入元素、移除元素跟找某個元素都只要 O(1) 的時間複雜度。

## Hash function
Hash function的核心概念就是把一個value轉換成另一個。
例如帳密被存入資料庫時，也會加密過，也可以說成被hashed過。array跟object也是被hashed過的。

hash function的功勞在於：如果在陣列內尋找特定元素，要遍歷陣列一個個去找，時間複雜度是O(n)。在Hash Table中特定元素，只要先把這個元素丟進hash function，就可以直接知道元素對應到Hash Table中哪一格。

## Division method
Division method就是根據key把元素塞進Hash Table內的某一格index。
```
m = Hash Table size
n = 要存進Hash Table內的元素的數量
index = key % m
0 <= index < m
```
* 優點：速度比較快
* 缺點：容易有很多collision
* Load factor：
```
m = Hash Table size
n = 要存進Hash Table內的元素的數量
load factor = n/m （會介於0~1）
```

## Collision
假設有兩個以上的元素被hashed到Hash Table內的同一格（有著一樣的index），會有衝突（collision）。


## Multiplication method
```
A=(5^1/2-1/)2
index = [m(keyA%1)]`
```
根據上述公式將元素塞進Hash Table。


## 處理collision
使用chaining處理collision：當有兩個以上的元素要塞進同一格，將元素改為`[elementA,elementB]`，再放進同一格。使得Hash Table基本上是array of arrays。

## 實作
```javascript=
class HashTable{
    constructor(size){
        this.size = size;
        this.table = []
        // HashTable is array of arrays
        for (let i=0;i<size;i++){
            this.table.push([]);
        }
    }
    // division method
    hashDivision(key){
        return key % this.size;
    }
     // multiplication method
    hashMultiplication(key){
        let A = (Math.sqrt(5)-1)/2
        return Math.floor( (key*A%1) * this.size);
    }
   
    set(key,value){
        let index = this.hashMultiplication(key)
        this.table[index].push({key,value})
    }
    
    get(key){
        let index = this.hashMultiplication(key)
        for(let i=0;i<this.table[index].length; i++){ // 怕有collision
            if(this.table[index][i].key == key){
                return this.table[index][i]
            }
        }
    }
    
    printAll(){
        console.log(this.table)
    }
}

const myHashTable = new HashTable(10)
```

## 何時該使用Hash Table？
1. 確保搜尋時，時間複雜度為O(1)。
2. 在大量資料內，進行搜尋、存入、刪除時。效率上比起array快，array會需要遍歷才知道元素內的key；在Hash Table內，只要有給key去索引，就很好進行搜尋、存入、刪除。
3. Hash Table無法像array那樣可以一個個針對元素去做個別處理。這時候就需要array。