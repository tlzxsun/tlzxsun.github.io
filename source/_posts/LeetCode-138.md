title: 'LeetCode.138 Copy List with Random Pointer  '
author: Matteo
tags: []
categories:
  - LeetCode
date: 2019-05-20 15:48:00
---
##### 题意
拷贝一个结点的所有子结点，与前面有一题一样的解法
```java
class Solution {

    Map<Node, Node> copied = new HashMap<>();

    public Node copyRandomList(Node head) {
        if (head == null) {
            return null;
        }
        if (copied.containsKey(head)) {
            return copied.get(head);
        }
        Node temp = new Node();
        copied.put(head, temp);
        temp.val = head.val;
        temp.next = copyRandomList(head.next);
        temp.random = copyRandomList(head.random);
        return temp;
    }
}
```