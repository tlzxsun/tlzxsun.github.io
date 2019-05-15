title: 'LeetCode.116 Populating Next Right '
author: Matteo
date: 2019-05-14 18:40:15
tags:
---
[https://leetcode.com/problems/populating-next-right-pointers-in-each-node/](https://leetcode.com/problems/populating-next-right-pointers-in-each-node/)
* 这题一开始想着使用按层遍历，但是不知道怎么遍历☹️，经查阅后得知使用LinkedList实现。
* 给root的next增加一个特殊标签，当取到next为标签时，说明这个元素是每行第一个，并给它的右结点增加标签，因为右结点是下一层第一个遍历的(满二叉树才行)
* AC代码
```java
class Solution {

    //上一个遍历的元素
    Node last;
    //第一行起始结点的特殊标签
    Node mark = new Node();

    public Node connect(Node root) {
        if (root != null) {
            //给root结点加上标签
            root.next = mark;
            LinkedList<Node> queue = new LinkedList<>();
            //将root结点入列
            queue.offer(root);
            while (!queue.isEmpty()) {
                //取出队列中首个结点
                Node node = queue.poll();
                //将右结点入列，这个结点将在之后被遍历
                if (node.right != null) {
                    queue.offer(node.right);
                    //前提这是一个满二叉树
                    //假如这个结点有特殊标签，说明这是一行的第一个标签，则给他的右结点增加标签，因为右结点是下一行的起始位置
                    if (node.next == mark) {
                        node.right.next = mark;
                    }
                }
                //将左子结点入列
                if (node.left != null) {
                    queue.offer(node.left);
                }

                //有特殊标签则next为null
                if (node.next == mark) {
                    node.next = null;
                } else {
                    //否则为上一个结点
                    node.next = last;
                }
                //将上一个访问的结点设为当前结点
                last = node;
            }
        }
        return root;
    }
}
```