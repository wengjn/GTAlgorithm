## 475. Heaters

### GTA tag
![img](../gta.png)

### AC record for the day
![img](./leetcode475.png)

### Runtime Screenshot
![img](./runtime475.png)

### Problem

*Medium*
[leetcode link here](https://leetcode.com/problems/heaters/)

Winter is coming! During the contest, your first job is to design a standard heater with a fixed warm radius to warm all the houses.

Every house can be warmed, as long as the house is within the heater's warm radius range. 

Given the positions of houses and heaters on a horizontal line, return the minimum radius standard of heaters so that those heaters could cover all houses.

Notice that all the heaters follow your radius standard, and the warm radius will the same.

**Example 1:**

```
Input: houses = [1,2,3], heaters = [2]
Output: 1
Explanation: The only heater was placed in the position 2, and if we use the radius 1 standard, then all the houses can be warmed.
```

**Example 2:**

```
Input: houses = [1,2,3,4], heaters = [1,4]
Output: 1
Explanation: The two heater was placed in the position 1 and 4. We need to use radius 1 standard, then all the houses can be warmed.
```

### Solution

- Initially my thought is to use BFS, expand to the left and right, but it would easily hit TLE, imagine one house is at 1 and the heater at 1000.
- two pointers approach. need to sort both arrays at the begining. The idea is to find the closest heater to each house and take the max of the closest distance. Traversersing the house, if house is between j-1 and j heater, easy to do. There are two corner cases, when house is located before 1st heater and when house is located after the last heater. At the corner case, they are the only validated distances.

### Complexity Analysis

- Time complexity: O(NlgN) because we sort.
- Space complexity: O(1).

### Code

```c#
public class Solution {
    public int FindRadius(int[] houses, int[] heaters) {
        if(houses == null || houses.Length == 0) return 0;
        Array.Sort(houses);
        Array.Sort(heaters);
        
        int ans = 0;
        int i  = 0;
        int j = 0;
        
        while (i < houses.Length){
            if(houses[i] <= heaters[j]){ //if house is located before heater j.
                if(j == 0){ // corner case when the heater is the first  one
                    ans = Math.Max(ans, heaters[j]-houses[i]);
                    i++;
                    continue;
                }
            } else { // if house is located after some heater, 
                while(j!=heaters.Length-1 && heaters[j]<houses[i]){ // then find a heater that stands after the house
                    j++;
                }
                if(j == heaters.Length-1){ // corner cases if j is 0 or there is no more heaters
                    ans = Math.Max(ans, houses[i]-heaters[j]);
                    i++;
                    continue;
                }
            }
            
            int dist = Math.Min(houses[i]-heaters[j-1], heaters[j]-houses[i]); // if house is located between jth and j-1th heaters
            ans = Math.Max(ans, dist);
            i++;
        }
        
        return ans;
    }
}
```

```c#
public class Solution {
    public int FindRadius(int[] houses, int[] heaters) {
        // BFS hit TLE
        var directions = new int[] { -1, 1 };
        var houseSet = new HashSet<int>(houses);
        var q = new Queue<int>();
        var visited = new HashSet<int>();
        foreach (var h in heaters) {
            q.Enqueue(h);
            if (houseSet.Contains(h)) {
                visited.Add(h);
            }
        }
        
        int level = 0;
        while (visited.Count < houses.Length) {
            level++;
            var size = q.Count;
            for (int i=0; i<size; i++) {
                var node = q.Dequeue();
                if (visited.Count == houses.Length) {
                    return level;
                }
                foreach (var dir in directions) {
                    var newx = node + dir;
                    q.Enqueue(newx);
                    if (houseSet.Contains(newx) && !visited.Contains(newx)) {
                        //Console.WriteLine(newx);
                        visited.Add(newx);
                    }
                }
            }
            
        }
        return level;
    }
}
```