title: LeetCode.209 Minimum Size Subarray Sum
author: Matteo
date: 2019-05-31 15:14:56
tags:
---
[https://leetcode.com/problems/minimum-size-subarray-sum/](https://leetcode.com/problems/minimum-size-subarray-sum/)
##### 题意
* 从nums数组中找出最小连续子序列的和不小于s
##### 解法
* 一开始的想法就是从每一个数向后累加，直接和大于s，但是这样复杂度为O($n^2$)，可能会超时
* 后来发现
1. 假定nums[start, i]的和不小于s，那么nums[start, i - 1]的任意子序列的和肯定小于s。
2. 假定sum - num[start]的和仍不小于s，根据1的结论，那么以start+1为起点的子序列长度的最小值肯定不小于[start, i]，因此将start后移直到sum小于s。
3. s继续累加直至sum不小于s，再重复2的步骤。
4. 这样的时间复杂度为O($\log n$)（我猜的）
```java
class Solution {
    public int minSubArrayLen(int s, int[] nums) {
        //100%答案里的魔法代码
        // if(s==697439)return 132;
        // if(s==120331635) return 2327;
        int sum = 0;
        int start = 0;
        int minCount = Integer.MAX_VALUE;
        for (int  i = 0; i < nums.length; i++) {
            sum += nums[i];
//            if (sum >= s) {
//                minCount = Math.min(i - start + 1, minCount);
                while (start <= i && sum >= s) {
                    minCount = Math.min(i - start + 1, minCount);
                    sum-=nums[start++];
                }
//            }
        }
        return minCount == Integer.MAX_VALUE?0:minCount;
    }
}
```