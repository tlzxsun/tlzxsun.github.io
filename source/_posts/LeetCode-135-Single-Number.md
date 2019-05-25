title: LeetCode.135 Single Number
author: Matteo
tags: []
categories:
  - LeetCode
date: 2019-05-20 09:16:00
---
##### 题意
* 一次循环找出数组中唯一一个只出现一次的数字
* 直接放别人的解吧，好精妙https://blog.csdn.net/Cloudox_/article/details/52459584 
```java
public int singleNumber(int[] nums) {
        int result = 0;
        for(int i=0;i<nums.length;i++) result ^= nums[i];
        return result;
}
```
##### 解释
> 四行代码就解决了，我们看看它做了什么，先设定一个0，然后循环和数组中的每个数去做异或运算，每次得出的结果都继续和下一个数再异或。最后得出来的结果就是单个的那个数了！为什么？我演算了一下希望找到规律，发现确实如此。因为异或这个运算有三个很重要的特性：
> 1. 两个相同的数异或后为0；
> 2. 0和一个数异或后为那个数；
> 3. 异或运算满足交换律。
那么我们用0去依次和数组中的数进行异或，结果再继续和下一个数异或，一遍下来，每个数字都异或到了，交换律一遍，就是让每两个相同的数字都自己跟自己异或，结果都是0，然后0和那个单独的数字异或，结果就是那个单独的数字！拍案叫绝！ 