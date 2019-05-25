title: LeetCode.147 Insertion Sort List
author: Matteo
date: 2019-05-24 21:04:08
tags:
---
[https://leetcode.com/problems/insertion-sort-list/](https://leetcode.com/problems/insertion-sort-list/)

##### 题意
* 使用插入排序对链表进行排序
##### 笨办法
* 每次找错序的节点，断开该节点后从头开始查找合适位置并插入，能AC但是非常慢
```java
class Solution {
    public ListNode insertionSortList(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        ListNode cur = head;
        //是否查找到最后一个节点
        while (cur.next != null) {
            //当前节点的值大于后一节点值
            if (cur.next.val < cur.val) {
                //当前结点的后一节点的位置需要调整
                ListNode modify = cur.next;
                //当前节点指向后一节点的后一节点，相当于断掉当前节后的后一节点
                cur.next = cur.next.next;
                //临时节点指向头结点
                ListNode tmp = head;
                //用于记录上一个遍历到的节点
                ListNode last = null;
                while (true) {
                    //假如调整节点的值小于临时节点
                    if (modify.val < tmp.val) {
                        //调整节点的下一节点设为临时节点
                        modify.next = tmp;
                        //假如临时节点不是头部节点
                        if (last != null) {
                            last.next = modify;
                        } else {
                            //调整节点需要放在头部
                            head = modify;
                        }
                        break;
                    }
                    //给上一节点赋值
                    last = tmp;
                    //继续查找下一节点
                    tmp = tmp.next;
                }
                //cur仍旧从头部开始查找
                cur = head;
            } else {
                //没有情况则继续向下查找
                cur = cur.next;
            }
        }
        return head;
    }
}
```
##### 好解法
* To be continued
* 同148