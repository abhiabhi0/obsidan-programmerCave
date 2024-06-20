****LRU**** algorithm is a standard problem and it may have variations as per need for example, in operating systems ****LRU**** plays a crucial role as it can be used as a page replacement algorithm in order to minimize page faults.

## Operations on LRU Cache:

- ****LRUCache (Capacity c):**** Initialize LRU cache with positive size capacity ****c.****
- ****get (key)****: Returns the value of Key ‘****k’**** if it is present in the cache otherwise it returns -1. Also updates the priority of data in the LRU cache.
- ****put (key, value):**** Update the value of the key if that key exists, Otherwise, add key-value pair to the cache. If the number of keys exceeded the capacity of LRU cache then dismiss the least recently used key.

```python
1. from collections import OrderedDict  

3. class LRUCacheImplement:  
4.   def __init__(self, size):  
5.     self.size = size  
6.     self.lru_cache = OrderedDict()  

8.   def get(self, key):  
9.     try:  
10.       value = self.lru_cache.pop(key)  
11.       self.lru_cache[key] = value  
12.       return value  
13.     except KeyError:  
14.       return -1  

16.   def put(self, key, value):  
17.     try:  
18.       self.lru_cache.pop(key)  
19.     except KeyError:  
20.       if len(self.lru_cache) >= self.size:  
21.         self.lru_cache.popitem(last=False)  
22.     self.lru_cache[key] = value  

24.   def show_entries(self):  
25.     print(self.lru_cache)  

27. # Create an LRU Cache with a size of 3  
28. cache = LRUCacheImplement(3)  

31. cache.put("1","1")  
32. cache.put("2","2")  
33. cache.put("3","3")  

35. cache.get("1")  
36. cache.get("3")  

38. cache.put("4","4")   
39. cache.show_entries()   
40. cache.put("5","5")   
41. cache.show_entries()
```

```go
package main

import (
	"container/list"
	"fmt"
)

type Node struct {
	Data   int
	KeyPtr *list.Element
}

type LRUCache struct {
	Queue    *list.List
	Items    map[int]*Node
	Capacity int
}

func Constructor(capacity int) LRUCache {
	return LRUCache{Queue: list.New(), Items: make(map[int]*Node), Capacity: capacity}
}

func (l *LRUCache) Get(key int) int {
	if item, ok := l.Items[key]; ok {
		l.Queue.MoveToFront(item.KeyPtr)
		return item.Data
	}
	return -1
}

func (l *LRUCache) Put(key int, value int) {
	if item, ok := l.Items[key]; !ok {
		if l.Capacity == len(l.Items) {
			back := l.Queue.Back()
			l.Queue.Remove(back)
			delete(l.Items, back.Value.(int))
		}
		l.Items[key] = &Node{Data: value, KeyPtr: l.Queue.PushFront(key)}
	} else {
		item.Data = value
		l.Items[key] = item
		l.Queue.MoveToFront(item.KeyPtr)
	}

}

func main() {
	// Test case 1
	// ["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
	// [[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
	//	[null, null, null, 1, null, -1, null, -1, 3, 4]
	fmt.Println("Test case 1")
	obj := Constructor(2)
	obj.Put(1, 1)
	obj.Put(2, 2)
	fmt.Println(obj.Get(1))
	obj.Put(3, 3)
	fmt.Println(obj.Get(2))
	obj.Put(4, 4)
	fmt.Println(obj.Get(1))
	fmt.Println(obj.Get(3))
	fmt.Println(obj.Get(4))

	// Test case 2
	// 	["LRUCache","put","put","put","put","get","get"]
	// [[2],[2,1],[1,1],[2,3],[4,1],[1],[2]]
	// [null,null,null,null,null,-1,3]
	fmt.Println("Test case 2")
	obj = Constructor(2)
	obj.Put(2, 1)
	obj.Put(1, 1)
	obj.Put(2, 3)
	obj.Put(4, 1)
	fmt.Println(obj.Get(1))
	fmt.Println(obj.Get(2))
}
```

