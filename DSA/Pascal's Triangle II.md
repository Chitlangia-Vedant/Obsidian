# [119. Pascal's Triangle II](https://leetcode.com/problems/pascals-triangle-ii/)

Given an integer `rowIndex`, return the `rowIndexth` (**0-indexed**) row of the **Pascal's triangle**.

In **Pascal's triangle**, each number is the sum of the two numbers directly above it as shown:

![](https://upload.wikimedia.org/wikipedia/commons/0/0d/PascalTriangleAnimated2.gif)

**Example 1:**

**Input:** rowIndex = 3
**Output:** [1,3,3,1]

**Example 2:**

**Input:** rowIndex = 0
**Output:** [1]

**Example 3:**

**Input:** rowIndex = 1
**Output:** [1,1]

**Constraints:**

- `0 <= rowIndex <= 33`

**Follow up:** Could you optimize your algorithm to use only `O(rowIndex)` extra space?

```cpp
class Solution {
public:
    vector<int> getRow(int rowIndex) {
        
    }
};
```

# Solution 1

## CODE (Mine)
```cpp
class Solution {
public:
    vector<int> getRow(int rowIndex) {
        vector<int> pascal(rowIndex+1);
        for(int i=0;i<rowIndex+1;i++){
            for(int j=0;j<=i;j++){
                if(j==0||j==i){
                    pascal[j]=1;
                }else if(j<i/2.0){
                    pascal[j]=pascal[i-j]+pascal[i-j-1];
                }else if(j==i/2.0){
                    pascal[j]=2*pascal[j];
                }else{
                    pascal[j]=pascal[i-j];
                }
            }           
        }
        return pascal;
    }
};
```

## CODE (Better)

```cpp
class Solution {
public:
    vector<int> getRow(int rowIndex) {
        vector<int> A(rowIndex+1, 0);
        A[0] = 1;
        for(int i=1; i<rowIndex+1; i++)
            for(int j=i; j>=1; j--)
                A[j] += A[j-1];
        return A;
    }
};
```

# Solution 2

Formula for Pascal's Triangle Recurrence Relation.

```
next_element = previous element×(rowIndex−position+1) / position
```

## CODE 

```cpp
class Solution:
    def getRow(self, rowIndex: int) -> List[int]:
        row = [1]

        for i in range(1, rowIndex + 1):
            next_element = row[i - 1] * (rowIndex - i + 1) // i
            row.append(next_element)

        return row
```