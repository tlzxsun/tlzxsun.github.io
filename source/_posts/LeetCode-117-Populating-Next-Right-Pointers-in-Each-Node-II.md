title: LeetCode.117 Populating Next Right Pointers in Each Node II
author: Matteo
date: 2019-05-15 09:20:54
tags:
---
[https://leetcode.com/problems/populating-next-right-pointers-in-each-node-ii/](https://leetcode.com/problems/populating-next-right-pointers-in-each-node-ii/)
##### 题意
 * 这一题与116的区别就在于这一题不是满二叉树，所以下一行的起点不是第一个节点的左结点，因此不能用116中的解法。最笨的办法就是用116的方法先层次遍历将每一个结点的next指向下一个遍历到的，然后再右序遍历将最右边结点的next清空，时间复杂度为O(2n)，空间复杂度为O(n)，这样也能AC😂
 
```java
class Solution {

    //上一个遍历的元素
    Node last;

    public Node connect(Node root) {
        if (root != null) {
            //给root结点加上标签
            LinkedList<Node> queue = new LinkedList<>();
            //将root结点入列
            queue.offer(root);
            while (!queue.isEmpty()) {
                //取出队列中首个结点
                Node node = queue.poll();
                //将右结点入列，这个结点将在之后被遍历
                if (node.right != null) {
                    queue.offer(node.right);
                }
                //将左子结点入列
                if (node.left != null) {
                    queue.offer(node.left);
                }
                node.next = last;
                //将上一个访问的结点设为当前结点
                last = node;
            }
            resetRight(root, 0);
        }
        return root;
    }
    
    HashSet<Integer> rights = new HashSet<>();
    void resetRight(Node root,int level) {
        if (root != null) {
            resetRight(root.right, level + 1);
            if (!rights.contains(level)) {
                root.next = null;
                rights.add(level);
            }
            resetRight(root.left, level + 1);
        }
    }
}
```
##### 比较好的方案
* 采用三个变量cur，head和pre，cur用来做层次遍历，head用来保存每一行的起始结点，pre用来保存上一个遍历到的结点，时间复杂度为O(n)，空间复杂度为O(1)

```java
class Solution {

    public Node connect(Node root) {
        if (root != null) {
            //cur用来遍历，head用来保存每一层的起始结节，pre用来保存上一个结点
            Node cur = root, pre = null, head = null;
            //外层循环用来判断是否遍历完成
            while (cur != null) {
                //内层循环用来判断该层是否遍历完成
                while (cur != null) {
                    //左结点不为空
                    if (cur.left != null) {
                        //上一个结点为空，说明是一层起点，给head和pre赋值
                        if (pre == null) {
                            pre = cur.left;
                            head = cur.left;
                        } else {
                            //否则将pre.next指向当前，并将pre.next赋值给pre
                            pre.next = cur.left;
                            pre = pre.next;
                        }
                    }
                    //同左结点
                    if (cur.right != null) {
                        if (pre == null) {
                            pre = cur.right;
                            head = cur.right;
                        } else {
                            pre.next = cur.right;
                            pre = pre.next;
                        }
                    }
                    //将cur指向cur.next遍历该层其它子树结点
                    cur = cur.next;
                }
                //一层遍历完成后将cur指向下一层起始节点，并初始化pre和head
                cur = head;
                pre = null;
                head = null;
            }
        }
        return root;
    }
}
```