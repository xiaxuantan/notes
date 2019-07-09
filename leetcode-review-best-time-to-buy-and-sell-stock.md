### 121. Best Time to Buy and Sell Stock

有一个数列，第i项代表第i天股票的价格。每一天可以做一次买入或者卖出，但某一时刻只可以持有一支股票。如果最多只能做一次交易(一次买入和卖出)，问能获得的最大利润。

**Solution**

因为只能做一次交易，所以只需要记得前面最低的股价就可以了。时间复杂度O(N)，空间复杂度O(1)。

```java
class Solution {
    public int maxProfit(int[] prices) {
        if (prices.length == 0) {
            return 0;
        }
        int ans = 0;
        int lowestPrice = prices[0];
        for (int i = 1; i < prices.length; i++) {
            if (prices[i] > lowestPrice) {
                ans = ans < prices[i] - lowestPrice ? prices[i] - lowestPrice : ans;
            } else {
                lowestPrice = prices[i];
            }
        }
        return ans;
    }
}
```

### 122. Best Time to Buy and Sell Stock II

上一题的背景，如果可以做多次交易(多次买入和卖出，但仍然只能持有一支股票)，问能获得的最大利润。

**Solution**

一个巧妙的解法，如果最优解中，在第i天买入，在第j天卖出，那么第i天到第j天的股票价格一定是递增的。证明：如果存在i < k < j，有stock(k) < stock(i) 。那么stock(k - 1) - stock(i) + stock(j) - stock(k)的收益会更大。由于是递增的，所以卖出 - 卖入 = 中间相邻两天相减之和。这样只需要扫一遍，时间复杂度O(N)，空间复杂度O(1)。

```java
class Solution {
    public int maxProfit(int[] prices) {
        int profit = 0;
        int cur = Integer.MAX_VALUE;
        for (int i = 0; i < prices.length; i++) {
            if (prices[i] > cur) {
                profit += (prices[i] - cur);
            }
            cur = prices[i];
        }
        return profit;
    }
}
```

### 123. Best Time to Buy and Sell Stock III

121题的背景，但最多只能做两次交易。

**Solution**

可以根据四种状态进行递推，第一次买入，第一次卖出，第二次买入，第二次卖出。

```python
 class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        y1, y2, y3, y4 = float('-inf'), 0, float('-inf'), 0
        for i in range(len(prices)):
            p = prices[i]
            t1 = max(y1, -p)
            t2 = max(y2, y1 + p)
            t3 = max(y3, y2 - p)
            t4 = max(y4, y3 + p)
            y1, y2, y3, y4 = t1, t2, t3, t4
        return max(y1, y2, y3, y4)
```

### 188. Best Time to Buy and Sell Stock IV

121题的背景，但最多只能做k次交易。

**Solution**

采用dp的思路，buy[t]表示前i天，买入第t次股票的最大收益，sell[t]表示前i天，卖出第t次股票的最大收益。这样可以用O(K)的空间复杂度，滚动算出最优解。时间复杂度O(N*K)。一个优化，如果K超出了可能的最大交易数，这道题退化成122，可以用O(N)的时间复杂度解出。

```java
class Solution {
    private int max(int x, int y) {
        return x > y ? x : y;
    }
    
    public int maxProfitLinear(int[] prices) {
        int ans = 0;
        for (int i = 1; i < prices.length; i++)
            if (prices[i] > prices[i - 1])
                ans += prices[i] - prices[i - 1];
        return ans;
    }
    
    
    public int maxProfit(int k, int[] prices) {
        if (k >= prices.length / 2) {
            return maxProfitLinear(prices);
        }
        int[] buy = new int[k + 1];
        int[] sell = new int[k + 1];
        int ans = 0;
        for (int i = 0; i <= k; i++) buy[i] = Integer.MIN_VALUE;
        for (int i = 0; i < prices.length; i++) {
            for (int t = k; t > 0; t--) {
                sell[t] = max(sell[t], buy[t] + prices[i]);
                buy[t] = max(buy[t], sell[t - 1] - prices[i]);
                ans = max(ans, sell[t]);
            } 
        }
        return ans;
    }
}

```

### 309. Best Time to Buy and Sell Stock with Cooldown

121的背景，但每卖出一次股票，接下来的一天必须休息，不能买入。

**Solution**

可以用三种状态进行递推，b表示最后一次操作是买入，s表示最后一次操作是卖出还未cooldown，r表示最后一次操作是卖出而且已经过了cooldown期。

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        b = float('-inf')
        s = r = 0
        for i in range(len(prices)):
            t_b = max(r - prices[i], b)
            t_s = b + prices[i]
            t_r = max(r, s)
            b, s, r = t_b, t_s, t_r
        return max(s, r)
```

### 714. Best Time to Buy and Sell Stock with Transaction Fee

121的背景，但每卖出一次股票，都需要收fee的手续费。

**Solution**

用sell、buy两个变量递推即可。

```python
class Solution:
    def maxProfit(self, prices: List[int], fee: int) -> int:
        b = float('-inf')
        s = 0
        for i in range(len(prices)):
            t_b = max(b, s - prices[i])
            t_s = max(s, b + prices[i] - fee)
            b, s = t_b, t_s
        return s

```

