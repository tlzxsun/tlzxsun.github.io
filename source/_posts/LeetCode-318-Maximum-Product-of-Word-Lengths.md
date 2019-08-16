title: LeetCode.318 Maximum Product of Word Lengths
author: Matteo
date: 2019-07-08 19:19:45
tags:
---
[https://leetcode.com/problems/maximum-product-of-word-lengths/](https://leetcode.com/problems/maximum-product-of-word-lengths/)
##### 题意
* 求字符串数组中不含相同字母的字符串的长度的乘积的最大值（好拗口～～）
##### 思路
* 由于只有26个字母，因此可以用一个int的每一位表示该字符串是否含有某个字符，两个数&的结果为0则表示没有相同的字母
* 开始以为暴力求解两两相乘的话时间复杂度为O($n^2$)，可能会有更好的解法，结果好像并没有
```java
//将 整数 num 的第 i 位的值 置为 1
    private static int getBit(int num, int i)
    {
        return (num | (1 << i));
    }
```
```java
//将 整数 num 的第 i 位的值 置为 0
    private static int getBit(int num, int i)
    {
       int mask = ~(1 << i);//000100
       return (num & (mask));//111011
    }
```
##### 解法
```java
public class Solution{
    public int maxProduct(String[] words){
        if(words == null || words.length == 0)
            return 0;
        int len = words.length;
        int[] num = new int[len];
        int maxProduct = 0;
        for(int i = 0; i < len; i++){
            String temp = words[i];
            for(int j = 0; j < temp.length(); j++){
                num[i] |= (1 << (temp.charAt(j) - 'a'));
            }
        }
        for(int i = 0; i < len; i++){
            for(int j = i + 1; j < len; j++){
                if((num[i] & num[j]) == 0){
                    int temp = words[i].length() * words[j].length();
                    if(temp > maxProduct)
                        maxProduct = temp;
                }
            }
        }
        return maxProduct;
    }
}
```