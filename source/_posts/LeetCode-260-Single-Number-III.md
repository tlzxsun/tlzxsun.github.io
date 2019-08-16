title: LeetCode.260 Single Number III
author: Matteo
date: 2019-06-10 17:42:30
tags:
---
[https://leetcode.com/problems/single-number-iii/](https://leetcode.com/problems/single-number-iii/)

##### 题意
* 数组中除了两个数出现一次，其它的数都出现两次，求解要求满足时间复杂度O(n)，空间复杂度O(1)
##### 解法
* 所有的数异或的结果就是两个数异或的结果，因为两个相同的数异或肯定是0，而0跟任何数异或都是那个数本身
* 最终结果转换成二进制的结果中为1的位，必然有其中一个数该位为1，另一个数该位为0，只有0，1异否才是1
* 将数组中的所有数与二进制中仅一位为1的数resInt异或，那么所有结果为0的数异或就是其中一个数，为1的是另外一个数。此时不需要考虑出现两次的数，因为他们与restInt的异或结果相同，两次异或之后结果为0
* A&-A = A原码的最后一个1代表的数字。[https://blog.csdn.net/zsjwish/article/details/80041665](https://blog.csdn.net/zsjwish/article/details/80041665)
```java
class Solution {
    public int[] singleNumber(int[] nums) {
        int[] res = new int[2];
        int resInt = 0;
        for (int num : nums) {
            resInt ^= num;
        }
        resInt &= -resInt;
        for (int num : nums) {
            if ((resInt & num) == 0) {
                res[0] ^= num;
            } else {
                res[1] ^= num;
            }
        }
        System.out.println(Arrays.toString(res));
        return res;
    }
}
```