title: LeetCode.377 Combination Sum IV
author: Matteo
date: 2019-08-15 19:11:35
tags:
---
[https://leetcode.com/problems/combination-sum-iv/](https://leetcode.com/problems/combination-sum-iv/)
##### 题意
* 给一个数组，求结果为target的组合数，数字可重复
##### 解法
1. DFS，直接算会TLE，缓存中间结果后可以通过
```java
class Solution {
    HashMap<Integer, Integer> cache = new HashMap<>();
    public int combinationSum4(int[] nums, int target) {
        if (target == 0) {
            return 1;
        } else if (target < 0) {
            return 0;
        }
        if (cache.containsKey(target)) {
            return cache.get(target);
        }
        int result = 0;
        for (int i = 0; i < nums.length; i++) {
            result += combinationSum4(nums, target - nums[i]);
        }
        cache.put(target, result);
        return result;
    }
}
```
2. 动态规划
* 问题可以转换为求[1, target]之间每个位置有多少种排列方式, 这样将问题分化为子问题. 状态转移方程可以得到为:
> dp[i] = sum(dp[i - nums[j]]),  (i-nums[j] > 0);
* 边界值为dp[0] = 1，即target为0时有一种情况，即一个数也不选
```java
class Solution {
    public int combinationSum4(int[] nums, int target) {
        int[] dp=new int[target+1];
        dp[0]=1;
        for(int i=1;i<=target;i++)
        {
            for(int j=0;j<nums.length;j++)
            {
                if(i>=nums[j])
                    dp[i]=dp[i]+dp[i-nums[j]];
            }
        }
        return dp[target];
    }
}
```