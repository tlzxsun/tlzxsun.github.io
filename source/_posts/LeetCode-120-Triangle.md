title: LeetCode.120 Triangle
author: Matteo
date: 2019-05-15 11:08:02
tags:
---
[https://leetcode.com/problems/triangle/](https://leetcode.com/problems/triangle/)
##### 思路
1. 一开始想到用递归，写的真是优美简洁，然并卵～～一个TLE糊脸
```java
class Solution {

    int min = Integer.MAX_VALUE;

    public int minimumTotal(List<List<Integer>> triangle) {
        search(triangle, 0, 0, 0);
        return min;
    }

    void search(List<List<Integer>> triangle, int level, int pos, int sum) {
        if (level == triangle.size()) {
            min = Math.min(sum, min);
        } else {
            search(triangle, level + 1, pos, sum + triangle.get(level).get(pos));
            search(triangle, level + 1, pos + 1, sum + triangle.get(level).get(pos));
        }
    }
}
```
2. 后来一想分层计算就行～
```java
class Solution {

    public int minimumTotal(List<List<Integer>> triangle) {
        if (triangle.size() > 0) {
            List<List<Integer>> length = new ArrayList<>(triangle);
            length.set(0, new ArrayList<>(triangle.get(0)));
            for (int i = 1; i < triangle.size(); i++) {
                length.set(i, new ArrayList<>(triangle.get(i)));
                length.get(i).set(0, triangle.get(i).get(0) + length.get(i - 1).get(0));
                length.get(i).set(length.get(i).size() - 1, triangle.get(i).get(triangle.get(i).size() - 1)
                        + length.get(i - 1).get(length.get(i - 1).size() - 1));
                for (int j = 1; j < triangle.get(i).size() - 1; j++) {
                    length.get(i).set(j, Math.min(length.get(i - 1).get(j), length.get(i - 1).get(j - 1)) + triangle.get(i).get(j));
                }
            }
            int min = Integer.MAX_VALUE;
            for (int i = 0; i < length.get(length.size() - 1).size(); i ++) {
                min = Math.min(length.get(length.size() - 1).get(i), min);
            }
            return min;
        }
        return 0;
    }
}
```