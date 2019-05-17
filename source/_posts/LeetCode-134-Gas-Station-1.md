title: LeetCode.134 Gas Station
author: Matteo
date: 2019-05-17 18:33:57
tags:
---
[https://leetcode.com/problems/gas-station/submissions/](https://leetcode.com/problems/gas-station/submissions/)
##### 思路1 
1. 算出每个station出发之后剩余的gas，保存在数组中
2. 从数组中找出剩余gas不小于0的位置，然后从该位置开始查询，走回原点或者gas小于0退出循环
3. 这个解法能AC，但是非常慢，继续想
* AC解
```java
class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        int gasLeft[] = new int[gas.length];
        for (int i = 0; i < gas.length; i++) {
            gasLeft[i] = gas[i] - cost[i];
        }
        for (int i = 0; i < gas.length; i++) {
            if (gasLeft[i] >= 0) {
                int g = gasLeft[i];
                for (int j = i + 1; ; j++) {
                    j%=gas.length;
                    g += gasLeft[j];
                    if (g < 0) {
                        break;
                    }
                    if (j == i) {
                        return i;
                    }
                }
            }
        }
        return -1;
    }
}
```
##### 思路2
> 累加在每个位置的left += gas[i] - cost[i], 就是在每个位置剩余的油量, 如果left一直大于0, 就可以一直走下取. 如果left小于0了, 那么就从下一个位置重新开始计数, 并且将之前欠下的多少记录下来, 如果最终遍历完数组剩下的燃料足以弥补之前不够的, 那么就可以到达, 并返回最后一次开始的位置.否则就返回-1.
证明这种方法的正确性: 
>1. 如果从头开始, 每次累计剩下的油量都为整数, 那么没有问题, 他可以从头开到结束.
>2. 如果到中间的某个位置, 剩余的油量为负了, 那么说明之前累积剩下的油量不够从这一站到下一站了. 那么就从下一站从新开始计数. 为什么是下一站, 而不是之前的某站呢? 因为第一站剩余的油量肯定是大于等于0的, 然而到当前一站油量变负了, 说明从第一站之后开始的话到当前油量只会更少而不会增加. 也就是说从第一站之后, 当前站之前的某站出发到当前站剩余的油量是不可能大于0的. 所以只能从下一站重新出发开始计算从下一站开始剩余的油量, 并且把之前欠下的油量也累加起来, 看到最后剩余的油量是不是大于欠下的油量.
https://blog.csdn.net/qq508618087/article/details/50990076
* 源码
```java
class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        int left = 0, lack = 0, start = 0;
        for (int i = 0; i< gas.length ; i++) {
            left += gas[i] - cost[i];
            if (left < 0) {
                start = i + 1;
                lack += left;
                left = 0;
            }
        }
        return (left + lack >= 0)?start:-1;
    }
}
```