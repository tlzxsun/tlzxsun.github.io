title: LeetCode.279 Perfect Squares
author: Matteo
date: 2019-06-10 21:18:54
tags:
---
[https://leetcode.com/problems/perfect-squares/](https://leetcode.com/problems/perfect-squares/)
##### 题意
* 给一个数，将它分解成整数的平方数之和，求最小的平方数的数量
##### 解法
* BFS，采用queue计算中间结果，current表示当前值，count表示最小数量
1. 将n入列
2. queue中取值赋给current，计算1到current之间的所有平方数
3. 遍历1到current之前的平方数num，假如current等于平方数，因为是BFS，每一次都会遍历所有的可能结果，那么此时肯定是count最小值，返回结果，否则将current-num加入临时结果集
4. 直到queue为空，说明这一层已经遍历结束，count++并将临时结果集中的值加入queue中
5. 重复步骤2
```java
class Solution {
    HashMap<Integer, Set<Integer>> cache = new HashMap<>();
    public int numSquares(int n) {
        int count = 1;
        Queue<Integer> queue = new LinkedList<>();
        queue.offer(n);
        Set<Integer> temps = new HashSet<>();
        while (true) {
            int current = queue.poll();
            Set<Integer> nums = getNums(current);
            Iterator<Integer> iterator = nums.iterator();
            while (iterator.hasNext()) {
                int next = iterator.next();
                if (current == next) {
                    return count;
                }
                temps.add(current - next);
            }
            if (queue.isEmpty()) {
                queue.addAll(temps);
                count++;
                temps.clear();
            }
        }
    }

    Set<Integer> getNums(int n) {
        int sqrt = (int) Math.sqrt(n);
        if (cache.containsKey(sqrt)) {
            return cache.get(sqrt);
        }
        Set<Integer> set = new HashSet<>();
        for (int i = 1; i <= sqrt; i++) {
            set.add(i * i);
        }
        cache.put(sqrt, set);
        return set;
    }
}
```
##### 改进
* BFS每一层都需要遍历所有结果，肯定有优化方案，to be continue...