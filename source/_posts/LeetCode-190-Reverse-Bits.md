title: LeetCode.190 Reverse Bits
author: Matteo
date: 2019-05-29 16:52:03
tags:
---
[https://leetcode.com/problems/reverse-bits/](https://leetcode.com/problems/reverse-bits/)
##### 题意
* 整数按位翻转
##### 解法
> 由于限制位数为32位，所以只需对待处理的整数n进行32次右移位，每当低位&1的结果为1，说明低位为1，此时将待输出的目标整数(默认值为0)左移动一位并加上1；每当低位&1的结果为0，说明低位为0，此时将待输出的目标整数左移一位即可；循环直到移动完32次，所得目标整数即为所求。
[https://blog.csdn.net/pistolove/article/details/46868017](https://blog.csdn.net/pistolove/article/details/46868017)

```java
class Solution {
    public int reverseBits(int n) {
        int value = 0;
        // 32位无符号数
        for (int i = 0; i < 32; ++i) {
            //最后一位为1，n右移，value左移并加1，即n的31-i位即value中的i位
            if ((n & 1) == 1) {
                value = (value << 1) + 1; // 左移动
                n >>= 1;
            } else {
                value = value << 1;
                n >>= 1; // 右移
            }
        }
        return value;
    }
}
```