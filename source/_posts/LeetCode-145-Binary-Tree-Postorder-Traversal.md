title: LeetCode.145 Binary Tree Postorder Traversal
author: Matteo
tags: []
categories:
  - LeetCode
date: 2019-05-24 19:28:00
---
[https://leetcode.com/problems/binary-tree-postorder-traversal/](https://leetcode.com/problems/binary-tree-postorder-traversal/)
##### 题意
* 后序遍历树，非递归实现，基本跟144一致，只是最后结果需要reverse一下，但是速度比较慢
```java
class Solution {

    List<Integer> results = new ArrayList<>();
    Stack<TreeNode> stacks = new Stack<>();
    public List<Integer> postorderTraversal(TreeNode root) {
        if (root == null) {
            return results;
        }
        stacks.push(root);
        while (!stacks.isEmpty()) {
            TreeNode current = stacks.pop();
            results.add(current.val);
            if (current.right != null) {
                stacks.push(current.right);
            }
            if (current.left != null) {
                stacks.push(current.left);
            }
        }
        Collections.reverse(results);
        return results;
    }
}
```