title: LeetCode.169 Majority Element
author: Matteo
date: 2019-05-27 14:43:58
tags:
---
[https://leetcode.com/problems/majority-element/](https://leetcode.com/problems/majority-element/)
##### 题意
* 找出一个无序数列中占多数的数
##### 解法
1. 排序后（n/2位置即结果）位置即结果
2. [https://en.wikipedia.org/wiki/Boyer%E2%80%93Moore_majority_vote_algorithm](https://en.wikipedia.org/wiki/Boyer%E2%80%93Moore_majority_vote_algorithm)

```java
class Solution {
    public int majorityElement(int[] nums) {
        //Boyer Moore Voting Algorithm
        int count = 0, candidateIdx = -1;
        for (int i = 0; i < nums.length; i++) {
            if (count == 0) {
                candidateIdx = i;
            }
            if (nums[i] == nums[candidateIdx]) {
                count++;
            } else {
                count--;
            }
        }
        return nums[candidateIdx];
        
    }
}
```
