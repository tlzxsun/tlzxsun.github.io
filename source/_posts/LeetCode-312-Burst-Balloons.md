title: LeetCode.312 Burst Balloons
author: Matteo
date: 2019-06-24 15:43:08
tags:
---
[https://leetcode.com/problems/burst-balloons/](https://leetcode.com/problems/burst-balloons/)
##### 题意
* 给你一个数组,你要爆掉所有数字，爆某个数字的时候会获得它现在相邻的两个数字和它的乘积。如果这个数字没有左相邻或右相邻，则乘1代替，问爆掉所有数字的最高分数。
##### 思路
* 暴力计算的话复杂度为O(n!)肯定会超时
* 看到别人的解法是区间dp
>dp[i][j]表示只爆掉[i,j]里所有数字后能获得的最大分数,那么我们枚举[i,j]里最后一个爆掉的数字的位置k.
由于我们只爆掉[i,j]里的元素,因此最后爆掉k的时候,k的左右邻居一定是i-1和j+1.
初始化为:dp[i][i]=arr[i-1]*arr[i]*arr[i+1]
递推式为:dp[i][j]=max(dp[i][k-1]+dp[k+1][j]+arr[i-1]*arr[k]*arr[j+1])
最后答案为:dp[0][size-1]
[https://blog.csdn.net/ljhandlwt/article/details/52914010](https://blog.csdn.net/ljhandlwt/article/details/52914010)
```java
class Solution {
    public int maxCoins(int[] nums) {
        int []n = new int[nums.length + 2];
        n[0] = 1;
        n[n.length - 1] = 1;
        System.arraycopy(nums, 0, n, 1, nums.length);
        int[][] dp = new int[n.length][n.length];
        return divid(n, dp, 0, n.length - 1);
    }

    int divid(int[] nums, int[][] dp, int low, int high) {
        if (low + 1 == high) {
            return 0;
        }
        if (dp[low][high] > 0) {
            return dp[low][high];
        }
        int ans = 0;
        for (int i = low + 1; i < high; i++) {
            ans = Math.max(ans, nums[low]*nums[i]*nums[high] + divid(nums, dp, low, i) + divid(nums, dp, i, high));
        }
        dp[low][high] = ans;
        return ans;
    }
}
```