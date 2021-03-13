## 997. Find the Town Judge

### GTA tag
![img](../gta.png)

### AC record for the day
![img](./Leetcode997.png)

### Runtime Screenshot
![img](./runtime997.png)

### Problem

*Easy*
[leetcode link here](https://leetcode.com/problems/find-the-town-judge/)

In a town, there are N people labelled from 1 to N.  There is a rumor that one of these people is secretly the town judge.

If the town judge exists, then:

The town judge trusts nobody.
Everybody (except for the town judge) trusts the town judge.
There is exactly one person that satisfies properties 1 and 2.
You are given trust, an array of pairs trust[i] = [a, b] representing that the person labelled a trusts the person labelled b.

If the town judge exists and can be identified, return the label of the town judge.  Otherwise, return -1.

**Example 1:**

```
Input: N = 2, trust = [[1,2]]
Output: 2
```

**Example 2:**

```
Input: N = 4, trust = [[1,3],[1,4],[2,3],[2,4],[4,3]]
Output: 3
```

### Solution

- Fancy problem but actually it is a graph problem. Once you have the trusted relationships, you can create a directed graph with each node from 1 to N, [i, j], i node point to j. indegree and outdegree. We want to find out one node has n-1 indegree but 0 outdegree. 
- I have one mistake here. I thought there could have multiple judge, which is why I'm using the dicitonary and hashset at first. Actually there cannot have more than one judge, imagine if they are two judge, they would have to truste eash other, which then they are not judge.
- we can build two array to solve such and then walk from 1 to N to find out ingree is n-1, outdegree is 0 node.
- we can also just use one array, ingree-outdegree == n-1

### Complexity Analysis

- Time complexity: O(N) 
- Space complexity: O(1). 

### Code

```c#
public class Solution {
    public int FindJudge(int N, int[][] trust) {
        var indegree = new int[N+1];
        var outdegree = new int[N+1];
        foreach (var t in trust) {
            indegree[t[1]]++;
            outdegree[t[0]]++;
        }
        
        for (int i=1; i<=N; i++) {
            if (indegree[i] == N-1 && outdegree[i] == 0) {
                return i; // we can have only one such judy, imagine if we have two, they would have to trust each other, then it is impossible
            }
        }
        return -1;
    }
}
```

```C#
public class Solution {
    public int FindJudge(int N, int[][] trust) {
        if (N < 0) return -1;
        if (N == 1) return 1;
        
        var ingree = new Dictionary<int, HashSet<int>>();
        var outgree = new Dictionary<int, HashSet<int>>();
        
        foreach (var t in trust) {
            if (!outgree.ContainsKey(t[0])) {
                outgree[t[0]] = new HashSet<int>();
            }
            if (!ingree.ContainsKey(t[1])) {
                ingree[t[1]] = new HashSet<int>();
            }
            outgree[t[0]].Add(t[1]);
            ingree[t[1]].Add(t[0]);
        }
        
        var candidate = new HashSet<int>();
        foreach (var item in ingree.OrderByDescending(x => x.Value.Count)) {
            if (item.Value.Count == (N-1) ) {
                candidate.Add(item.Key);
            }
        }
        if (candidate.Count == 0) return -1;
        var res = new List<int>();
        foreach (var item in candidate) {
            if (!outgree.ContainsKey(item)) {
                res.Add(item);
            }
        }
        
        return res.Count == 1 ? res[0] : -1;
    }
}
```