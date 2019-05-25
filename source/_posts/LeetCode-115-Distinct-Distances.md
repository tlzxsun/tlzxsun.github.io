title: LeetCode.115 Distinct Distances
author: Matteo
tags: []
categories: []
date: 2019-05-14 14:29:00
---
[https://leetcode.com/problems/distinct-subsequences/](https://leetcode.com/problems/distinct-subsequences/)
* 开始时候想着递归做，后来想想肯定会超时，就放弃了。昨天刚做了八皇后问题，感觉跟这个比较像，先定确定一个位置，再继续向后查找，直到找到一个解。找到解后再回溯，写出来发现还是超时😞
* 先贴下超时的解
```java
class Solution {
    public int numDistinct(String s, String t) {
        int count = 0;
        //标记s和t的查找位置
        int sIndex = 0, tIndex = 0;
        List<Integer> list = new ArrayList<>();
        while (sIndex < s.length()) {
            //如果该位置字符相等则s和t均向后查找，并记下s中的位置
            if (s.charAt(sIndex) == t.charAt(tIndex)) {
                list.add(sIndex);
                sIndex++;
                tIndex++;
                //不相等相s继续向后查找
            } else {
                sIndex++;
            }
            //假如已经查找到t的结束，则结果加1，s回溯到上个查找到的位置加1
            if (tIndex == t.length()) {
                count++;
                sIndex = list.get(list.size() - 1) + 1;
                list.remove(list.size() - 1);
                tIndex--;
            }
            //假如s仍旧在末尾
            if (sIndex == s.length()) {
                if (tIndex ==  t.length()) {

                } else {
                    //继续回溯，直接找到一个可用位置
                    while (list.size() > 0 && sIndex >= s.length()) {
                        sIndex = list.get(list.size() - 1) + 1;
                        list.remove(list.size() - 1);
                        tIndex--;
                    }
                }
            }
        }
        return count;
    }
}
```
* 尝试动态规划解决
* 以rabbbit和rabbit为例先画一个动态规划表

|||r|a|b|b|b|i|t|
|-|-|-|-|-|-|-|-|-|
||1|1|1|1|1|1|1|1|
|r|0|1|1|1|1|1|1|1|
|a|0|0|1|1|1|1|1|1|
|b|0|0|0|1|2|3|3|3|
|b|0|0|0|0|1|3|3|3|
|i|0|0|0|0|0|0|3|3|
|t|0|0|0|0|0|0|0|3|

* 定义一个数组用来保存状态dp[t.length + 1][s.length + 1]，初始状态为dp[i][0] = 0, dp[0][j] = 1, 可以理解为如果T为空串，则为1，因为空串是任意字符串的子串。如果S为空，那么为0，因为空串不能包含一个非空字符串.
* 根据t[i]和s[j]有两种状态，~~这里虽然做出解来了但是一直没弄明白为什么这么做😞，很悲哀~~
  * t[i]==s[j], 则dp[i][j]=dp[i-1][j-1] + dp[i][j-1]，即如果当前字符相等，那么dp[i][j]就可以继承dp[i-1][j-1]的值，再加上没有s[j-1]这个字符的值（即s中除去当前字符去匹配t的结果再加上s和t中均去除当前字符的匹配结果。因为当前字符相等，假如之前的串能匹配的，加上当前字符后肯定也能匹配；如果s不需要当前字符能匹配的，加上当前字符后也肯定能匹配）
  * t[i]!=s[j]，则dp[i][j]=dp[i][j-1]，即如果当前的字符不相等，则dp[i][j]的值为S[0, j-1]去掉当前不匹配的这个字符的子串包含几个T[0, i-1]的子序列
* AC代码
```java
class Solution {

    int dp[][];
    public int numDistinct(String s, String t) {
        dp = new int[t.length() + 1][s.length() + 1];
        for (int i = 0; i <= s.length(); i++) {
            dp[0][i] = 1;
        }
        for (int i = 1; i <= t.length(); i++) {
            for (int j = 1; j <= s.length(); j++) {
                if (s.charAt(j - 1) == t.charAt(i - 1)) {
                    dp[i][j] = dp[i][j-1] + dp[i-1][j-1];
                } else {
                    dp[i][j] = dp[i][j-1];
                }
            }
        }
        return dp[t.length()][s.length()];
    }
}
```