title: LeetCode.295 Find Median from Data Stream
author: Matteo
date: 2019-06-14 14:22:37
tags:
---
[https://leetcode.com/problems/find-median-from-data-stream/](https://leetcode.com/problems/find-median-from-data-stream/)
##### 题意
* 实现一个MedianFinder，实现添加数字和查找中位数的功能，中位数定义为当数字个数位奇数时，返回中间的数，否则返回中间两个数的平均值
##### 解法
* 完全不知道怎么解，看了网上的解法才知道大小堆😂
* 大顶堆表示元素从小到大排列，大的元素在顶，小顶堆表示元素从大到小排列，小的元素在顶。且大堆中所有元素均小于小堆中元素。
* 解法中用PriorityQueue实现大小堆
```java
class MedianFinder {

    //小顶堆，元素从小到大排列，先取到小的元素
    PriorityQueue<Integer> small = new PriorityQueue<>();
    //大顶堆，元素从大到小排列，先取到大的元素
    PriorityQueue<Integer> big = new PriorityQueue<>(new Comparator<Integer>() {
        @Override
        public int compare(Integer o1, Integer o2) {
            return o2 - o1;
        }
    });

    /** initialize your data structure here. */
    public MedianFinder() {

    }

    public void addNum(int num) {
        //假如大顶堆为空，则将元素加入大顶堆
        if (big.size() == 0) {
            big.add(num);
            return;
        }
        //两个堆中元素数量相同
        if (big.size() == small.size()) {
            //假如当前数大于小顶堆顶部元素，那么先将小顶堆顶部元素加入大顶堆，并将将该元素加入小顶堆，否则将该元素加入大大顶堆
            if (num > small.peek()) {
                big.add(small.poll());
                small.add(num);
            } else {
                big.add(num);
            }
        } else if (big.size() > small.size()) {
            //假如当前数小于大顶堆顶部元素，那么将大顶堆顶部元素加入小顶堆，并将该元素加入大顶堆，否则将该元素加入小顶堆
            if (num < big.peek()) {
                small.add(big.poll());
                big.add(num);
            } else {
                small.add(num);
            }
        } else if (big.size() < small.size()) {
            //假如当前数大于小顶堆顶部元素，则处理同两堆个数相同时情况
            if (num > small.peek()) {
                big.add(small.poll());
                small.add(num);
            } else {
                big.add(num);
            }
        }
    }

    public double findMedian() {
        if (big.size() > small.size()) {
            return big.peek();
        } else if (big.size() < small.size()) {
            return small.peek();
        } else {
            return (big.peek() + small.peek())/2.0;
        }
    }
}
```
