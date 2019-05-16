title: LeetCode.130 Surrounded Regions
author: Matteo
date: 2019-05-16 20:11:12
tags:
---
[https://leetcode.com/problems/surrounded-regions/](https://leetcode.com/problems/surrounded-regions/)
##### 思路
* 一开始想的是顺序找到第一个位置然后找出路，这样的话太复杂，需要记下路径判断该路径能否突围
* 后面反着想就是从四个边开始找注定能突围的位置，然后向四个位置查找，只要是连通的肯定能突围，后续计算量会小很多
* 写完AC后发现时间差很多，去查看了100%的答案，发现思路完成一样，于是一句句改提交为什么会慢这么多，最后发现是因为我用System.out.println()打印日志导致的😂

##### 解法
```java
class Solution {
    public void solve(char[][] board) {

        if (board.length == 0) {
            return;
        }

        for (int i = 0; i < board.length; i++) {
            set(board, i, 0);
            set(board, i, board[0].length - 1);

        }
        for (int i = 0; i < board[0].length; i++) {
            set(board, 0, i);
            set(board, board.length - 1, i);

        }
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[0].length; j++) {
                if (board[i][j] != '.') {
                    board[i][j] = 'X';
                } else {
                    board[i][j] = 'O';
                }
            }
        }
    }

    void set(char[][] board, int i, int j) {
        if (i >= 0 && i < board.length && j >= 0 && j < board[0].length && board[i][j] == 'O') {
            board[i][j] = '.';
            set(board, i + 1, j);
            set(board, i - 1, j);
            set(board, i, j + 1);
            set(board, i, j - 1);
        }
    }
}
```