title: LeetCode.152 Maximum Product Subarray
author: Matteo
date: 2019-05-25 18:02:37
tags:
---
[https://leetcode.com/problems/maximum-product-subarray/](https://leetcode.com/problems/maximum-product-subarray/)
##### 题意
* 求一个数组中子序列的最大乘积
##### 解法
1. 暴力求解，复杂度O($ n^2$)
2. 动态规划：由于负负得正，因此需要定义一个dp[n][2]，其中dp[i][0]用来记录到i位置时的最小乘积，dp[i][1]记录到i到的最大乘积，得到如下动态转移方程，时间复杂度为O(n)，提交后发现排名非常靠后，肯定有更好的解法
```java
dp[i][0] = Math.min(Math.min(dp[i - 1][0] * nums[i], dp[i - 1][1] * nums[i]), nums[i]);
dp[i][1] = Math.max(Math.max(dp[i - 1][0] * nums[i], dp[i - 1][1] * nums[i]), nums[i]);
```
代码如下
```java
class Solution {
    public int maxProduct(int[] nums) {
        int dp[][] = new int[nums.length][2];
        int max = nums[0];
        dp[0][0] = nums[0];
        dp[0][1] = nums[0];
        for (int i = 1; i < nums.length; i++) {
            dp[i][0] = Math.min(Math.min(dp[i - 1][0] * nums[i], dp[i - 1][1] * nums[i]), nums[i]);
            dp[i][1] = Math.max(Math.max(dp[i - 1][0] * nums[i], dp[i - 1][1] * nums[i]), nums[i]);
            max = Math.max(max, dp[i][1]);
        }
        return max;
    }
}
```
3. 查看了排名最靠前的解法，遍历数组，依次相乘，记下最大值，当遇到0刚重新开始计算，然后从后往前再计算一遍，再取两次结果的最大值。大概想了下
  3.1. 0的情况比较简单，即0之前部分，0和0之后部分乘积比较
  不考虑0的情况下
  3.2.1 偶数个负数的情况，所有的负数肯定会参与运算
  3.2.2 奇数个负数的情况，乘积最大化的情况只能是排除头尾负数的情况
时间复杂度也为O(n)
```java
class Solution {

    public int maxProduct(int[] nums) {

        int max = 1;
        int result = nums[0];
        for(int i = 0; i < nums.length; i++){
            max = max * nums[i];
            if(result < max) {
                result = max;
            }
            if(max == 0) {
                max = 1;
            }
        }

        max = 1;
        int result2 = nums[nums.length -1];
        for(int i = nums.length -1; i>0;i--){
            max = max * nums[i];
            if(result2 < max) {
                result2 = max;
            }
            if(max == 0){
                max = 1;
            }
        }

        return Math.max(result,result2);
    }

}
```
4. 比较奇怪的是动态规划明明只进行了一次遍历，但是却比3.的解法更快，可能是由于动态规划每次遍历需要进行2次乘法和5次比较，而下面的3.的遍历只须进行1次乘法和2次比较