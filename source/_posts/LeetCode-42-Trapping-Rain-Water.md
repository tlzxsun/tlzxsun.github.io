title: LeetCode.42 Trapping Rain Water
author: Matteo
tags: []
categories: []
date: 2019-05-10 15:47:00
---
[https://leetcode.com/problems/trapping-rain-water/](https://leetcode.com/problems/trapping-rain-water/)
<center>![示例.jpg](https://upload-images.jianshu.io/upload_images/16684799-daaa1f8abc589d91.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)</center> 
* 开始的想法是从i往后找，直到找到第一个不比height[i]小的数的位置j，计算i-j之间的区域，然后继续从j向后找。但是这样会导致递增里无法计算面积，因此方法变成从i向两找不小于height[i]的位置，但是这样又会导致相等时面积重复计算。
* 最终采用的方案是从i向后找，假如找到height[j] >= height[i]的时直接返回j，计算i-j之间的区域，再从j向后查找，假如都比height[i]小的话则找到遍历到的最大的height[j]，计算i-j之间的区域，再从j向后查找。这题虽然是hard难度，但是感觉并没有那么难。
附上ac代码
```java
class Solution {
    public int trap(int[] height) {
        if (height.length < 2) {
            return 0;
        }
        int size = 0;
        int pos = 0;
        while (pos < height.length) {
            if (height[pos] == 0) {
                pos++;
                continue;
            }
            int rightPos = -1;
            int tempHigh = 0;
            for (int i = pos + 1; i < height.length; i++) {
                //找到大于height[i]则直接返回
                if (height[i] >= height[pos]) {
                    rightPos = i;
                    break;
                } else {
                    //否则返回height[i]最大时位置
                    if (height[i] > tempHigh) {
                        tempHigh = height[i];
                        rightPos = i;
                    }
                }
            }
            if (rightPos != -1) {
                size += Math.min(height[pos], height[rightPos]) * (rightPos - pos - 1);
                for (int i = pos + 1; i < rightPos; i++) {
                    size -= height[i];
                }
                pos = rightPos;
            } else {
                pos++;
            }
        }
        return size;
    }
}
```