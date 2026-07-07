# [118. Pascal's Triangle](https://leetcode.com/problems/pascals-triangle/)

Given an integer `numRows`, return the first numRows of **Pascal's triangle**.

In **Pascal's triangle**, each number is the sum of the two numbers directly above it as shown:

![](https://upload.wikimedia.org/wikipedia/commons/0/0d/PascalTriangleAnimated2.gif)

**Example 1:**

**Input:** numRows = 5
**Output:** [[1],[1,1],[1,2,1],[1,3,3,1],[1,4,6,4,1]]

**Example 2:**

**Input:** numRows = 1
**Output:** [[1]]

**Constraints:**

- `1 <= numRows <= 30`

```cpp
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        
    }
};
```

# Solution 1 : Dynamic Programming

## CODE (Mine)

```cpp
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int>> pascal;
        pascal.push_back({1}); 
        for(int i=1;i<numRows;i++){
            vector<int> layer;
            for(int j=0;j<=i;j++){
                if(j==0||j==i){
                    layer.push_back(1);
                }
                else{
                    layer.push_back(pascal[i-1][j-1]+pascal[i-1][j]);
                }
            }       
            pascal.push_back(layer);    
        }
        return pascal;
    }
};
```
## CODE (Mine)(Space Optimized)

```cpp
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int>> pascal(numRows);
        for(int i=0;i<numRows;i++){
            for(int j=0;j<=i;j++){
                if(j==0||j==i){
                    pascal[i].push_back(1);
                }
                else{
                    pascal[i].push_back(pascal[i-1][j-1]+pascal[i-1][j]);
                }
            }           
        }
        return pascal;
    }
};
```

# Solution 2: Using Combinatorial Formula

**Intuition:** Pascal's triangle **can also be generated using combinatorial formula C(n, k) = C(n-1, k-1) + C(n-1, k)**, where C(n, k) **represents the binomial coefficient**. **We can calculate each element of the triangle using this formula.** This approach avoids the need for storing the entire triangle in memory, **making it memory-efficient.**

- Use the binomial coefficient formula C(n, k) to calculate each element.
- Build the triangle row by row using the formula.

```cpp
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int>> pascal(numRows);

        for (int i = 0; i < numRows; i++) {
            pascal[i].resize(i + 1);

            long long val = 1;   // C(i, 0)

            for (int j = 0; j <= i; j++) {
                pascal[i][j] = (int)val;

                // Compute C(i, j+1) from C(i, j)
                val = val * (i - j) / (j + 1);
            }
        }

        return pascal;
    }
};
```

# Solution 3: Using Recursion

**Intuition:** In Pascal's triangle, **each element is the sum of the two elements directly above it**. We can use a **recursive approach** to generate the triangle. **The base case is when numRows is 1**, in which case we return [[1]]. **Otherwise, we recursively generate the triangle for numRows - 1** and then calculate the current row by summing the adjacent elements from the previous row.

- Base case: If numRows is 1, return [[1]].
- Recursively generate the triangle for numRows - 1.
- Calculate the current row by summing adjacent elements from the previous row.

```cpp
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        if (numRows == 0) return {};
        if (numRows == 1) return {{1}};
        
        vector<vector<int>> prevRows = generate(numRows - 1);
        vector<int> newRow(numRows, 1);
        
        for (int i = 1; i < numRows - 1; i++) {
            newRow[i] = prevRows.back()[i - 1] + prevRows.back()[i];
        }
        
        prevRows.push_back(newRow);
        return prevRows;
    }
};
```