title: LeetCode.72 Edit Distance
author: Matteo
date: 2019-05-13 11:38:49
tags:
---
[https://leetcode.com/problems/edit-distance/](https://leetcode.com/problems/edit-distance/)
* 这一题没做出来，参考的别人的答案😞
* 使用dis[i][j]记录匹配到i,j位置里的最短距离
* 边界情况dis[0][i]和dis[0][j]，即words1, words2一方为空串，可知距离为另一方长度
* 当words[i]==words[j]， 则dis[i][j]=dis[i-1][j-1]
* 当words[i]!=words[j-1]，则有下面三种情况，取最小值
  * words1插入，dis[i][j]=dis[i][j-1]+1
  * words1删除，dis[i][j]=dis[i-1][j]+1
  * words1替换，dis[i][j]=dis[i-1][j-1]+1
* 以horse和ros为列

|||r|o|s|
|-|-|-|-|-|
| |0|1|2|3|
|h|1|1|2|3|
|o|2|2|1|2|
|r|3|2|2|2|
|s|4|3|3|2|
|e|5|4|4|3|

* 附上AC代码
```java
class Solution {
    public int minDistance(String word1, String word2) {
        int r = word1.length();
        int c = word2.length();
        int dis[][] = new int[r + 1][c + 1];
        for (int i = 1; i <= r; i++) {
            dis[i][0] = i;
        }
        for (int i = 1; i <= c; i++) {
            dis[0][i] = i;
        }
        for (int i = 1; i <= r; i++) {
            for (int j = 1; j <= c; j++) {
                if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                    dis[i][j] = dis[i - 1][j - 1];
                } else {
                    dis[i][j] = Math.min(dis[i-1][j-1], Math.min(dis[i-1][j], dis[i][j-1]));
                }
            }
        }
        return dis[r][c];
    }
}
```