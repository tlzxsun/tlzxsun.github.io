title: LeetCode.343 Integer Break
author: Matteo
date: 2019-07-16 17:18:36
tags:
---
[https://leetcode.com/problems/integer-break/](https://leetcode.com/problems/integer-break/)
##### 题意
* 将一个整数n分为若干数，求这些数的最大乘积
##### 思路
* BFS
```java
class Solution {

    Map<Integer, Integer> cache = new HashMap<>();

    public int integerBreak(int n) {
        if (n == 1) {
            return 1;
        }
        if (cache.containsKey(n)) {
            return cache.get(n);
        }
        int max = 0;
        for (int i = 1; i <= n; i++) {
            max = Math.max(max, i * Math.max(n - i, integerBreak(n - i)));
        }
        cache.put(n, max);
//        System.out.println(n + "," + max);
        return max;
    }
}
```
* 动态规划
>我们可以先开辟一个数组dp[n+1]，保证每个dp[i]保存对应数字i的最大拆分结果
那么，我们通过遍历i之前的所有最大拆分结果dp[j]，结果dp[i] = Math.max( dp[i], dp[j] * (i-j) );
看似思路很对，但里面还有一个坑。我们有两个特殊的数字需要处理：2和3。dp[2] = 1和dp[3] = 2，这两个数字是两个最大拆分结果小于本身的数字，所以如果j为2或3，我们不如使用j本身，反而比使用dp[j]更大。
https://blog.csdn.net/blueblueskyz/article/details/80766488
```java
public int integerBreak(int n) {
        int[] dp = new int[n+1];
        dp[1] = 1;
        dp[2] = 1;

        for(int i = 3; i <= n; i++) {
            for(int j = 1; j < i; j++) {
                int diff;
                if( j == 2 || j == 3) {
                    diff = j * (i-j);
                } else {
                    diff = dp[j] * (i-j);
                }
                dp[i] = Math.max(dp[i], diff);
            }
        }
        return dp[n];
    }
```