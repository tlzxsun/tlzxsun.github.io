title: LeetCode.199 Binary Tree Right Side View
author: Matteo
date: 2019-05-29 21:04:46
tags:
---
[https://leetcode.com/problems/binary-tree-right-side-view/](https://leetcode.com/problems/binary-tree-right-side-view/)
##### 题意
* 按层次输出二叉树最右结点
##### 解法
1. BFS
按层次遍历BFS一般用queue作为辅助，先将根结点入列，然后依次出列，每次将出列结点的左右结点再加入队列。这里需要记下最右结点，需要做一个调整，普通遍历的时候并没有区分层次。这里需要每次while循环时记下当前结点数量个数，然后将当前所有结点出列，并将左右结点入列，每次while循环时的结点数量就是该层的数量，将该层第一个结点加入结果即可。
```java
class Solution {
    List<Integer> results = new ArrayList<>();
    public List<Integer> rightSideView(TreeNode root) {
        if (root == null) {
            return results;
        }
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                TreeNode node = queue.poll();
                if (i == size - 1) {
                    results.add(node.val);
                }
                if (node.left != null) {
                    queue.offer(node.left);
                }
                if (node.right != null) {
                    queue.offer(node.right);
                }
            }
        }
        return results;
    }
}
```
2. DFS
递归求解，先遍历右子树再遍历左子树，遍历时传入当前的层次，之前的做法是用一个set来保存每一层是否被遍历过。网上看到更巧妙的办法只需要判断当前层次是否跟结果集的size一致，因为每次遍历到新的一层的时候，结果集中只有之前层的结果，level刚好跟size一致。
```java
class Solution {
    List<Integer> results = new ArrayList<>();
    public List<Integer> rightSideView(TreeNode root) {
        traversal(root, 0);
        return results;
    }

    void traversal(TreeNode root, int level) {
        if (root != null) {
            if (level == results.size()) {
                results.add(root.val);
            }
            traversal(root.right, level + 1);
            traversal(root.left, level + 1);
        }
    }
}
```