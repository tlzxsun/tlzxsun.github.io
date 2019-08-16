title: LeetCode.233 Number of Digit One
author: Matteo
date: 2019-06-05 17:16:05
tags:
---
[https://leetcode.com/problems/number-of-digit-one/](https://leetcode.com/problems/number-of-digit-one/)
##### 题意
* 求1到n之间所有数中1的出现次数
##### 解法
* 先计算出位数与n相同且为10的倍数的数，根据n的最大位digit分为两种情况
1. digit == 1，刚f(n) = 1 + n%k + f(n%k) + f(k-1)，即以1开头位数与n相同的数共有n%k+1个，再加上n%k里1的出现次数再加上0到k-1里1的出现次数即是最后结果
2. digit != 1，则f(n) = f[k, n] + f(k-1)，即结果为k到n的数里1的出现次数加上0到k-1里1的出现次数
2.1 其中f[k, n] = k + (digit-1) * f(k-1) + f(n%k)，因为digit大于1，所以1...000到1...999肯定在n的范围内，这里以1开关的数共用k个，从2...000到digit...000之前有(digit - 1) * k-1个不以1开头的数，即他们的结果为(digit-1) * f(k-1)，再加上digit...000到n之前的数即n%k中包含的1的数量
```java
class Solution {
    public int countDigitOne(int n) {
        if (n == 0) {
            return 0;
        } else if (n < 10) {
            return 1;
        }
        long k = 1;
        while (k <= n) {
            k *= 10;
        }
        k/=10;
        int digit = (int) (n/k);
        if (digit > 1) {
            return (int) (k + (digit - 1) * countDigitOne((int) (k - 1)) + countDigitOne((int) (n%k)) + countDigitOne((int) (k - 1)));
        } else {
            return (int) (1 + n%k + countDigitOne((int) (n%k)) + countDigitOne((int) (k - 1)));
        }
    }
}
```