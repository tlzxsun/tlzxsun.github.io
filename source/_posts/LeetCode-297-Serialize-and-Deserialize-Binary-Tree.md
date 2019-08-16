title: LeetCode.297 Serialize and Deserialize Binary Tree
author: Matteo
date: 2019-06-20 11:26:53
tags:
---
[https://leetcode.com/problems/serialize-and-deserialize-binary-tree/](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/)

##### 题意
* 实现Codec中序列化与反序列二叉树方法
##### 解法
* 序列化时采用按层次遍历，其中左右子结点为空时也加入结果
* 反序列化时
1. 将第一个结点加入queue
2. queue中出列，如果结点不为null，刚从结果中取两个结点，分别赋值给结点的左右子树
3. 重复2直至queue为空
```java
class Codec {

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        if (root == null) {
            return null;
        }
        Queue<TreeNode> queue = new LinkedList<>();
        List<TreeNode> nodes = new ArrayList<>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            TreeNode node = queue.poll();
            nodes.add(node);
            if (node != null) {
                queue.offer(node.left);
                queue.offer(node.right);
            }
        }
        StringBuilder builder = new StringBuilder(String.valueOf(nodes.get(0).val));
        for (int i = 1; i < nodes.size(); i++) {
            builder.append(",");
            if (nodes.get(i) == null) {
                builder.append("null");
            } else {
                builder.append(nodes.get(i).val);
            }
        }
        System.out.println(builder.toString());
        return builder.toString();
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        if (data == null) {
            return null;
        }
        String parts[] = data.split(",");
        int cur = 0;
        int val = Integer.parseInt(parts[cur++]);
        TreeNode node = new TreeNode(val);
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(node);
        while (!queue.isEmpty()) {
            TreeNode n = queue.poll();
            if (n != null) {
                TreeNode l = null, r = null;
                if (!"null".equals(parts[cur])) {
                    l = new TreeNode(Integer.parseInt(parts[cur]));
                    queue.offer(l);
                }
                cur++;
                if (!"null".equals(parts[cur])) {
                    r = new TreeNode(Integer.parseInt(parts[cur]));
                    queue.offer(r);
                }
                cur++;
                n.left = l;
                n.right = r;
            }
        }

        return node;
    }
}
```