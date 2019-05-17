title: LeetCode.133 Clone Graph
author: Matteo
date: 2019-05-17 15:58:05
tags:
---
[https://leetcode.com/problems/clone-graph/](https://leetcode.com/problems/clone-graph/)
##### 思路
* 头昏眼花想不出任何方法，看了别人思路发现好简单
>1、广度优先遍历(BFS)
2、深度优先遍历(DFS)
2.1、递归
2.2、非递归
[https://www.cnblogs.com/ganganloveu/p/4119462.html](https://www.cnblogs.com/ganganloveu/p/4119462.html)
##### 解法
```java
class Solution {
    HashMap<Node, Node> map = new HashMap<>();
    public Node cloneGraph(Node node) {
        if (node == null) {
            return null;
        }
        if (map.containsKey(node)) {
            return map.get(node);
        }
        Node clone = new Node(node.val, new ArrayList<>());
        map.put(node, clone);
        for (Node n: node.neighbors) {
            Node temp = cloneGraph(n);
            clone.neighbors.add(temp);
        }
        return clone;
    }
}
```