title: LeetCode.309 Best Time to Buy and Sell Stock with Cooldown
author: Matteo
date: 2019-08-15 19:10:09
tags:
---
一段时间没做题了，发现真的没有天赋
##### 题意
* 还是买卖股票，不限次数，但是卖出后第二天不能做任何交易
##### 解法
* 感觉是dp吧
* 双重循环，i表示买入，j表示卖出，第j天时的最大收益为
1. 当天未交易的结果dp[j - 1]
2. 当天完成交易的结果prices[j] - prices[i] + dp[i-2]
3. 非i天买入，j天时的结果dp[j]
* 3种情况取最大值
```java
class Solution {
    public int maxProfit(int[] prices) {
        if (prices.length < 2) {
            return 0;
        }
        int dp[] = new int[prices.length];
        for (int i = 0; i < prices.length - 1; i++) {
            for (int j = i + 1; j < prices.length; j++) {
                dp[j] = Math.max(Math.max(dp[j], dp[j - 1]), (prices[j] - prices[i]) + (i > 1?dp[i-2]:0));
            }
        }
        return dp[prices.length - 1];
    }
}
```