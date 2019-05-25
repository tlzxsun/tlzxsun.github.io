title: LeetCode.139 Word Break
author: Matteo
tags: []
categories:
  - LeetCode
date: 2019-05-23 19:37:00
---
##### 题意
* 给一个字符串和一个字符串数组，判断字符串是否能由数组的字符串组合而成，可以重复使用同时不必全部使用
##### TLE解法
* TLE的自然是递归了😩
```java
class Solution {
    Set<String> used = new HashSet<>();

    public boolean wordBreak(String s, List<String> wordDict) {
        return search(s, wordDict, 0);
    }

    boolean search(String s, List<String> wordDict, int start) {
        for (int i = 0; i < wordDict.size(); i++) {
            if (s.startsWith(wordDict.get(i), start)) {
                if (start + wordDict.get(i).length() == s.length() && s.endsWith(wordDict.get(i))) {
                    return true;
                }
                boolean result = search(s, wordDict, start + wordDict.get(i).length());
                if (result) {
                    return true;
                }
            }
        }
        return false;
    }
}
```
##### 正确解法
* 字符串比较的正确打开方式自然就是动态规划了，唉～
```java
class Solution {

    public boolean wordBreak(String s, List<String> wordDict) {
        int length = s.length();
        //dp用来记录到dp[i]为止是否符合条件
        boolean dp[] = new boolean[length + 1];
        //dp[0]可以理解为空字符串肯定符合条件
        dp[0] = true;
        for (int i = 1; i <= length; i++) {
            //假如dp[j]符合条件，且s[j,i)也符合条件，那么dp[i]也是符合条件的
            for (int j = 0; j < i; j++) {
                String temp = s.substring(j, i);
                if (dp[j] && wordDict.contains(temp)) {
                    dp[i] = true;
                    break;
                }
            }
        }
        return dp[length];
    }
}
```