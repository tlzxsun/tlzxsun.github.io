title: LeetCode.292 Nim Game
author: Matteo
date: 2019-06-13 15:47:32
tags:
---
[https://leetcode.com/problems/nim-game/](https://leetcode.com/problems/nim-game/)
##### 题意
* 两个人依次取数，每次可以取1，2，3，给出一个数，求先取的能否获胜
##### 解法
* 获胜的人肯定是最后一个取数，可能取1，2，3，f(n)时只要f(n-1)，f(n-2)，f(n-3)中有一个不能取胜，那么f(n)就能取胜，类似于斐波那挈数列的求法。但是当n很大的时候递归肯定会StackOverFlow，因此用一个数组来保存中间结果，结果Memory Limit Exceeded😂😂
* 后来再一想，f(n)依赖于f(n-1)，f(n-2)，f(n-3)。其中f(1)，f(2)，f(3)是false，f(4)是true，那么f(5)，f(6)，f(7)为false。即只有为4的倍数的时候才不能取胜😂😂😂😂
```java
class Solution {
    public boolean canWinNim(int n) {
        return n % 4 != 0;
    }
}
```