title: LeetCode.143 Reorder List
author: Matteo
tags: []
categories:
  - LeetCode
date: 2019-05-24 17:38:00
---
##### 题意
* [https://leetcode.com/problems/reorder-list/](https://leetcode.com/problems/reorder-list/)
* 给链表重新排序，顺序变为L0→Ln→L1→Ln-1→L2→Ln-2→…
##### 笨办法
* 每次找到最后一个节点，并断开最后一个节点与上一个节点连接，使第一个节点指向最后一个节点，最后一个节点指向第一个节点的原next节点。虽然能AC但是极慢
```java
class Solution {
    public void reorderList(ListNode head) {
        if (head == null || head.next == null) {
            return;
        }
        ListNode tmp;
        //head不为null，且节点数大于2
        while (head != null && head != (tmp = findLast(head))) {
            //最后一个结点指向head.next
            tmp.next = head.next;
            //head.next指向找到的最后一个节点
            head.next = tmp;
            //head变为head.next
            head = tmp.next;
        }
    }

    //找到head打头的最后一个结点
    ListNode findLast(ListNode head) {
        if (head.next == null) {
            return head;
        }
        while (true) {
            if (head.next != null && head.next.next == null) {
                ListNode find =  head.next;
                //断开上一个节点的连接
                head.next = null;
                return find;
            } else {
                head = head.next;
            }
        }
    }
}
```
##### 好解法
> 先使用快慢指针将链表从中间分割成两段，然后后半段就地逆置．之后合并插入到前半段链表即可，时间复杂度O(n)。
[https://blog.csdn.net/qq508618087/article/details/50480251](https://blog.csdn.net/qq508618087/article/details/50480251)
```java
class Solution {
    public void reorderList(ListNode head) {
        if (head == null || head.next == null) {
            return;
        }
        //快慢指针，慢指针最终指向((n - 1)/2)
        ListNode fast = head, slow = head;
        while (fast.next != null && fast.next.next != null) {
            fast = fast.next.next;
            slow = slow.next;
        }
        //快指针指向后半部分
        fast = slow.next;
        //断开前后部分连接
        slow.next = null;
        //慢指针指向head
        slow = head;
        //反转后半部分
        fast = reverse(fast);
        while (fast != null && slow != null) {
            ListNode tf = fast.next, ts = slow.next;
            //快指针next指向慢指针的next
            fast.next = slow.next;
            //慢指针next指向快指针
            slow.next = fast;
            fast = tf;
            slow = ts;
        }
    }

    ListNode reverse(ListNode head) {
        ListNode pre = head, cur = head.next, tmp;
        while (cur != null) {
            tmp = cur.next;
            cur.next = pre;

            pre = cur;
            cur = tmp;
        }
        head.next = null;
        return pre;
    }
}
```