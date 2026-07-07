# [146. LRU Cache](https://leetcode.com/problems/lru-cache/)

Design a data structure that follows the constraints of a **[Least Recently Used (LRU) cache](https://en.wikipedia.org/wiki/Cache_replacement_policies#LRU)**.

Implement the `LRUCache` class:

- `LRUCache(int capacity)` Initialize the LRU cache with **positive** size `capacity`.
- `int get(int key)` Return the value of the `key` if the key exists, otherwise return `-1`.
- `void put(int key, int value)` Update the value of the `key` if the `key` exists. Otherwise, add the `key-value` pair to the cache. If the number of keys exceeds the `capacity` from this operation, **evict** the least recently used key.

The functions `get` and `put` must each run in `O(1)` average time complexity.

**Example 1:**

**Input**
```
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
**Output**
[null, null, null, 1, null, -1, null, -1, 3, 4]
```

**Explanation**
```
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // cache is {1=1}
lRUCache.put(2, 2); // cache is {1=1, 2=2}
lRUCache.get(1);    // return 1
lRUCache.put(3, 3); // LRU key was 2, evicts key 2, cache is {1=1, 3=3}
lRUCache.get(2);    // returns -1 (not found)
lRUCache.put(4, 4); // LRU key was 1, evicts key 1, cache is {4=4, 3=3}
lRUCache.get(1);    // return -1 (not found)
lRUCache.get(3);    // return 3
lRUCache.get(4);    // return 4
```

**Constraints:**

- `1 <= capacity <= 3000`
- `0 <= key <= 104`
- `0 <= value <= 105`
- At most `2 * 105` calls will be made to `get` and `put`.

```cpp
class LRUCache {
public:
    LRUCache(int capacity) {
        
    }
    
    int get(int key) {
        
    }
    
    void put(int key, int value) {
        
    }
};

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache* obj = new LRUCache(capacity);
 * int param_1 = obj->get(key);
 * obj->put(key,value);
 */
```

# Solution

## CODE (Mine)

```cpp
class LRUCache {
public:
    int capacity;

    // List stores {key, value}
    list<pair<int, int>> ls;

    // Maps key -> iterator in the list
    unordered_map<int, list<pair<int, int>>::iterator> mp;

    LRUCache(int capacity) {
        this->capacity = capacity;
    }

    int get(int key) {
        auto it = mp.find(key);

        // Key not found
        if (it == mp.end())
            return -1;

        // Move the accessed node to the front (most recently used)
        ls.splice(ls.begin(), ls, it->second);

        return it->second->second;
    }

    void put(int key, int value) {
        auto it = mp.find(key);

        // Key already exists
        if (it != mp.end()) {
            it->second->second = value;
            ls.splice(ls.begin(), ls, it->second);
            return;
        }

        // Insert new key-value pair
        ls.push_front({key, value});
        mp[key] = ls.begin();

        // Remove least recently used item if capacity exceeded
        if (mp.size() > capacity) {
            mp.erase(ls.back().first);
            ls.pop_back();
        }
    }
};
```

TC: O(1)
SC: O(N): Insert N Values