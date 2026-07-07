Temporary Memory used to fetch frequently accessed data quickly

Cache has a Size Limit

Cache: [1,2,3,4,5]
Limit: 5

Insert: 6
Need to Remove one value from [1,2,3,4,5]
- This is called Cache Replacement Algorithms


Eg of Cache Replacement Algorithms:
- LRU: Least Recently Used
- MRU: Most Recently Used
- LFU: Least Frequently Used
- MFU: Most Frequently Used
etc
# LRU Cache

When the Cache Memory is full, 
LRU Picks the data that is least recently used (read/write), and removes it in order to 
make space for new data

## Working of LRU Cache:

Let's suppose we have a Cache following LRU Policy
Capacity = 3
### Operations:

A - put (key=1, value = A) 
B - put (key=2, value = B) 
C - put (key=3, value = C) 
D - get (key = 2)
E - get (key = 4)
F - put (key = 4, value = D)
G - put (key = 3, value = E)
H - put (key = 5, value = K)

### Output: 

Originally:
cache = [], capacity : 3

```
A - cache = [{1,A}] - No Replacement Required, curr_size <= capacity
B - cache = [{1,A}, {2,B}] - No Replacement Required, curr_size <= capacity
C - cache = [{1,A}, {2,B}, (3,C)] - No Replacement Required, curr_size <= capacity
D - OP: {2,B}: Priority Order: [{2,B}, {3,C}, {1,A}: LRU]
E - OP: -1, because key: 4 is NOT Present in Cache
F - OP: Now cache is FULL, we use LRU to determine key with least priority and remove it
	LRU: {1,A} -> Removed
	Cache: [{4,D}, {2,B}, {3,C}: LRU]
G - OP: 	Cache: [{3,E}, {4,D}, {2,B}: LRU]
H - OP: Now cache is FULL, we use LRU to determine key with least priority and remove it
	LRU: {2,B} -> Removed
	Cache: [{5,K}, {3,E}, {4,D}: LRU]
```