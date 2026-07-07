# [1329. Sort the Matrix Diagonally](https://leetcode.com/problems/sort-the-matrix-diagonally/)

A **matrix diagonal** is a diagonal line of cells starting from some cell in either the topmost row or leftmost column and going in the bottom-right direction until reaching the matrix's end. For example, the **matrix diagonal** starting from `mat[2][0]`, where `mat` is a `6 x 3` matrix, includes cells `mat[2][0]`, `mat[3][1]`, and `mat[4][2]`.

Given an `m x n` matrix `mat` of integers, sort each **matrix diagonal** in ascending order and return _the resulting matrix_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/01/21/1482_example_1_2.png)

```
**Input:** mat = [[3,3,1,1],[2,2,1,2],[1,1,1,2]]
**Output:** [[1,1,1,1],[1,2,2,2],[1,2,3,3]]
```

**Example 2:**

```
**Input:** mat = [[11,25,66,1,69,7],[23,55,17,45,15,52],[75,31,36,44,58,8],[22,27,33,25,68,4],[84,28,14,11,5,50]]
**Output:** [[5,17,4,1,52,7],[11,11,25,45,8,69],[14,23,25,44,58,15],[22,27,31,36,50,66],[84,28,75,33,55,68]]
```

# Solution1

1. Traverse `mat[row-1][0]` -> `mat[0][0]` -> `mat[0][column-1]`
2. For each `[i][j]` add every `[i+k][j+k]` (Till i+k<row && j+k<column) to a vector
3. Sort the vector
4. Add the sorted vector into same diagonal
## CODE (Mine)

```cpp
class Solution {
public:
    vector<vector<int>> diagonalSort(vector<vector<int>>& mat) {
        int row=mat.size(),column=mat[0].size();
        int i=row-1,j=0;
        while(i>=0){
            vector<int> dig;
            for(int a=i,b=j;a<row&&b<column;a++,b++){
                dig.push_back(mat[a][b]);
            }
            sort(dig.begin(),dig.end());
            int c=0;
            for(int a=i,b=j;a<row&&b<column;a++,b++){
                mat[a][b]=dig[c++];
            }
            i--;
        }
        i++;
        j++;
        while(j<column){
            vector<int> dig;
            for(int a=i,b=j;a<row&&b<column;a++,b++){
                dig.push_back(mat[a][b]);
            }
            sort(dig.begin(),dig.end());
            int c=0;
            for(int a=i,b=j;a<row&&b<column;a++,b++){
                mat[a][b]=dig[c++];
            }
            j++;
        }
        return mat;
    }
};
```

## CODE (Mine) (Cleaner)

```cpp
class Solution {
public:
    vector<vector<int>> diagonalSort(vector<vector<int>>& mat) {
        int row=mat.size(),column=mat[0].size();
        int i=row-1,j=0;
        while(i>0){
            vector<int> dig;
            for(int a=0;i+a<row&&j+a<column;a++){
                dig.push_back(mat[i+a][j+a]);
            }
            sort(dig.begin(),dig.end());
            for(int a=0;i+a<row&&j+a<column;a++){
                mat[i+a][j+a]=dig[a];
            }
            i--;
        }
        while(j<column){
            vector<int> dig;
            for(int a=0;i+a<row&&j+a<column;a++){
                dig.push_back(mat[i+a][j+a]);
            }
            sort(dig.begin(),dig.end());
            int c=0;
            for(int a=0,c;i+a<row&&j+a<column;a++){
                mat[i+a][j+a]=dig[a];
            }
            j++;
        }
        return mat;
    }
};
```

# Solution 2

1. We store values in the same diagonal into same list.
2. Sort the list.
3. Put values back

Here's an example to show how the code works:  
![[Pasted image 20260612160338.png]]

## CODE
```cpp
class Solution {
public:
    vector<vector<int>> diagonalSort(vector<vector<int>>& mat) {
        int m = mat.size(), n = mat[0].size();
        map<int, priority_queue<int, vector<int>, greater<int>>> d;
        for(int i=0; i < m; i++){
            for(int j=0; j < n; j++){
                d[i - j].push(mat[i][j]);
            }
        }
        for(int i=0; i < m; i++){
            for(int j=0; j < n; j++){
                mat[i][j] = d[i - j].top();
                d[i - j].pop();
            }
        }
        return mat;
    }
};
```