title: LeetCode.99 Recover Binary Search Tree
author: Matteo
date: 2019-05-13 18:30:36
tags:
---
[https://leetcode.com/problems/recover-binary-search-tree/](https://leetcode.com/problems/recover-binary-search-tree/)
* 二叉搜索树中序遍历(In-Order Traversal)必定有序

> 先来看一个naive的方法，即使用O(n)的空间复杂度应该怎么解决．
>我们可以开一个数组，中序遍历树，并将其依次保存在数组中．如果是一个正常的二叉搜索树将会是完全有序的．然而当交换了其中两个结点之后就会出现两个结点出现在错误的位置．举个栗子：[1, 2, 3, 4, 5, 6]，这是一个正常的二叉搜索数的序列，当交换了其中两个结点之后变成了[1, 5, 3, 4, 2, 6]．
那么如何找到这两个交换的结点呢？我们知道如果一个大的数移到了前面，他必然会比他后面的一个数大，也就是说当我们遍历数组的时候碰到的第一个比后面结点大的结点就是被交换过来的大的结点．那么交换到后面的小的结点也可以用同样的道理找到，即这个小的结点必然比前面一个结点小，但是如果交换的两个结点不是相邻结点的话会有两个结点比前面结点小，因为大的结点交换到前面来他必然比后面一个结点大，然后真正的交换过去的小的结点也会比前面的小．所以我们要找到的是最后一个比前面结点小的结点．
有了上面的理论我们就可以很容易的在数组中找到两个失序的结点了．然后将其值交换就可以了．
好了，既然我们已经知道了使用O(n)空间复杂度的算法了，我们就可以不用将结点保存下来了，因为我们可以直接用中序遍历直接找到这两个结点，但是我们需要一个另外的标记，即当前结点的前一个结点，这样就可以还原上面的算法了．
[https://blog.csdn.net/qq508618087/article/details/50516683](https://blog.csdn.net/qq508618087/article/details/50516683)

```java
class Solution {
//    List<Integer> vals = new ArrayList<>();
//    Integer v1, v2;
//    public void recoverTree(TreeNode root) {
//        inOrder(root);
//        for (int i = 0; i < vals.size(); i++) {
//            if (i < vals.size() - 1 && vals.get(i) > vals.get(i + 1) && v1 == null) {
//                v1 = vals.get(i);
//            }
//            if (i > 0 && vals.get(i) < vals.get(i - 1)) {
//                v2 = vals.get(i);
//            }
//        }
//        fix(root);
//    }
//
//    void fix(TreeNode root) {
//        if (root != null) {
//            if (root.left != null) {
//                fix(root.left);
//            }
//            if  (root.val == v2) {
//                root.val = v1;
//            } else if (root.val == v1) {
//                root.val = v2;
//            }
//            if (root.right != null) {
//                fix(root.right);
//            }
//        }
//    }
//
//    void inOrder(TreeNode root) {
//        if (root != null) {
//            if (root.left != null) {
//                inOrder(root.left);
//            }
//            vals.add(root.val);
//            if (root.right != null) {
//                inOrder(root.right);
//            }
//        }
//    }

    TreeNode t1, t2, last;

    public void recoverTree(TreeNode root) {
        inOrder(root);
        t1.val = t1.val^t2.val;
        t2.val = t1.val^t2.val;
        t1.val = t1.val^t2.val;
    }

    void inOrder(TreeNode root) {
        if (root != null) {
            inOrder(root.left);
            if (last != null && last.val > root.val) {
//t1和t2可能在一次出现！！！
//                if (t1 == null) {
//                    t1 = last;
//                } else {
//                    t2 = root;
//                }
                if (t1 == null) {
                    t1 = last;
                }
                t2 = root;
            }
//当遍历了当前结点后给last结点赋值
            last = root;
            inOrder(root.right);
        }
    }

}
```