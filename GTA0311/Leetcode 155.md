## 155. Min Stack

### GTA tag
![img](../gta.png)

### AC record for the day
![img](./leetcode%20155.png)

### Runtime Screenshot
![img](./runtime%20155.png)

### Problem

*Easy*
[leetcode link here](https://leetcode.com/problems/min-stack/)

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

push(x) -- Push element x onto stack.
pop() -- Removes the element on top of the stack.
top() -- Get the top element.
getMin() -- Retrieve the minimum element in the stack.


**Example 1:**

```
Input
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]

Output
[null,null,null,null,-3,null,0,-2]

Explanation
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin(); // return -3
minStack.pop();
minStack.top();    // return 0
minStack.getMin(); // return -2
```

### Solution

- Read carefully, it needs all operation to be O(1). We can rule out to use search or heap. And we only get the min, not removing them from stack. Numbers in the stack can be the same, not necessary unique. Using built-in stack should be good.
- Two stacks approach. One to save all raw data, one to only save the min. If the same as min, also push.
- Improved one. if min is repetive, we only want to know the count of this min, no need to save it as a data in the stack. In this case, we have the min stack to keep a pair. item1 is the actual value and item2 is the count.

### Complexity Analysis

- Time complexity: O(1), this is the requirement
- Space complexity: O(N). we need two stacks here.

### Code

```c#
// two stacks approach
public class MinStack {
    private Stack<int> data;
    private Stack<int> min;

    /** initialize your data structure here. */
    public MinStack() {
        data = new Stack<int>();
        min = new Stack<int>();
    }
    
    public void Push(int x) {
        data.Push(x);
        if (min.Count > 0 && min.Peek() < x) {
            return;
        } else {
            min.Push(x);
        }
    }
    
    public void Pop() {
        if (data.Count == 0) return;
        var val = data.Pop();
        if (min.Count > 0 && min.Peek() == val) {
            min.Pop();
        }
    }
    
    public int Top() {
        return data.Peek();
    }
    
    public int GetMin() {
        return min.Peek();
    }
}
```

```c#
// this is the improved one. no need to keep pushing mins into min stack. just keep counting it
public class MinStack {
    private Stack<int> data;
    private Stack<int[]> min; // a pair in this min tracker stack, item1 is the value, item2 is the count

    /** initialize your data structure here. */
    public MinStack() {
        data = new Stack<int>();
        min = new Stack<int[]>();
    }
    
    public void Push(int x) {
        data.Push(x);
        if (min.Count == 0 || min.Peek()[0] > x) {
            min.Push(new int[] { x, 1 });
        } else if (min.Peek()[0] == x) {
            min.Peek()[1]++;
        }
    }
    
    public void Pop() {
        if (data.Count == 0) return;
        int val = data.Pop();
        if (min.Count > 0 && min.Peek()[0] == val) {
            min.Peek()[1]--;
            if (min.Peek()[1] == 0) {
                min.Pop();
            }
        }
    }
    
    public int Top() {
        return data.Peek();
    }
    
    public int GetMin() {
        return min.Peek()[0];
    }
}
```