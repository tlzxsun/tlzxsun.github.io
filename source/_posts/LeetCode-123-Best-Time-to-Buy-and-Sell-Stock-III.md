title: LeetCode.123 Best Time to Buy and Sell Stock III
author: Matteo
tags: []
categories:
  - LeetCode
date: 2019-05-15 18:27:00
---
[https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/)
##### 思路
1. 暴力解法就是在121的基础上加个二分计算，能AC但是需要447ms，时间复杂度O(2$n^2$)
```java
class Solution {

    public int maxProfit(int[] prices, int start, int end) {
        int min = Integer.MAX_VALUE, max = 0;
        for (int i = start; i < end; i++) {
            //记录到截止当前位置最小的值
            min = Math.min(min, prices[i]);
            //最大值为当前结果与max中取大的
            max = Math.max(prices[i] - min, max);
        }
        return max;
    }

    public int maxProfit(int[] prices) {
        //先计算未二分时结果
        int max = maxProfit(prices, 0, prices.length);
        //依次二分比较
        for(int i = 2; i <= prices.length - 2; i++) {
            max = Math.max(max, maxProfit(prices, 0, i) + maxProfit(prices, i, prices.length));
        }
        return max;
    }
}
```
2. 后来想法就是依次计算每个位置到后面位置的最大值，并记下从0到当前位置的最大值，时间复杂度为O($n^2$)，AC时间变223ms差不多刚好一半～，还是太慢
```java
class Solution {
    public int maxProfit(int[] prices) {
        int result = 0;
        //记录从0到i位置的最大值
        int max[] = new int[prices.length];
        for (int i = 0; i < prices.length - 1; i++) {
            //m表示从i到j位置的最大值
            int m = 0;
            for (int j = i + 1; j < prices.length; j++) {
                m = Math.max(m, prices[j] - prices[i]);
                max[j] = Math.max(max[j], m);
            }
            if (i == 0) {
                //第一行即0到末尾的最大值
                result = m;
            } else {
                //否则需要判断
                result = Math.max(result, m + max[i - 1]);
            }
        }
        return result;
    }
}
```
3. 精妙答案，无法理解，也可以这么理解吧（~~四个变量，分别表示第一次买完，第一次卖完，第二次买完，第二次卖完后手上的钱~~）
```java
class Solution {
    public int maxProfit(int[] prices) {
        if(prices == null || prices.length == 0) {
            return 0;
        }
        int buy1 = Integer.MIN_VALUE;
        int sell1 = 0;
        int buy2 = Integer.MIN_VALUE;
        int sell2 = 0;
        for(int i = 0; i < prices.length; i++) {
            buy1 = Math.max(buy1, -prices[i]);
            sell1 = Math.max(sell1, prices[i] + buy1);
            buy2 = Math.max(buy2, sell1 - prices[i]);
            sell2 = Math.max(sell2, prices[i] + buy2);
        }
        return sell2;
    }
}
```