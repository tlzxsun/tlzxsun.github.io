title: LeetCode.174 Dungeon Game
author: Matteo
date: 2019-05-28 09:15:53
tags:
---
[https://leetcode.com/problems/dungeon-game/](https://leetcode.com/problems/dungeon-game/)
##### 题意
* 勇士从迷宫的左上角走到右下角去救公主，路上每次房间有怪物（扣血）或者血瓶（加血），勇士的血量必须大于0，否则的GG了。求勇士出发时的最小血量
##### 思路
* 开始想用dfs做，但是需要遍历所有情况，时间复杂度为O($c^r$)，即使做出来也肯定是TLE。觉得应该是用动态规划做，但是想的从左上角开始规划，一直没想出来。后来了看了网上的解法，从公主位置开始规划，就简单很多。下面说下思路
1. 公主房间的血量为max(1, 1- dungeon[row-1][column-1])，因为公主房间是最后一个，此时血量为1-dungeon[row-1][column-1]即可，但是出发时的最小血量必须是正数，所以结果即为max(1, 1 - dungeon[row-1][column-1])
2. 从最后一排去救公主的勇士只能右左向右走，由于公主房间的最小血量已经确定，那么前一个房间的最小血量即为max(1, dp[row-1][j] - dungeon[row-1][j+1])，最后一列的情况同理
3. 经过1，2后，边界已经确定，可知dp[i][j]=max(1, min(dp[i][j+1], d[i+1][j]) - dungeon[i][j])。因为勇士只能向右或向下走，那么这个位置的最小值就为右边或下边的值里小的那个，再减去当前房间所需血量。
4. PS
  4.1 网上的解法很多都是定义dp[row+1][column+1]，dp[row][j]和dp[i][column]为Integer.MAX_VALUE，dp[row][column-1]=1，dp[column-1][row]=1，个人觉得这样不是很好理解，而且需要定义额外的数组。
  4.2 这里的解法的话，边界相对更好理解，而且dp数组跟dungeon数组可以为同一个，占用更少的内存
```java
class Solution {

    public int calculateMinimumHP(int[][] dungeon) {
        int row = dungeon.length, column = dungeon[0].length;
        //公主房间情况
        dungeon[row - 1][column - 1] = Math.max(1, 1 - dungeon[row - 1][column - 1]);
        //最右边界
        for (int i = row - 2; i >= 0; i--) {
            dungeon[i][column - 1] = Math.max(1, dungeon[i][column - 1] - dungeon[i + 1][column - 1]);
        }
        //最下边界
        for (int j = column - 2; j >= 0; j--) {
            dungeon[row - 1][j] = Math.max(1, dungeon[row - 1][j] - dungeon[row - 1][j + 1]);
        }
        for (int i = row - 2; i >= 0; i--) {
            for (int j = column - 2; j >= 0; j--) {
                //dp
                dungeon[i][j] = Math.max(1, Math.min(dungeon[i + 1][j], dungeon[i][j + 1]) - dungeon[i][j]);
            }
        }
        return dungeon[0][0];
    }
}
```