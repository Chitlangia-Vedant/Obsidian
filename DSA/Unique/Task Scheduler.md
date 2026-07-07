# [621. Task Scheduler](https://leetcode.com/problems/task-scheduler/)

You are given an array of CPU `tasks`, each labeled with a letter from A to Z, and a number `n`. Each CPU interval can be idle or allow the completion of one task. Tasks can be completed in any order, but there's a constraint: there has to be a gap of **at least** `n` intervals between two tasks with the same label.

Return the **minimum** number of CPU intervals required to complete all tasks.

**Example 1:**

**Input:** tasks = ["A","A","A","B","B","B"], n = 2

**Output:** 8

**Explanation:** A possible sequence is: A -> B -> idle -> A -> B -> idle -> A -> B.

After completing task A, you must wait two intervals before doing A again. The same applies to task B. In the 3rd interval, neither A nor B can be done, so you idle. By the 4th interval, you can do A again as 2 intervals have passed.

**Example 2:**

**Input:** tasks = ["A","C","A","B","D","B"], n = 1

**Output:** 6

**Explanation:** A possible sequence is: A -> B -> C -> D -> A -> B.

With a cooling interval of 1, you can repeat a task after just one other task.

**Example 3:**

**Input:** tasks = ["A","A","A", "B","B","B"], n = 3

**Output:** 10

**Explanation:** A possible sequence is: A -> B -> idle -> idle -> A -> B -> idle -> idle -> A -> B.

There are only two types of tasks, A and B, which need to be separated by 3 intervals. This leads to idling twice between repetitions of these tasks.

**Constraints:**

- `1 <= tasks.length <= 104`
- `tasks[i]` is an uppercase English letter.
- `0 <= n <= 100`
  
```cpp
class Solution {
public:
    int leastInterval(vector<char>& tasks, int n) {
        
    }
};
```

# Solution 1: Using priority queue

- First count the number of occurrences of each task and store that in a map/vector.
- Then push the count into a priority queue, so that the maximum frequency task can be accessed and completed first.
- Then until all tasks are completed (i.e the priority queue is not empty):
    - intialise the cycle length as n+1. n is the cooldown period so the cycle will be of n+1 length.  
        Example: for ['A','A','A','B','B'] and n=2,  
        the tasks can occur in the following manner:  
        [A B idle]->[A B idle]->[A]. See here each cycle is n+1 length long, only then A can repeat itself.
    - for all elements in the priority queue, until the cycle length is exhausted, pop the elements out of the queue and if the task is occurring more than once then add it to the remaining array (which stores the remaining tasks).  
        This means that we are completing that task once in this cycle.So keep counting the time.
    - Then, add the occurrence of tasks back to the priority queue.
    - Add the idle time into the time count.

Idle time is the time that was needed in the cycle because no task was available. It is the remaining cycle length in our algorithm. Idle time should be only added if the priority queue is empty (i.e all tasks are completed).

## CODE

```cpp
class Solution {
public:
    int leastInterval(vector<char>& tasks, int n) {
        priority_queue<int> pq;
        vector<int>mp(26,0);

        for(char i:tasks){
            mp[i-'A']++;  // count the number of times a task needs to be done
        }   
        for(int i=0;i<26;++i){
            if(mp[i]) 
            pq.push(mp[i]);
        }

        int time=0; // stores the total time taken 
        while(!pq.empty()){
            vector<int>remain;
            int cycle=n+1;  // n+1 is the CPU cycle length, if n is the cooldown period then after a task A there will be n more tasks. Hence n+1.

            while(cycle and !pq.empty()){
                int max_freq=pq.top(); // the task at the top should be first assigned the CPU as it has highest frequency
                pq.pop();
                if(max_freq>1){ // task with more than one occurrence, the next occurrence will be done in the next cycle 
                    remain.push_back(max_freq-1); // add it to remaining task list
                }
                ++time; 
                --cycle; 
            }

            for(int count:remain){
                pq.push(count); 
            }
            if(pq.empty())break; // if the priority queue is empty than all tasks are completed and we don't need to include the idle time
            time+=cycle; // this counts the idle time 
        }
        return time;
    }
};
```
# Solution 2: By deducing a formula

Let's understand this with an example:

```javascript
tasks= ['A','A','A','B','B','B','C','D'], n=2
```

Maximum frequency=3 and maximum occurring task= A,B  
Here our possible solution is:

```rust
['A'->'B'->'C'] -> ['A','B','D'] ->['A','B']
total time: 3+3+2=8
```

Notice something: The cycle A->B->other_task is repeating 2 (maximum frequency-1) times and then A->B occurs.  
A and B are the maximum frequency elements which are making the cycle of length 3(n+1).  
We can say that

```sql
total time= (cycle length)*(maximum frequency-1) + number maximum frequency tasks that are left  
i.e total time=(n+1)*(max_freq-1)+count_maxfreq_task
```

In scenarios where the total time is less than the number of tasks, the minimum time required would be the number of tasks itself.

## CODE

```cpp
#include <vector>
#include <iostream>
#include <unordered_map>
#include <algorithm>

int leastInterval(const std::vector<char>& tasks, int n) {
    if (tasks.empty()) return 0;

    std::unordered_map<char, int> taskCounts;
    for (char task : tasks) {
        taskCounts[task]++;
    }

    int maxCount = 0;
    int maxCountTasks = 0;
    for (const auto& pair : taskCounts) {
        if (pair.second > maxCount) {
            maxCount = pair.second;
            maxCountTasks = 1;
        } else if (pair.second == maxCount) {
            maxCountTasks++;
        }
    }

    int partCount = maxCount - 1;
    int partLength = n - (maxCountTasks - 1);
    int emptySlots = partCount * partLength;
    int availableTasks = tasks.size() - maxCount * maxCountTasks;
    int idles = std::max(0, emptySlots - availableTasks);

    return tasks.size() + idles;
}
```