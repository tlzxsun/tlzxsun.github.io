title: LeetCode.306 Additive Number
author: Matteo
date: 2019-06-21 15:21:11
tags:
---
[https://leetcode.com/problems/additive-number/](https://leetcode.com/problems/additive-number/)
##### 题意
* 求解一个字符串是否是类似fibonacci，即每个数是否前两个数构成
##### 解法
* 看不懂题意～～看懂以后发现好像并不难，先求出前两个数，再往后推算即可
```java
class Solution {

    public boolean isAdditiveNumber(String num) {
        int len = num.length();
        //先取第一个数
        for (int i = 1; i <= len; i++) {
            //从第一个数之后取第二个数
            for (int j = i + 1; j <= len - 1; j++) {
                String s1 = num.substring(0, i);
                String s2 = num.substring(i, j);
                //假如有数0开头且位数大于1那么不是一个合法的数，跳过
                if ((s1.startsWith("0") && s1.length() > 1) || (s2.startsWith("0") && s2.length() > 1)) {
                    continue;
                }
                //递归求解是满足要求
                if (dfs(s1, s2, num.substring(s1.length() + s2.length()))) {
                    return true;
                }
            }
        }
        return false;
    }

    boolean dfs(String s1, String s2, String s3) {
        //大数加求s1,s2的和
        String s = addStr(s1, s2);
        int len = s.length();
        String rest = s3.substring(0, len > s3.length()?s3.length():len);
        //假如s3刚好等于s1+s2，那么符合条件
        if (len == s3.length() && rest.equals(s)) {
            return true;
            //如果s3的长度小于s1+s2，那么不符合条件
        } else if (s3.length() < len || !rest.equals(s)) {
            return false;
        } else {
            rest = s3.substring(len);
            //继续递归求解
            return dfs(s2, s, rest);
        }
    }

    String addStr(String s1, String s2) {
        String str = "";
        int cf = 0, n = 0;
        int i = 0, i1 = s1.length() - 1, i2 = s2.length() - 1;
        while (i1 >=0 || i2 >= 0) {
            if (i1 >= 0 && i2 >= 0) {
                n = (s1.charAt(i1--) - '0') + (s2.charAt(i2--) - '0') + cf;
            } else if (i1 >= 0 && i2 < 0) {
                n = (s1.charAt(i1--) - '0') + cf;
            } else if (i2 >= 0 && i1 < 0) {
                n = (s2.charAt(i2--) - '0') + cf;
            }
            str = str + n%10;
            cf = n/10;
        }
        if (cf > 0) {
            str += "1";
        }
        return new StringBuilder(str).reverse().toString();
    }
}
```