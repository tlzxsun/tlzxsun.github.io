title: LeetCode.354 Russian Doll Envelopes
author: Matteo
date: 2019-08-15 19:10:50
tags:
---
[https://leetcode.com/problems/russian-doll-envelopes/](https://leetcode.com/problems/russian-doll-envelopes/)
##### 题意
* 求大众套娃，咦，是俄罗斯套娃的最大层数
##### 解法
* dfs+cache，从每个位置开始搜索，并记下中间结果，否则会超时
```java
class Solution {

    HashMap<int[], Integer> cache = new HashMap<>();

    public int maxEnvelopes(int[][] envelopes) {
        int length = envelopes.length;
        if (length == 0) {
            return 0;
        }
        int max = 1;
        for(int i = 0; i < length; i++) {
            max = Math.max(max, search(envelopes, envelopes[i]));
        }
        return max;
    }

    int search(int[][] envelopes, int[] current) {
        if (cache.containsKey(current)) {
            return cache.get(current);
        }
        int length = envelopes.length;
        int max = 1;
        for (int i = 0; i < length; i++) {
            if (envelopes[i][0] > current[0] && envelopes[i][1] > current[1]) {
                max = Math.max(max, 1 + search(envelopes, envelopes[i]));
            }
        }
        cache.put(current, max);
        return max;
    }
}
```