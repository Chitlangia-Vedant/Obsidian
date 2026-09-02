[57. Insert Interval](https://leetcode.com/problems/insert-interval/)

You are given an array of non-overlapping intervals `intervals` where `intervals[i] = [starti, endi]` represent the start and the end of the `ith` interval and `intervals` is sorted in ascending order by `starti`. You are also given an interval `newInterval = [start, end]` that represents the start and end of another interval.

Two intervals are considered overlapping if they share **at least** one point.

Insert `newInterval` into `intervals` such that `intervals` is still sorted in ascending order by `starti` and `intervals` still does not have any overlapping intervals (merge overlapping intervals if necessary).

Return `intervals` _after the insertion_.

**Note** that you don't need to modify `intervals` in-place. You can make a new array and return it.

**Example 1:**

**Input:** intervals = `[[1,3],[6,9]]`, newInterval = `[2,5]`
**Output:** `[[1,5],[6,9]]`

**Example 2:**

**Input:** intervals = `[[1,2],[3,5],[6,7],[8,10],[12,16]]`, newInterval = `[4,8]`
**Output:** `[[1,2],[3,10],[12,16]]`
**Explanation:** Because the new interval `[4,8]` overlaps with `[3,5],[6,7],[8,10]`.

**Constraints:**

- `0 <= intervals.length <= 104`
- `intervals[i].length == 2`
- `0 <= starti <= endi <= 105`
- `intervals` is sorted by `starti` in **ascending** order.
- `newInterval.length == 2`
- `0 <= start <= end <= 105`

# Approach 1
## Intuition

The intervals are already sorted by start time, so we only need to find where `newInterval` fits.

Your approach uses two `lower_bound`s:
- `smaller` → first interval whose **start** is >= `newInterval[0]`.
- `larger` → first interval whose **end** is >= `newInterval[1]`.

Then:
1. Check whether the interval just before `smaller` overlaps with `newInterval`.
2. Check whether the interval at `larger` overlaps with `newInterval`.
3. If both sides overlap, merge them and erase all intervals between them.
4. Otherwise, erase the intervals covered by `newInterval`.
5. If `newInterval` did not overlap anything, insert it at `smaller`.

## Algorithm

1. Find `smaller` using:
   `lower_bound(..., newInterval)`
   This finds the first interval with `start >= newInterval[0]`.

2. Find `larger` using a custom comparator:
   `x[1] < val[1]`
   This finds the first interval with `end >= newInterval[1]`.

3. If `smaller > larger`, return the original intervals because the calculated range is invalid.

4. Check the interval before `smaller`:
   - If its end is >= `newInterval[0]`, it overlaps.
   - Extend its end to `newInterval[1]`.
   - Set `add = false`.

5. Check the interval at `larger`:
   - If its start is <= `newInterval[1]`, it overlaps.
   - Extend its start to `newInterval[0]`.
   - Set `add = false`.

6. If both sides overlap:
   - Merge the left and right intervals.
   - Erase everything from `smaller` through `larger`.

7. Otherwise, erase the intervals from `smaller` to `larger`.

8. If `add` is still true, no interval overlapped with `newInterval`, so insert it at `smaller`.
## CODE (Mine)

```cpp
class Solution {
public:
    vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
        auto smaller=lower_bound(intervals.begin(),intervals.end(),newInterval);
        auto larger=lower_bound(intervals.begin(),intervals.end(),newInterval,[](auto x,auto val){return x[1]<val[1];});
        bool add=true;
        if(smaller>larger) return intervals;
        if(smaller!=intervals.begin()&&(*(smaller-1))[1]>=newInterval[0]){
            (*(smaller-1))[1]=newInterval[1];
            add=false;
        }
        if(larger!=intervals.end()&&(*larger)[0]<=newInterval[1]){
            (*larger)[0]=newInterval[0];
            add=false;
        }
        if(smaller!=intervals.begin()&&larger!=intervals.end()&&(*(smaller-1))[1]>=(*larger)[0]){
            (*(smaller-1))[1]=(*larger)[1];
            intervals.erase(smaller,larger+1);
        }else{
            intervals.erase(smaller,larger);
        }
        if(add){
            intervals.insert(smaller,newInterval);
        }
        return intervals;
    }
};
```
## Dry Run

`intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]]`
`newInterval = [4,8]`

`smaller` points to `[6,7]` because `6 >= 4`.

`larger` finds the first interval whose end >= `8`, which is `[8,10]`.

Now check the interval before `smaller`:
`[3,5]`

Since `5 >= 4`, it overlaps.

So:
`[3,5] → [3,8]`

Now check `larger`:
`[8,10]`

Since `8 <= 8`, it overlaps.

So:
`[8,10] → [4,10]`

Both sides overlap, so merge them:
`[3,8] + [4,10] → [3,10]`

Erase the intervals between them.

Result:
`[[1,2],[3,10],[12,16]]`

## Time & Space Complexity

- `lower_bound` → `O(log n)`
- `erase` / `insert` on a vector → `O(n)` in the worst case.
- Overall: **O(n)**
- Extra Space: **O(1)** apart from the returned vector / modifications.

## Key Idea

Use the sorted order to locate the range affected by `newInterval`, then merge the overlapping intervals and erase the intervals that are completely covered.

# Approach 2
## Intuition

```
intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
```

In solution 2, we try to find a place where we should insert new interval. To do that, **we compare end time of current interval with start time of new interval.**

```sql
end time of current < start time new

   ↓
[[1,2],[3,5],[6,7],[8,10],[12,16]]
= 2 < 4 → true

res = [[1,2]]

         ↓
[[1,2],[3,5],[6,7],[8,10],[12,16]]
= 5 < 4 → false, we found a place to insert new interval.
```

In the second step, compare start time of current interval with end time of new interval to comfirm current two intervals are overlapping each other.

---

⭐️ Points

- Why we check the second step?

Somebody is wondering why we check the step step because we already checked they are overlapping in the first step.

The answer is there is possiblity that we have multiple intervals after current interval.

If we definitely know current interval is the last, we don't have to check it but look at input array. We will start to merge `[3,5]` and `[4,8]`, but we still have `3` intervals after `[3,5]`.

In that case, we have to comcpare start time of current interval with end of the last interval in `res`.

That's why we need the second step.

---

If the second step, the condition below is true, we will update start and end time of new interval.

```sql
start time of current interval <= end time of new interval
```

```javascript
                     ↓
intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]]
newInterval = [4,8]

= 3 <= 8 → true
```

In this case, we will update newInterval. Start time and end time should be

```sql
start = min(start time of new, start time of current)
end = max(end time of new, end time of current)
```

we want the widest range if we merge multiple intervals, that's why start time should be minimum number and end time should be maximum number.

```sql
                     ↓
intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]]
newInterval = [4,8]

= 3 <= 8 → true

newInterval[0] = min(4, 3)
newInterval[1] = max(8, 5)
newInterval = [3,8]
```

We continue until we don't meet

```sql
start time of current interval <= end time of new interval
```

```sql
res = [[1,2]]

                           ↓
intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]]
newInterval = [3,8]

= 6 <= 8 → true, they are overlapping

newInterval[0] = min(3, 6)
newInterval[1] = max(8, 7)
newInterval = [3,8]
----------------------------------------------------------
                                 ↓
intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]]
newInterval = [3,8]

= 8 <= 8 → true, they are overlapping

newInterval[0] = min(3, 8)
newInterval[1] = max(8, 10)
newInterval = [3,10]
----------------------------------------------------------
                                         ↓
intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]]
newInterval = [3,10]

= 12 <= 10 → false, they are not overlapping
```

In that case, we break the second loop and add the new interval.

```
res = [[1,2],[3,10]]
```

After that, all we have to do is add rest of intervals which is `[12,16]`.

```kotlin
return [[1,2],[3,10],[12,16]]
```

## Complexity

- Time complexity: O(n)

- Space complexity: O(n)
## CODE

```cpp
class Solution {
public:
    vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
        int n = intervals.size();
        vector<vector<int>> res;

        int i = 0;
        while (i < n && intervals[i][1] < newInterval[0]) {
            res.push_back(intervals[i]);
            i++;
        }

        while (i < n && intervals[i][0] <= newInterval[1]) {
            newInterval[0] = min(newInterval[0], intervals[i][0]);
            newInterval[1] = max(newInterval[1], intervals[i][1]);
            i++;
        }

        res.push_back(newInterval);
        while (i < n) {
            res.push_back(intervals[i]);
            i++;
        }

        return res;
    }
};
```

## Step by Step Algorithm

```python
n = len(intervals)
```

- **Explanation**: Calculate the total number of intervals and store it in `n`. This helps control the loop later when iterating through all intervals.

```python
res = []
```

- **Explanation**: Initialize an empty list `res` to store the final merged intervals.

```python
i = 0
while i < n and intervals[i][1] < newInterval[0]:
    res.append(intervals[i])
    i += 1
```

- **Explanation**:
    - **Condition**: Continue the loop while `i < n` and the end of the current interval (`intervals[i][1]`) is less than the start of `newInterval` (`newInterval[0]`).
    - This part of the code adds all intervals that are completely before `newInterval` (no overlap) to `res`.
    - **Action**: Each non-overlapping interval is added to `res`, and the index `i` is incremented.

```python
while i < n and intervals[i][0] <= newInterval[1]:
    newInterval[0] = min(newInterval[0], intervals[i][0])
    newInterval[1] = max(newInterval[1], intervals[i][1])
    i += 1
```

- **Explanation**:
    - **Condition**: Continue the loop while `i < n` and the start of the current interval (`intervals[i][0]`) is less than or equal to the end of `newInterval` (`newInterval[1]`).
    - This indicates overlapping intervals, so they need to be merged.
    - **Action**: Update `newInterval` to merge with `intervals[i]` by adjusting:
        - The start of `newInterval` to the minimum of the current start values.
        - The end of `newInterval` to the maximum of the current end values.
    - After merging, increment `i` to move to the next interval.

```python
res.append(newInterval)
```

- **Explanation**: Add the merged `newInterval` (containing all merged overlapping intervals) to `res`.

```python
res.extend(intervals[i:])
```

- **Explanation**: Add any remaining intervals after `newInterval` (which are not overlapping) directly to `res`.

```python
return res
```

- **Explanation**: Return `res`, which now contains all intervals, including the newly inserted interval, with all overlapping intervals merged.