title: LeetCode.337 House Robber III
author: Matteo
date: 2019-05-29 20:23:16
tags:
---
[https://leetcode.com/problems/house-robber-iii/](https://leetcode.com/problems/house-robber-iii/)
##### 题意
* 还是专业劫匪打劫，不能抢相邻的房子，但是这次房子的结构不再是线性而是一个二叉树了。
##### 解法
* 开始想着把二叉树先展开，发现不行。后来想到用递归来实现，递归的思路就比较简单了，大致上就是打劫当前结点所得加上打劫左右子树的左右子树的结果相加与打劫当前结点左右子树结果相加取大的那个。先完后发现代码好少，但是提交后时间非常长，因为递归每次都会去计算，随着层次变深，结点的遍历次数也在递增，加上缓存后时间缩小到原来的百分之一。
```java
class Solution {
    
    HashMap<TreeNode, Integer> cache = new HashMap<>();
    
    public int rob(TreeNode root) {
        if (root != null) {
            if (cache.containsKey(root)) {
                return cache.get(root);
            }
            int current = root.val;
            if (root.left != null) {
                current += rob(root.left.left);
                current += rob(root.left.right);
            }
            if (root.right != null) {
                current += rob(root.right.left);
                current += rob(root.right.right);
            }
            int next = rob(root.left) + rob(root.right);
            int max = Math.max(next, current);
            cache.put(root, max);
            return max;
        }
        return 0;
    }
}
```