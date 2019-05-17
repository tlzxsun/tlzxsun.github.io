title: LeetCode.131 Palindrome Partitioning
author: Matteo
date: 2019-05-17 10:02:46
tags:
---
[https://leetcode.com/problems/palindrome-partitioning/](https://leetcode.com/problems/palindrome-partitioning/)
##### 题意
* 取出所有的回文子串，想到的就是dfs，开始想着可能会超时，后来写出来了发现并没有
##### 解法
```java
class Solution {
    Stack<String> stack = new Stack<>();
    List<List<String>> results = new ArrayList<>();
    public List<List<String>> partition(String s) {
        dfs(s);
        return results;
    }

    void dfs(String s) {
        for (int i = 1; i <= s.length(); i++) {
            String temp = s.substring(0, i);
            if (isPalindrome(temp)) {
                stack.push(temp);
                if (i == s.length()) {
                    results.add(new ArrayList<String>(stack));
//                    System.out.println(stack);
                } else {
                    dfs(s.substring(i, s.length()));
                }
                stack.pop();
            }
        }
    }

    boolean isPalindrome(String s) {
        for (int i = 0; i < s.length() / 2; i++) {
            if (s.charAt(i) != s.charAt(s.length() - 1 - i)) {
                return false;
            }
        }
        return true;
    }
}
```