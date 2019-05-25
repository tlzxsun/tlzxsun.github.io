title: LeetCode.148 Sort List
author: Matteo
date: 2019-05-25 09:22:56
tags:
---
[https://leetcode.com/problems/sort-list/](https://leetcode.com/problems/sort-list/)
##### 题意
* 不借助额外空间对链表进行排序
##### 解法
* 先使用快慢指针将链表分为前后两部分，分别对两部分排序，最后对排序好的两个链表进行合并
```java
class Solution {
    public ListNode sortList(ListNode head) {
        //当head为null或仅剩head的时候直接返回
        if (head == null || head.next == null) {
            return head;
        }
        ListNode fast = head;
        ListNode slow = head;
        //快慢指针二分链表
        while (fast.next != null && fast.next.next != null) {
            fast = fast.next.next;
            slow = slow.next;
        }
        fast = slow.next;
        slow.next = null;
        slow = head;
        //对前后部分分别排序
        slow = sortList(slow);
        fast = sortList(fast);
        //排完序后合并两个链表
        return merge(slow, fast);
    }

    ListNode merge(ListNode n1, ListNode n2) {
        if (n1 == null) {
            return n2;
        }
        if (n2 == null) {
            return n1;
        }
        //n1比n2小，则n1指向n1.nex和n2的合并结果，反之也一样
        if (n1.val < n2.val) {
            n1.next = merge(n1.next, n2);
            return n1;
        } else {
            n2.next = merge(n1, n2.next);
            return n2;
        }
    }
}
```