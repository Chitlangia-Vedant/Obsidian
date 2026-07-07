# Goldmine

Given a gold mine of m x n dimensions.

Each field in this mine contains a positive integer which is the amount of gold present at that position.
Initially the miner is at first column but can be at any row.

He can move only in three directions: right, right-up and right-down. 
That is, east, northeast and southeast from a given cell

Starting from any row (but always the first column), 
find out the maximum amount of gold that the miner can collect.

Input format

The first line contains T, the number of test cases. 
The following T lines contain 2 integers m and n which tell us the rows and columns respectively. 
This is followed by all the 2D array elements in a single line, separated by space

Output format

For each test case, output the maximum number of gold collected

Example Input #1

```
1
4 4 
1 3 1 5 
2 2 4 1 
5 0 2 3 
0 6 1 2
```

Output #1
```
16
```
# Understanding:

Directions: Right, Right Up, Right Down
Start: Any Row, First Column (Start from: 1,2,5,0)
End: Any Row, Last Column (End at: 5,1,3,2)

1 3 1 5 
2 2 4 1 
5 0 2 3 
0 6 1 2

Task: Collect Max Gold Coins

OP: 16

Explanation:

5-6-2-3: 16
5-2-4-5: 16

5-6-2-4-5: INVALID
1-3-2-4-2-3-2: INVALID

```
int getMaxGold(int[][] mat, int m, int n)
{

}
```

# Solution:

## (1) Identify:
"maximum amount of gold"


## (2) Decide a State Expression:

State Variables: Determines the current position in DP
				- Determines the Order of DP



state(row) ---> Not sufficient for finding position
state(col) ---> Not sufficient for finding position


state(row, col) ----> Sufficient for finding position

state(i,j): Maximum of Gold Coins Collected till (i,j)
			by traversing right, right up, right down

state(i,j) = MAX(right, right up, right down)

## (3) Formulate the State Relation - IMP

How does the current state result relates to previous results

state(k) ----> k-1, k-2 etc

ANS:
MAX(right, right up, right down)

Direction Matrix/Mapping:

curr: i, j

Right: i, j+1
Left: i, j-1
Up: i-1, j
Down: i+1, j
Right Up: i-1, j+1
Right Down: i+1, j+1

ANS = MAX(right, right up, right down)

`"mat[i][j] = max(mat[i][j+1], mat[i-1][j+1], mat[i+1][j+1])"`

State Relation:

`"mat[i][j] = max(mat[i][j+1], mat[i-1][j+1], mat[i+1][j+1])"`

Boundary Condition:

1st Row: No right up
Last Row: No right down
Last Col: No right, No right up, No right down

# CODE:

```Java
import java.io.*;
import java.util.*;

public class Main {
    
    static final int MAX = 100;
      
    static int getMaxGold(int gold[][], int m, int n)
    {
        int goldTable[][] = new int[m][n];// dp[m][n]
// goldTable[i][j] = Max No of Gold Coins Collected TILL (i,j) by Traversing in 3 Directions
//  Directions = [Right, Right Up, Right Down]
    
          
        for(int[] rows:goldTable)
            Arrays.fill(rows, 0);
      
        for (int col = n-1; col >= 0; col--)
        {
            for (int row = 0; row < m; row++)
            {
            /*
            
            Curr: i,j
            
            right: i,j+1
            right up: i-1, j+1
            right down: i+1, j+1
            
            first Row: No right_up
            last row: No right_down
            last col: No right, No right_up, No right_down


            
            */
                  
            // (i, j+1)
            int right = (col == n-1) ? 0 : goldTable[row][col+1]; 
      
            // (i-1, j+1)
            int right_up = (row == 0 || col == n-1) ? 0 : 
                goldTable[row-1][col+1];
      
            // (i+1, j+1)
            int right_down = (row == m-1 || col == n-1) ? 0 :
                goldTable[row+1][col+1];
      
                // Max gold collected from taking
                // either of the above 3 paths
            goldTable[row][col] = 
                gold[row][col] + Math.max(right, 
                            Math.max(right_up, right_down));
                // Curr pos + MAX(right/right up/right down)
                                                          
            }
        }
      

        int res = goldTable[0][0];
        
        for (int i = 0; i < m; i++)
        {
            for (int j=0; j<n; j++)
                System.out.print(goldTable[i][j] + " ");
            System.out.println(" ");            
        }
        
        System.out.println(" Stopped Printing");

         
        // Need to get Maximum Starting Only from 1st Column, Any ROW, HENCE, max(mat[i][0])
        for (int i = 1; i < m; i++)
            res = Math.max(res, goldTable[i][0]);
        
        // Question: Start from Only 1st Row, Any Column
        //	res = Math.max(res, goldTable[0][j]);

        // Question: Start from Any Row, Any Col
        //	res = Math.max(res, goldTable[i][j]);
        
        return res;
    }
    
    public static void main(String[] args) 
    {
    Scanner s = new Scanner(System.in);
    int tc,m,n,i=0,j=0;
    tc = s.nextInt();  
    int[][] gold= new int[MAX][MAX];

    while (tc-- > 0)
    {
        m = s.nextInt();
        n = s.nextInt();
        
        for (i=0; i<m; i++)
        {
            for (j=0; j<n; j++)
            {
                gold[i][j] = s.nextInt();
            }
        }
        
        System.out.println(getMaxGold(gold, m, n));
    }
  }
}
```

TC: O(M*N)
SC: O(M*N)

IP:
```
1
4 4 
1 3 1 5 
2 2 4 1 
5 0 2 3 
0 6 1 2
```

OP:
```
13 12 6 5  
14 11 9 1  
16 9 5 3  
11 11 4 2  
Stopped Printing
16
```