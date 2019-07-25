### 198 House Robber

给定一个list里面有很多数，你可以pick这个list里面的若干个数，但条件是不能有两个数相邻，求最大的和。

**Solution**

从左往右遍历，对每个数，决策有两种，pick或者不pick，而且这两种决策只和前一个数的结果有关。这样只需要两个变量存着当前的和就行。时间复杂度O(N)，空间复杂度O(1)。

```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        yesterday_rob = 0
        yesterday_rest = 0
        for i in range(len(nums)):
            today_rob = yesterday_rest + nums[i]
            today_rest = max(yesterday_rest, yesterday_rob)
            yesterday_rob = today_rob
            yesterday_rest = today_rest
        return max(yesterday_rob, yesterday_rest)
```

### 213. House Robber II

和前面的题背景类似，唯一区别是这个list是一个环。也就是说，头和尾需要看做是相连的。

**Solution**

和前面的题解法类似，由于首和尾只能取其一，所以可以去掉都一个数扫一次，去掉末尾的数扫一次。

```python
class Solution:
    def rob(self, nums: List[int]) -> int:
    
        def rob_linear(nums):
            y_rob = 0
            y_rest = 0
            for i in range(len(nums)):
                t_rob = y_rest + nums[i]
                t_rest = max(y_rob, y_rest)
                y_rob = t_rob
                y_rest = t_rest
            return max(y_rob, y_rest)
        
        if not nums: return 0
        if len(nums) == 1: return nums[0]
        return max(nums[0], rob_linear(nums[:-1]), rob_linear(nums[1:]))
```

### 337. House Robber III

pick一棵二叉树上的若干个数，条件时不能有相邻的树节点被选中，求最大的和。

**Solution**

和前面的思路一样，分rob和rest两种情况考虑。然后做一个树的后序遍历。

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def rob(self, root: TreeNode) -> int:
        
        def rob_tree(root):
            if not root:
                return 0, 0
            l_rob, l_rest = rob_tree(root.left)
            r_rob, r_rest = rob_tree(root.right)
            rob = l_rest + r_rest + root.val
            rest = max(l_rob, l_rest) + max(r_rob, r_rest)
            return rob, rest
    
        return max(rob_tree(root))
```

