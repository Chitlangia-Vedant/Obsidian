# Rat in a Maze- All Variations: (8 Variations)

Check for Possible Path in a 2D Matrix
The Task is to "Check if there is a path from source to destination"

Source: Top Left: mat[0][0]
Destination: Bottom Right: mat[m-1][n-1]

In the Matrix,
-1 is considered as BLOCKAGE
(Cannot Go through that Cell)

0 is considered to be SAFE CELL
(Can Go through it)


Note:
Source and Destination will ALWAYS be 0 (SAFE CELL)

Directions:
All 4 Directions: Up, Down, Left, Right


## Constraints:

0 <= i < row
0 <= j < col
1 <= row <= 1000
1 <= col <= 1000


## Test Case-1:

```
mat[][]
= 
{
   S:0  0   -1   0
	-1  0   -1   -1
	0   0   -1    0
	-1  0    0    0: D
}

S: 0,0
D: m-1, n-1
```

OP: true

Path:

```
{
   S:M  M   -1    0
	-1  M   -1   -1
	 0  M   -1    0
	-1  M    M    M: D
}
```

## Test Case-2:

```
mat[][]
= 
{
   S:0  0   -1   0
	-1  -1   -1   -1
	0   0   -1    0
	-1  0    0    0: D
}

S: 0,0
D: m-1, n-1
```

OP:
false

## Test Case-3: (V2)
```
mat[][]
= 
{
   S:0  0   -1   0
	-1  0   -1   -1
	0   0    0:D  0
	-1  0    0    0
}

S: 0,0
D: x, y
x = 2, y = 2
```

OP:
true

## Test Case-4: (V2)

```
mat[][]
= 
{
   S:0  0   -1   0
	-1  0   -1   -1
	0   0   -1  0:D
	-1  0   -1    0
}

S: 0,0
D: x, y

x = 2, y = 3
```

OP:
false

## Test Case-5: (V4)

```
mat[][]
= 
{
    0  0   0   0: S
	-1  0   -1   -1
	0  D:0   -1  0
	-1  0   -1    0
}

S: c, d
D: x, y

c = 0, d = 3
x = 2, y = 1
```

OP:
true

## Test Case-6: (V6)

```
mat[][]
= 
{
   S:0  0   0   0: 
	-1  0   -1   -1
	0   0   0:D  0
	-1  0   -1    0
}

S: c, d
D: x, y

c = 0, d = 0
x = 2, y = 2

K = 2 Steps

(Jump 2 Steps at Once - Right, Left, Up, Down)
```

OP:
true

2 Ways:
- Right and Down
- Down and Right

## Test Case-7: (V6)

```
mat[][]
= 
{
   S:0  0   0   0: 
	-1  0   -1   -1
	0   0   0   0: D
	-1  0   -1    0
}

S: c, d
D: x, y

c = 0, d = 0
x = 2, y = 3

K = 2 Steps

(Jump 2 Steps at Once - Right, Left, Up, Down)
```

OP:
false

## Test Case-8: (V7)

```
mat[][]
= 
{
   S:0  0   0   0: 
	-1  0   -1   -1
	0   0   0   0: D
	-1  0   -1    0
}

S: c, d
D: x, y

c = 0, d = 0
x = 2, y = 3

K = 2 Steps

(Jump At Max 2 Steps - Right, Left, Up, Down)
```

OP:
true

## Function
```
bool canTravel(int[][] mat)
{

}
```

# Solution:

## Direction Matrix/Mapping:

Curr State: i, j

```
R: i, j -> i, j+1: [0, 1]
L: i, j -> i, j-1: [0, -1]
U: i, j -> i-1, j: [-1, 0]
D: i, j -> i+1, j: [1, 0]
```

Diagonal Directions:

```
NW: up left: i, j -> i-1, j-1: [-1, -1]

NE: up right: i, j -> i-1, j+1: [-1, 1]

SW: down left: i, j -> i+1, j-1: [1, -1]

SE: down right: i, j -> i+1, j+1: [1, 1]
```

## Approach:

(1) Queue which stores current cell (i,j) for PATH
(2) Insert (0,0) (Source) in Queue
(3) Run a loop till Queue is Empty
- Check for All Paths - Backtracking
(4) Mark All Cells travelled as Visited
(5) If you reach Destination, 
	return true
else
	return false


## Variations:

(1) V1: S = (0, 0), D = [m-1, n-1], Dir = 4 Directions
(2) V2: S = (0, 0), D = [x, y], Dir = 4 Directions
// Source Constant, Varying Destinations for Each Test Case
(3) V3: S = (0, 0), D = [x, y], Dir = 2 Directions // Right and Down
(4) V4: S = (c, d), D = [x, y], Dir = 4 Directions
// Varying Source, Varying Destinations for Each Test Case
(5) V5: S = (c, d), D = [x, y], Dir = 8 Directions

(6) V6: S = (c, d), D = [x, y], Dir = 8 Directions, EXACTLY K Steps at a Time - GOOGLE
(7) V7: S = (c, d), D = [x, y], Dir = 8 Directions, Maximum K Steps at a Time (1,2,3,4,5......K)

(1.......K)- Take Any Number of Steps - Left/Right/Up/Down etc


# CODE:

```python
def pathExist(mat, c, d, x, y, K):
	 
	Dir = [[0,1], [0,-1], [-1,0], [1,0], [-1, -1], [-1,1], [1,-1], [1,1]]
	vis = False * max(N) // N = m*n - Mark All cells as Unvisited

	queue = [] // Queue for Storing Paths
	queue.append((c,d)) // Source
	// queue.append((0,0)) // V1

	while (len(queue)>0):

		curr_pair = queue[0] // Queue contains current pair, curr_pair = i, j
		queue.pop()

		vis[curr_pair[0]][curr_pair[1]] = true // vis[i][j] = true

		// Check if destination is reached
		if (curr_pair == x,y): 
			return true

		// Check for All Possible Paths - Backtracking 
		// curr_pair = i, j, curr_pair[0] = i, curr_pair[1] = j

		for i in range((len(Dir))):
			for j in range(1,K+1):
				row = curr_pair[0] + j * Dir[i][0]
				col = curr_pair[1] + j * Dir[i][1]

			// Boundary Conditions and Not Already Visited and Non-Blockage Cell
				if (row>0 && row<m && col>0 && col<n && mat[row][col]!=-1 && vis[row][col]==false):
					queue.append((row, col))

	// reached here, outside while, means exhausted all possibilities - Path does not exist
	return false
```


## First Way:

Brute Force:

backtrack(i, j-1)
backtrack(i, j+1)
backtrack(i-1, j)
backtrack(i+1, j)

TC: O(4^N)
N: m*n: Total Number of Cells

## Second Way:

for i in range((len(Dir))):
	row = curr_pair[0] + Dir[i][0]
	col = curr_pair[1] + Dir[i][1]


row = i+1, col = j: DOWN
row = i-1, col = j: UP

TC: O(len(Dir))
