## 1481. Least Number of Unique Integers after K Removals

### GTA tag
![img](../gta.png)

### AC record for the day
![img](./leetcode1481.png)

### Runtime Screenshot
![img](./runtime0312.png)

### Problem

*Medium*
[leetcode link here](https://leetcode.com/problems/least-number-of-unique-integers-after-k-removals/)

Given an array of integers arr and an integer k. Find the least number of unique integers after removing exactly k elements.


**Example 1:**

```
Input
Input: arr = [5,5,4], k = 1
Output: 1
Explanation: Remove the single 4, only 5 is left.
```

**Example 2:**

```
Input: arr = [4,3,1,1,3,3,2], k = 3
Output: 2
Explanation: Remove 4, 2 and either one of the two 1s or three 3s. 1 and 3 will be left.
```

### Solution

- This is an easy problem. Use dictionary to save the item value and its frequency (the count of that number). We want to get the least number of unique items left after the removal, we would need to remove the ones that has least frequency in the list. When doing the removal, we can count the ones that being removed or we can use hashset to save te ones that get removed from the dictionary. We don't want to do it at the dictionary traversal.

### Complexity Analysis

- Time complexity: O(NlgN) because we use dictionary orderBy.
- Space complexity: O(N). we use a dictionary.

### Code

```c#
public class Solution {
    public int FindLeastNumOfUniqueInts(int[] arr, int k) {
        var dict = new Dictionary<int, int>();
        foreach (var item in arr) {
            if (!dict.ContainsKey(item)) {
                dict[item] = 1;
            } else {
                dict[item]++;
            }
        }
        
        int count = 0;
        int m = 0;
        foreach (var item in dict.OrderBy(x => x.Value)) {
            m++;
            count += item.Value;
            if (count == k) break;
            if (count > k) {
                m--;
                break;
            }
        }
        
        return dict.Count - m;
    }
}
```