title: LeetCode.368 Largest Divisible Subset
author: Matteo
date: 2019-08-15 19:11:12
tags:
---
[https://leetcode.com/problems/largest-divisible-subset/](https://leetcode.com/problems/largest-divisible-subset/)
##### 题意
* 求一个数组中最长子序列，子序列中任意两个数满足i % j == 0 || j % i == 0
##### 思路
这个问题相当于对数组进行排序，求一个子序列，其中每一个数都是前一个的整数倍。
1. 开始想的是从每一个数开始向后遍历，记下最大值时的结果。时间复杂度为O(n!)，基本上无法计算出来，加上缓存后在最后一个用例还是TLE，最后再加上另一处优化总算是没有超时。
```java
class Solution {
    HashMap<Integer, List<Integer>> cache = new HashMap<>();

    List<Integer> fResult = new ArrayList<>();
    long ts = System.currentTimeMillis();

    public List<Integer> largestDivisibleSubset(int[] nums) {
        Arrays.sort(nums);
        for (int i = 0; i < nums.length; i++) {
            List<Integer> result = search(i, nums);
            if (result.size() > fResult.size()) {
                fResult = new ArrayList<>(result);
                if (fResult.size() == nums.length) {
                    return fResult;
                }
            }
        }
//        search(1, nums);
        return fResult;
    }

    List<Integer> search(int i, int[] nums) {
        if (cache.containsKey(i)) {
            return cache.get(i);
        }
        List<Integer> result = new ArrayList<>();
        List<Integer> max = new ArrayList<>();
        result.add(nums[i]);
        long last = nums[i];
        long temp = nums[i];
        for (int j = i + 1; j < nums.length; j++) {
            int k = 2;
            while (temp < nums[j]) {
                temp = last * k++;
            }
            if (temp == nums[j]) {
                List<Integer> tempList = search(j, nums);
                if (tempList.size() > max.size()) {
                    max = tempList;
                }
                if (tempList.size() == nums.length - j) {
                    break;
                }
            }
        }
        result.addAll(max);
        cache.put(i, result);
        return result;

    }
}
```
2. 看了别人的答案基本上都是dp，具体的思路见注解吧
```java
class Solution {
    public List<Integer> largestDivisibleSubset(int[] nums) {
        if (nums.length == 0) {
            return new ArrayList<>();
        }
        int len = nums.length, curMax = 1, k = 0;
        //par表示该位置数前一个满足条件的数
        int par[] = new int[len];
        int dp[] = new int[len];
        List<Integer> result = new ArrayList<>();

        //倒序
        Arrays.sort(nums);
        for (int i = 0; i < len/2;i++) {
            nums[i] = nums[i] ^ nums[len - 1 - i];
            nums[len - 1 - i] = nums[i] ^ nums[len - 1 - i];
            nums[i] = nums[i] ^ nums[len - 1 - i];
        }
        //par初始值为当前位置，dp为1
        for (int i = 0; i < len; i++) {
            par[i] = i;
            dp[i] = 1;
        }
        for (int i = 1; i < len; i++) {
            for (int j = 0; j < i; j++) {
                //nums[j]不能被nums[i]整除，继续循环
                if (nums[j] % nums[i] != 0) {
                    continue;
                }
                //nums[j]能被nums[i]整除，那么包含nums[i]的最长序列为dp[i]和dp[j]+1中最大值
                //假如dp[i]位置小于dp[j]+1，即证明存在更长的序列
                if (dp[i] < dp[j] + 1) {
                    //i上一个数即为j
                    par[i] = j;
                    dp[i] = dp[j] + 1;
                }
                //假如dp[i]大于当前最大值，那么当前最大值即为dp[i]，最后位置为i
                if (dp[i] > curMax) {
                    curMax = dp[i];
                    k = i;
                }
            }
        }
        //假如上一个数不等于本身，那么将该数加入结果集，给k重新赋值
        while (par[k] != k) {
            result.add(nums[k]);
            k = par[k];
        }
        //将第一个数加入结果集
        result.add(nums[k]);
        return result;
    }
}
```