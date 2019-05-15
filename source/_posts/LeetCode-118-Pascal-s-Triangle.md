title: LeetCode.118 Pascal's Triangle
author: Matteo
date: 2019-05-15 10:20:27
tags:
---
[https://leetcode.com/problems/pascals-triangle/](https://leetcode.com/problems/pascals-triangle/)
##### 思路
1. 开始想的是直接递归解决，提交后发现TLE，于是采用HashMap记录中间状态，能够AC
```java
class Solution {

    HashMap<Integer, HashMap<Integer, Integer>> map = new HashMap<>();
    List<List<Integer>> results = new ArrayList<>();
    public List<List<Integer>> generate(int numRows) {
        for (int i = 0; i < numRows; i++) {
            List<Integer> temp = new ArrayList<>();
            for (int j = 0; j <= i; j++) {
                temp.add(f(i, j));
            }
            results.add(temp);
        }
        return results;
    }

    int f(int level, int pos) {
        if (!map.containsKey(level)) {
            map.put(level, new HashMap<>());
        }
        HashMap<Integer, Integer> lmap = map.get(level);
        if (lmap.containsKey(pos)) {
            return lmap.get(pos);
        } else {
            int val = 0;
            if (level == 0 || level == 1) {
                val = 1;
            } else if (pos == 0 || pos == level) {
                val = 1;
            } else {
                val = f(level - 1, pos - 1) + f(level - 1, pos);
            }
            lmap.put(pos, val);
            return val;
        }

    }
}
```
2. 后来找到了新的方案，第0层只有一个1，后面每层在前面插入一个1，然后从位置1开始，当前位置就等于后面两个位置的和。代码精简了一大半。但是时间居然一样，好尴尬～
```java
class Solution {
    List<List<Integer>> results = new ArrayList<>();
    public List<List<Integer>> generate(int numRows) {
        List<Integer> result = new LinkedList<>();
        result.add(1);
        for (int i = 0; i < numRows; i++) {
            if (i > 0) {
                result.add(0, 1);
                for (int j = 0; j < i - 1; j++) {
                    result.set(j + 1, result.get(j + 1) + result.get(j + 2));
                }
            }
            results.add(new ArrayList<>(result));
        }
        return results;
    }
}
```