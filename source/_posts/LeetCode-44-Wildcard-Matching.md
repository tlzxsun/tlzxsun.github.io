title: LeetCode.44 Wildcard Matching
author: Matteo
tags: []
categories:
  - LeetCode
date: 2019-05-10 16:51:00
---
[https://leetcode.com/problems/wildcard-matching/](https://leetcode.com/problems/wildcard-matching/)
* 前几天做[https://leetcode.com/problems/interleaving-string/](https://leetcode.com/problems/interleaving-string/)的时候，开始用的递归，在最后一个测试用例时TLF。学习别人思路时看到一句话，所以字符串匹配的都用动态规划，要不然肯定TLF，很有道理的样子。因此这题一开始就没打算用递归。
* 以"adceb"和"\*a*b"为例讲下思路

| | |a|b|c|e|b|
|---|---|---|---|---|---|---|
|*|T|T|T|T|T|T|
|a|F|T|F|F|F|F|
|*|F|T|T|T|T|T|
|b|F|F|F|T|F|T|
二维数组r用来保存状态，r[i][j]表示该p[i]匹配到s[j-1]的状态，因此列数为s.length() + 1。
* 首先初始化第一行，根据p[0]有三种情况：
1. p[0]==\*  则该行均为true，因为*可以匹配0到任意个任意字符。
2. p[0]==?  则r[0][1]为true，其它为false，因为?只能匹配一个任意字符
3. p[0]==s[0] 则同p[0]==?
4. p[0]!=s[0] 首字母无法匹配，直接返回false
* 之后每行根据上一行状态判断，找到上一行中为true的位置j，根据p[i]分为三种情况
1. p[i]==\* 则从j起r[i][]均为true
2. p[i]==? 则r[i][j + 1]为true
3. p[i]==s[j] 则r[i][j + 1]为true

附上ac代码
```java
class Solution {
    public boolean isMatch(String s, String p) {
        //特殊情况处理
        if (p.length() == 0) {
            return s.length() == 0;
        }
        if (s.length() == 0) {
            return p.replace("*", "").length() == 0;
        }
        boolean r[][] = new boolean[p.length()][s.length() + 1];
        //初始化第一行
        if (p.charAt(0) == '*') {
            for (int i = 0; i <= s.length(); i++) {
                r[0][i] = true;
            }
        } else if (p.charAt(0) == '?') {
            r[0][1] = true;
        } else if (s.charAt(0) == p.charAt(0)) {
            r[0][1] = true;
        } else {
            return false;
        }
        for (int i = 1; i < p.length(); i++) {
            for (int k = 0; k <= s.length(); k++) {
                if (r[i - 1][k]) {
                    //分情况判断
                    if (p.charAt(i) == '*') {
                        for (int j = k; j <= s.length(); j++) {
                            r[i][j] = true;
                        }
                    } else {
                        if (k >= s.length()) {
                            break;
                        }
                        if (p.charAt(i) == '?') {
                            r[i][k + 1] = true;
                        } else if (s.charAt(k) == p.charAt(i)) {
                            r[i][k + 1] = true;
                        } else {
                            r[i][k + 1] = false;
                        }
                    }
                }
            }
        }
        return r[p.length() - 1][s.length()];
    }
}
```