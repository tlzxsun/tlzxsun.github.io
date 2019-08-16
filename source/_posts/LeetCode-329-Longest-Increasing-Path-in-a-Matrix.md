title: LeetCode.329 Longest Increasing Path in a Matrix
author: Matteo
date: 2019-07-22 14:06:20
tags:
---
[https://leetcode.com/problems/longest-increasing-path-in-a-matrix/](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/)
##### 题意
* 求二维数组中上下左右延伸的最长序列长度
##### 思路
* 这类题首先想到的就dfs，从一个数开始搜索，查找这个数开始的最大序列，最终结果就是就是每一个位置的结果取最大值。
* 开始想的比较复杂，写了很多没用的逻辑，比如判断这个数是否用过还开了新数组来记录并回溯（因为是递增的，同样的数肯定不会用两次）。写完之后发现第135个case超时了，因为每个数都需要向4个方向搜索，时间复杂度基本上就是O($4^{n^2}$)，n稍大就不可能计算出来。
* 后来看了别人的解，思路基本上一样，只是中间结果用一个dp数组记下，避免重复计算。
```java
class Solution {
    int[][] dp;

    public int longestIncreasingPath(int[][] matrix) {
        if (matrix.length == 0) {
            return 0;
        }
        int row = matrix.length, column = matrix[0].length;
        dp = new int[row][column];
        int result = 1;
        for (int i = 0 ; i < row; i++) {
            for (int j = 0; j < column; j++) {
                result = Math.max(result, search(matrix, i, j));
            }
        }
        return result;
    }

    int search(int[][] matrix, int r, int c) {
        if (dp[r][c] != 0) {
            return dp[r][c];
        }
        int mx = 1;
        int row = matrix.length, column = matrix[0].length;
        if (r + 1 < row && matrix[r + 1][c] > matrix[r][c]) {
            mx = Math.max(mx, 1 + search(matrix, r + 1, c));
        }

        if (r - 1 >= 0 && matrix[r - 1][c] > matrix[r][c]) {
            mx = Math.max(mx, 1 + search(matrix, r - 1, c));
        }

        if (c + 1 < column && matrix[r][c + 1] > matrix[r][c]) {
            mx = Math.max(mx, 1 + search(matrix, r, c + 1));
        }

        if (c - 1 >= 0 && matrix[r][c - 1] > matrix[r][c]) {
            mx = Math.max(mx, 1 + search(matrix, r, c - 1));
        }
        dp[r][c] = mx;
        return mx;
    }
}
```