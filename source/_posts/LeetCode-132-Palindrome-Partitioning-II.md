title: LeetCode.132 Palindrome Partitioning II
author: Matteo
date: 2019-05-17 15:09:47
tags:
---
[https://leetcode.com/problems/palindrome-partitioning-ii/](https://leetcode.com/problems/palindrome-partitioning-ii/)
##### 题意
* 找出将字符串分割成回文子串的最少分割数
##### 思路
* 开始想着用131的dfs，因为131没有TLE，提交之后果然还是TLE了，131因为要输出所有的分割，所以没有类似"ababababababababababababcbabababababababababababa"这样的测试用例。
* 后来想用动态规划做，但是没有想出来怎么做，后来参考的别人的思路。但是第二次的DP是自己想出来的，第一次的解法很精妙，没想出来～
> (动态规划) O(n2)O(n2)
一共进行两次动态规划。
第一次动规：计算出每个子串是否是回文串。
状态表示：st[i][j]st[i][j] 表示 s[i…j]s[i…j] 是否是回文串;
转移方程：s[i…j]s[i…j] 是回文串当且仅当 s[i]s[i]等于s[j]s[j] 并且 s[i+1…j−1]s[i+1…j−1] 是回文串；
边界情况：如果s[i…j]s[i…j]的长度小于等于2，则st[i][j]=(s[i]==s[j])st[i][j]=(s[i]==s[j]);
在第一次动规的基础上，我们进行第二次动规。
状态表示：f[i]f[i] 表示把前 ii 个字符划分成回文串，最少划分成几部分；
状态转移：枚举最后一段回文串的起点 jj，然后利用 st[j][i]st[j][i] 可知 s[j…i]s[j…i] 是否是回文串，如果是回文串，则 f[i]f[i] 可以从 f[j−1]+1f[j−1]+1 转移；
边界情况：0个字符可以划分成0部分，所以 f[0]=0f[0]=0。
题目让我们求最少切几刀，所以答案是 f[n]−1f[n]−1。
时间复杂度分析：两次动规都是两重循环，所以时间复杂度是 O(n2)O(n2)。
https://www.acwing.com/solution/LeetCode/content/227/

```java
class Solution {
    public int minCut(String s) {
        int length = s.length();
        boolean status[][] = new boolean[length][length];
        for (int i = 0; i < length; i++) {
            for (int j = i; j >= 0; j--) {
                if (i - j < 3) {
                    //长度小于等于3时只需判断头尾字符只否相等
                    status[j][i] = s.charAt(i) == s.charAt(j);
                } else {
                    /*否则需要再判断去除头尾的子路是否是回文，这里解法很精妙，常规遍历的话有一半是重复的，而且status[i][j]依赖的s[i + 1][j - 1]并未计算出来，
                    这里的解法从status[i][i]计算到status[0][i]，status[i-1][i-1]在上一次循环中已经计算出来了*/
                    status[j][i] = (s.charAt(i) == s.charAt(j) && status[j + 1][i - 1]);
                }
            }
        }
        int dp[] = new int[length];
        Arrays.setAll(dp, new IntUnaryOperator() {
            @Override
            public int applyAsInt(int operand) {
                return Integer.MAX_VALUE;
            }
        });
        //第一个不用切分
        dp[0] = 0;
        for (int i = 1; i < length; i++) {
            //假如s[0, i]是回文，则不到i时不需要任何切分
            if (status[0][i]) {
                dp[i] = 0;
            } else {
                //从i向前查找
                for (int j = i; j >= 0; j--) {
                    //假如s[j, i]是回文，刚i位置时最小切分取当前切分数与dp[j - 1] + 1中小的值
                    if (status[j][i]) {
                        dp[i] = Math.min(dp[i], dp[j - 1] + 1);
                    }
                }
                // dp[i] = Math.min(dp[i], dp[i - 1] + 1);
            }
        }
        return dp[length - 1];
    }
}

```