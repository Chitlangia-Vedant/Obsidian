# [997. Find the Town Judge](https://leetcode.com/problems/find-the-town-judge/)

In a town, there are `n` people labeled from `1` to `n`. There is a rumor that one of these people is secretly the town judge.

If the town judge exists, then:

1. The town judge trusts nobody.
2. Everybody (except for the town judge) trusts the town judge.
3. There is exactly one person that satisfies properties **1** and **2**.

You are given an array `trust` where `trust[i] = [ai, bi]` representing that the person labeled `ai` trusts the person labeled `bi`. If a trust relationship does not exist in `trust` array, then such a trust relationship does not exist.

Return _the label of the town judge if the town judge exists and can be identified, or return_ `-1` _otherwise_.

**Example 1:**

**Input:** n = 2, trust = `[[1,2]]`
**Output:** 2

**Example 2:**

**Input:** n = 3, trust = `[[1,3],[2,3]]`
**Output:** 3

**Example 3:**

**Input:** n = 3, trust = `[[1,3],[2,3],[3,1]]`
**Output:** -1

**Constraints:**

- `1 <= n <= 1000`
- `0 <= trust.length <= 104`
- `trust[i].length == 2`
- All the pairs of `trust` are **unique**.
- `ai != bi`
- `1 <= ai, bi <= n`

```Java
class Solution {
    public int findJudge(int n, int[][] trust) {
        
    }
}
```

# Understanding:

1 -----> 2
Town Judge: 2

1 ----> 3 <----- 2
Town Judge: 3

1 <----> 3 <----- 2
Town Judge: -1

# Solution:

Everybody (except for the town judge) trusts the town judge.
InDegree (townJudge) = N-1

The town judge trusts nobody.
OutDegree (townJudge) = 0

InDegree - OutDegree = N-1-0 = N-1

# Approach:

```
trust[i] = [ai, bi]

a ------> b: Edge
```

- Directed and Unweighted Graph

# CODE:

```Java
class Solution {
    public int findJudge(int n, int[][] trust) 
    {
		int[] trust_score = new int[n+1]; // All values = 0

        // Calculate Indegree and Outdegree
        for (int[] edges: trust) // O(E)
        {
            trust_score[edges[0]]--; // Outdegree
            trust_score[edges[1]]++; // Indegree
        }
        
        // Diff of Indegree - Outdegree = N-1-0 = N-1
        for (int i = 1; i<=n ; i++) // O(V)
        {
            if (trust_score[i] == n-1)
                return i;
        }

        return -1;
    }
}
```
TC: O(V+E)
SC: O(V)