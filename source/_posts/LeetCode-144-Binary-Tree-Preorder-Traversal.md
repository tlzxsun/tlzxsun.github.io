title: LeetCode.144 Binary Tree Preorder Traversal
author: Matteo
date: 2019-05-24 18:09:05
tags:
---
[https://leetcode.com/problems/binary-tree-preorder-traversal/](https://leetcode.com/problems/binary-tree-preorder-traversal/)
##### 题意
* 二叉树前序遍历，递归的比较简单，非递归实现需要用栈存节点
```java
class Solution {

    List<Integer> results = new ArrayList<>();
    Stack<TreeNode> stacks = new Stack<>();
    public List<Integer> preorderTraversal(TreeNode root) {
        if (root == null) {
            return results;
        }
        //根节点入栈
        stacks.push(root);
        while (!stacks.isEmpty()) {
            //出栈
            TreeNode current = stacks.pop();
            //加入结果集
            results.add(current.val);
            //将右节点加入stack，之后遍历
            if (current.right != null) {
                stacks.push(current.right);
            }
            //将左节点加入stack，在右节点之前遍历
            if (current.left != null) {
                stacks.push(current.left);
            }
        }
        return results;
    }
}
```