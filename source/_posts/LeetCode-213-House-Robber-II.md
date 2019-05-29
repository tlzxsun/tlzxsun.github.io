title: LeetCode.213 House Robber II
author: Matteo
date: 2019-05-29 19:17:08
tags:
---
[https://leetcode.com/problems/house-robber-ii/submissions/](https://leetcode.com/problems/house-robber-ii/submissions/)
##### 题意
* 与198基本一样，专业劫匪去打劫一排房子，由于打劫相邻的房子会触发警报，因此只能打劫不相邻的房子，并且房子围成了圈，所以不能同时打劫首尾的房子，求能打劫的最大钱数。
##### 解法
* 动态规划，很容易得到动态方程dp[i] = Math.max(nums[i] + dp[i-2], dp[i-1])，即当前位置最大钱为打劫到前两栋加上当前的钱与打劫到前一栋的最大值，由于是i-2，因此dp数组大小定义为nums.length+2。
* 由于首尾的房子不能同时打劫，因此分开计算不包含第一间和最后一间的结果，取两者最大值。
```java
class Solution {
    public int rob(int[] nums) {
        if (nums.length == 1) {
            return nums[0];
        }
        int dp1[] = new int[nums.length + 1];
        for (int i = 2; i < dp1.length; i++) {
            dp1[i] = Math.max(dp1[i - 2] + nums[i - 2], dp1[i-1]);
        }

        int dp2[] = new int[nums.length + 1];
        for (int i = 2; i < dp2.length; i++) {
            dp2[i] = Math.max(dp2[i - 2] + nums[i - 1], dp2[i-1]);
        }
        return Math.max(dp1[nums.length], dp2[nums.length]);
    }
}
```