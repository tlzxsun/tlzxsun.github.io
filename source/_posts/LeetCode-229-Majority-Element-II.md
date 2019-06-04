title: LeetCode.229 Majority Element II
author: Matteo
date: 2019-06-04 20:33:19
tags:
---
[https://leetcode.com/problems/majority-element-ii/](https://leetcode.com/problems/majority-element-ii/)
##### 题意
* 求数组中占比超过1/3的数
##### 解法
* 投票法（先背下～）
```java
class Solution {
    public List<Integer> majorityElement(int[] nums) {
        //Space Complexity : O(n)
        //Time Complexity : O(1)
        //There can be only 2 major elements
        ArrayList<Integer> result = new ArrayList<Integer>();
        if(nums.length==0){
            return result;
        }
        int number1 = nums[0];
        int number2 = nums[0];
        int n = nums.length;
        
        int count1 = 0,count2 = 0,i=0;
        while(i<n){
            if(nums[i]==number1){
                count1++;
            }
            else if(nums[i]==number2){
                count2++;                
            }
            else if(count1==0){
                number1 = nums[i];
                count1 = 1;
            }
            else if(count2==0){
                number2 = nums[i];
                count2 = 1;
            }
            else{
                count1--;
                count2--;
            }
            i++;
        }
        count1 = 0;
        count2 = 0;
        for(i=0;i<nums.length;i++){
            if(nums[i]==number1){
                count1++;
            }
            else if(nums[i]==number2){
                count2++;
            }
        }
        if(count1>n/3){
            result.add(number1);
        }
        if(count2>n/3){
            result.add(number2);
        }
        return result;
    }
}
```