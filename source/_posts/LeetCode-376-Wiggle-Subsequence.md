title: LeetCode.376 Wiggle Subsequence
author: Matteo
date: 2019-08-16 18:20:25
tags:
---
[https://leetcode.com/problems/wiggle-subsequence/](https://leetcode.com/problems/wiggle-subsequence/)
##### 题意
* 求数组中最长子序列满足wiggle subsequence要求（即一大一小错开）
##### 解法
1. DFS
对数组进行循环，第一个数时下一个数可以比它大也可以比它小，之后依次递推。直接DFS肯定会TLE，需要记录中间结果
```java
class Solution {
    int dp[][];
    public int wiggleMaxLength(int[] nums) {
        //dp用来记录中间结果
        dp = new int[3][nums.length];
        int max = 0;
        for (int i = 0; i < nums.length; i++) {
            max = Math.max(max, 1 + search(nums, i, 0));
        }
        return max;
    }

    int search(int[] nums, int pos, int flag) {
        //flag -1, 0, 1分别表示下一个数比当前的小，大或小，大
        if (dp[flag + 1][pos] != 0) {
            return dp[flag + 1][pos];
        }
        int result = 0;
        for (int i  = pos; i < nums.length; i++) {
            if (flag == 0) {
                if (nums[i] > nums[pos]) {
                    result = Math.max(result, 1 + search(nums, i, -1));
                } else if (nums[i] < nums[pos]) {
                    result = Math.max(result, 1 + search(nums, i, 1));
                }
            } else if (flag == 1) {
                if (nums[i] > nums[pos]) {
                    result = Math.max(result, 1 + search(nums, i, -1));
                }
            } else if (flag == -1) {
                if (nums[i] < nums[pos]) {
                    result = Math.max(result, 1 + search(nums, i, 1));
                }
            }
        }
        dp[flag + 1][pos] = result;
        return result;
    }
}
```
2. DP
>状态表示：
对于每一个数nums[i],以它为结尾的摆动序列有两种情况
（1） 最后以‘下降沿’的方式到达nums[i]
（2） 最后以‘上升沿’的方式达到nums[i]
我们使用 dp[i][0] 表示： 最后以‘下降沿’的方式到达nums[i]的摆动序列的最大长度
我们使用 dp[i][1] 表示： 最后以‘上升沿’的方式到达nums[i]的摆动序列的最大长度
初始状态： dp[i][0] = dp[i][1] = 1;
状态转移：
由上述可知：dp[i][1] ( 或dp[i][0] ) 的值可以由前面所有比他小（或大）的数更新
dp[i][0] = max( dp[j][1] + 1 ) ( 0 <= j < i && a[i] < a[j] )
dp[i][1] = max( dp[j][0] + 1 ) ( 0 <= j < i && a[i] > a[j] )
[https://blog.csdn.net/STILLxjy/article/details/83084608](https://blog.csdn.net/STILLxjy/article/details/83084608)
* 假如从最后一个数往前倒推，那么之前所有的结果都是未知的，那么只能是DFS这种解法。假如从第一个数开始往前倒推，那么求下一个数时，前面的结果都是已知的。
```java
class Solution {
    public int wiggleMaxLength(int[] nums) {
        int n = nums.length;
        if(n == 0) return 0;
        int ans = 1;
        int dp[][] = new int[n][2];
        dp[0][0] = 1; dp[0][1] = 1;
        for(int i=1;i<n;i++)
        {
            dp[i][0] = 1; dp[i][1] = 1;
            for(int j=i-1;j>=0;j--)
            {
                if(nums[i] > nums[j])
                {
                    dp[i][1] = Math.max(dp[i][1], dp[j][0]+1);
                }
                else if(nums[i] < nums[j])
                {
                    dp[i][0] = Math.max(dp[i][0], dp[j][1]+1);
                }
                else if(nums[i] == nums[j])
                {
                    dp[i][0] = Math.max(dp[i][0],dp[j][0]);
                    dp[i][1] = Math.max(dp[i][1],dp[j][1]);
                }
            }
            ans = Math.max(ans, dp[i][0]);
            ans = Math.max(ans, dp[i][1]);
        }
        return ans;
    }
}
```