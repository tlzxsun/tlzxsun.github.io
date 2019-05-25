title: LeetCode.135 Candy
author: Matteo
tags: []
categories:
  - LeetCode
date: 2019-05-22 09:29:00
---
##### 题意
给一个数组代表权重，给数组每一个元素赋值，值最小为1，且相邻的元素权重大的值必须大于权重小的
##### 分析
* 直接给第一个元素赋值1，然后依次比较，有三种情况
1. ratings[i] > ratings[i - 1]，则当前值为前一个的值加1
2. ratings[i] == ratings[i - 1]，则当前值为1
2. ratings[i] < ratings[i - 1]，刚当前值为1，这时候需要处理前一个值也为1的情况，需要往前遍历，将ratings[i - 1]加1
* 这个解法能AC，但是非常慢
```java
class Solution {
    public int candy(int[] ratings) {
        if (ratings.length < 2) {
            return ratings.length;
        }
        int cnts[] = new int[ratings.length];
        cnts[0] = 1;
        for (int i = 1; i < ratings.length; i++) {
            if (ratings[i] > ratings[i-1]) {
                cnts[i] = cnts[i - 1] + 1;
            } else if (ratings[i] < ratings[i-1]) {
                cnts[i] = 1;
                recover(ratings, cnts, i);
            } else {
                cnts[i] = 1;
            }
        }
        int count = 0;
        for (int i = 0; i < cnts.length; i++) {
            count += cnts[i];
        }
        return count;
    }

    void recover(int ratings[], int[] cnts, int start) {
        while (start > 0) {
            //往前遍历处理，cnts相同，且ratings小的情况
            if (ratings[start - 1] > ratings[start] && cnts[start - 1] == cnts[start]) {
                cnts[start - 1] ++;
            } else {
                break;
            }
            start--;
        }
    }
}
```
##### 贪心
* 直接参考别人的解法吧～https://blog.csdn.net/JackZhang_123/article/details/78008766
```java
import java.util.Arrays;

/*
 * 典型的贪心算法
 * 题本身可以用贪心法来做，我们用candy[n]表示每个孩子的糖果数，遍历过程中，
 * 如果孩子i+1的rate大于孩子i 的rate，那么当前最好的选择自然是：给孩子i+1的糖果数=给孩子i的糖果数+1
 * 如果孩子i+1的rate小于等于孩子i 的rate咋整？这个时候就不大好办了，
 * 因为我们不知道当前最好的选择是给孩子i+1多少糖果。
 * 解决方法是：暂时不处理这种情况。等数组遍历完了，我们再一次从尾到头遍历数组，
 * 这回逆过来贪心，就可以处理之前略过的孩子。
 * 最后累加candy[n]即得到最小糖果数。
 * */
public class Solution 
{
    public int candy(int[] ratings) 
    {
        if(ratings==null || ratings.length<=0)
            return 0;

        int []num = new int[ratings.length];
        Arrays.fill(num, 1);

        for(int i=1;i<ratings.length;i++)
        {
            if(ratings[i]>ratings[i-1])
                num[i]=num[i-1]+1;
        }

        for(int i=ratings.length-2;i>=0;i--)
        {
            if(ratings[i]>ratings[i+1] && num[i] < num[i+1]+1)
                num[i]=num[i+1]+1;
        }

        int sum=0;
        for(int i=0;i<num.length;i++)
            sum+=num[i];
        return sum; 
    }
}
```