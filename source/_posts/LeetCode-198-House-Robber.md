title: LeetCode.198 House Robber
author: Matteo
date: 2019-05-29 18:26:30
tags:
---
[https://leetcode.com/problems/house-robber/](https://leetcode.com/problems/house-robber/)
##### 题意
* 专业劫匪去打劫一排房子，由于打劫相邻的房子会触发警报，因此只能打劫不相邻的房子，求能打劫的最大钱数
##### 解法
* 动态规划，很容易得到动态方程dp[i] = Math.max(nums[i] + dp[i-2], dp[i-1])，即当前位置最大钱为打劫到前两栋加上当前的钱与打劫到前一栋的最大值，由于是i-2，因此dp数组大小定义为nums.length+2
```java
class Solution {
    public int rob(int[] nums) {
        int dp[] = new int[nums.length + 2];
        for (int i = 2; i < dp.length; i++) {
            dp[i] = Math.max(dp[i - 2] + nums[i - 2], dp[i-1]);
        }
        return dp[nums.length + 1];
    }
}
```