title: LeetCode.51 N Queens
author: Matteo
date: 2019-05-13 14:50:59
tags:
---
[https://leetcode.com/problems/n-queens/](https://leetcode.com/problems/n-queens/)
* 经典N皇后问题，先背一个解法吧，具体见注释
```java
class Solution {
    int q[];
    List<List<String>> results = new ArrayList<>();

    public List<List<String>> solveNQueens(int n) {
        int r = 0, c = 0;
        q = new int[n];
        for (int i = 0; i < n; i++) {
            q[i] = -1;
        }
        while (r < n) {
            while (c < n) { //探测r行是否可以放置
                if (valid(r, c)) {  //r行c列可以放置
                    q[r] = c;   //放置
                    c = 0;  //下一行从0开始探测
                    break;
                } else {
                    c++;    //探测下一列
                }
            }
            if (q[r] == -1) {   //该行未找到位置
                if (r == 0) {   //第一行未找到，没有结果，返回
                    break;
                } else {    //没有找到可以放置皇后的列，回溯
                    r--;
                    c = q[r] + 1;   //上一行皇后位置向后移动
                    q[r] = -1;  //重置上一行原来位置
                    continue;
                }
            }
            if (r == n - 1) {   //最后一行找到了皇后位置
                addResult(n);   //加入结果
                c = q[r] + 1;   //从最后一行放置皇后位置后探测
                q[r] = -1;  //重置最后一行皇后
                continue;
            }
            r++;    //探测下一行
        }
        return results;
    }

    void addResult(int n) {
        List<String> result = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            StringBuilder builder = new StringBuilder();
            for (int j = 0; j < n; j++) {
                if (j != q[i]) {
                    builder.append(".");
                } else {
                    builder.append("Q");
                }
            }
            result.add(builder.toString());
        }
        results.add(result);
    }

    boolean valid(int r, int c) {
        for (int i = 0; i < r; i++) {
            //q[i] == c，即之前列中皇后是否在该列
            //c - q[i]即c-i行的c，r-i即r-i行的r，两者相等则说明在对角线上
            if (q[i] == c || Math.abs(c - q[i]) == Math.abs(r - i)) {
                return false;
            }
        }
        return true;
    }
}
```