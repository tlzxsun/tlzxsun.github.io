title: LeetCode.223 Rectangle Area
author: Matteo
date: 2019-06-04 14:28:38
tags:
---
[https://leetcode.com/problems/rectangle-area/](https://leetcode.com/problems/rectangle-area/)
##### 题意
* 求两个矩形所占总面积
##### 解法
* 两个矩形可能有三种情况
1. 相交，取两个矩形面积相加减去重叠面积
2. 不相交，返回两个矩形面积之和
3. 包含，取两上矩形面积中的最大值
计算出重叠面积后如果重叠面积等于其中一个的面积，说明两个矩形是包含关系。
虽然结果不出超出int范围，但中间过程中还是会溢出，所以先转成long计算
```java
class Solution {
    public int computeArea(int A, int B, int C, int D, int E, int F, int G, int H) {
        int s1 = (C - A) * (D - B);
        int s2 = (G - E) * (H - F);
        long maxL = Math.max(A, E);
        long maxB = Math.max(B, F);
        long minR = Math.min(C, G);
        long minT = Math.min(D, H);
        long width = minR - maxL;
        long height = minT - maxB;
        long lapover = 0;
        if (width > 0 && height > 0) {
            lapover = width * height;
        }
        if (lapover == s1 || lapover == s2) {
            return Math.max(s1, s2);
        } else {
            return (int)(s1 + s2 - lapover);
        }
    }
}
```