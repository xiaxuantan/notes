### 485. Max Consecutive Ones

给定一个只含0或者1的数组，求最多有多少个连续的1。

```python
class Solution:
    def findMaxConsecutiveOnes(self, nums: List[int]) -> int:
        cur = ans = 0
        for i in range(len(nums)):
            cur = (cur + nums[i]) * nums[i]
            ans = max(ans, cur)
        return ans
```

### 487. Max Consecutive Ones II

给定一个只含0或者1的数组，可以把其中至多一个0变成1，问最多有多少个连续的1。

```python
class Solution:
    def findMaxConsecutiveOnes(self, nums: List[int]) -> int:
        ans = cur = i = 0
        last = -1
        for i in range(len(nums)):
            if nums[i] == 1:
                cur += 1
            else:
                last = cur
                cur = 0
            ans = max(ans, cur + last + 1)
        return ans
```

### 1004. Max Consecutive Ones III

给定一个只含0或者1的数组，可以把其中至多K个0变成1，问最多有多少个连续的1。

**Solultion**

比较经典的滑动窗口问题，尾指针每次都滑动一位，一旦发现K次0->1的机会都用完，就开始滑动头指针。

```python
class Solution:
    def longestOnes(self, A: List[int], K: int) -> int:
        head = tail = ans = 0
        while tail < len(A):
            if A[tail] == 0:
                K -= 1
            while K < 0:
                if A[head] == 0:
                    K += 1
                head += 1
            ans = max(ans, tail - head + 1)
            tail += 1
        return ans
```

