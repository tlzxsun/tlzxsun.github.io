title: LeetCode.300 Longest Increasing Subsequence
author: Matteo
date: 2019-06-20 11:27:13
tags:
---
[https://leetcode.com/problems/longest-increasing-subsequence/](https://leetcode.com/problems/longest-increasing-subsequence/)
##### 题意
* 求序列中最长可不连接递增序列😂
##### 解法
1. 双重遍历，复杂度$O(n^2)$
2. 动态规划，以nums[i]结束的最大递增序列，即从nums[i]向前遍历，如果nums[j]小于nums[i]那么nums[i]=Math.max(nums[i], nums[j] + 1)
```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        int max = 1;
        int dp[] = new int[nums.length];
        Arrays.setAll(dp, new IntUnaryOperator() {
            @Override
            public int applyAsInt(int operand) {
                return 1;
            }
        });
        for (int i = 0; i < nums.length; i++) {
            for (int j = i - 1; j >= 0; j--) {
                if (nums[i] > nums[j]) {
                    dp[i] = Math.max(dp[i], dp[j] + 1);
                }
                max = Math.max(max, dp[i]);
            }
        }
        return max;
    }
}

```