title: LeetCode.315 Count of Smaller Numbers After Self
author: Matteo
date: 2019-07-11 10:29:00
tags:
---
[https://leetcode.com/problems/count-of-smaller-numbers-after-self/](https://leetcode.com/problems/count-of-smaller-numbers-after-self/)
##### 题意
* 求一个数组中每个位置之后比当前元素小的元素的个数
##### 思路
* 暴力递归居然也能AC～～～
* 参考别人思路采用二叉搜索树，在构建树的同时计算比当前节点小的个数
  1. 当前节点root为空，则构建一个新节点，新节点smaller为0，返回新节点
  2. 当前值比当前节点小，那么当前节点smaller++，返回插入当前节点左子树的结果
  3. 当前值不小于当前节点值，那么返回插入当前节点右子树的结果+当前结点的smaller+当前值大于当前节点值?1:0（因为此时节点需要插入右子树，所以比当前结点小的必定比插入的节点小）
```C++
// Binary Search Tree
class Solution {
public:
    struct Node {
        int val, smaller;
        Node *left, *right;
        Node(int v, int s) : val(v), smaller(s), left(NULL), right(NULL) {}
    };
    int insert(Node*& root, int val) {
        if (!root) return (root = new Node(val, 0)), 0;
        if (root->val > val) return root->smaller++, insert(root->left, val);
        return insert(root->right, val) + root->smaller + (root->val < val ? 1 : 0);
    }
    vector<int> countSmaller(vector<int>& nums) {
        vector<int> res(nums.size());
        Node *root = NULL;
        for (int i = nums.size() - 1; i >= 0; --i) {
            res[i] = insert(root, nums[i]);
        }
        return res;
    }
};
```
* C++可以直接传参数的指针?，在方法中修改参数指针的指针地址，即可修改原对象，但是Java中参数传递的是参数的引用，修改参数的引用并不能修改原参数。因此java的实现会有所不用，这里给value赋了Integer.MIN_VALUE，当取到该值时说明该对象未被处理～

```java
class Solution {


    List<Integer> results = new ArrayList<>();
    HashMap<Long, N> map = new HashMap<>();
//    public List<Integer> countSmaller(int[] nums) {
//        N root = null;
//        for (int i = nums.length - 1; i >= 0; i --) {
//            results.add(0, insert(root, nums[i]));
//        }
//        return results;
//    }
//
//    int insert(N node, int value) {
//        if (node == null) {
//            node = new N(value, 0);
//            return 0;
//        } else {
//            if (value < node.value) {
//                node.smaller++;
//                return insert(node.left, value);
//            } else {
//                return insert(node.right, value) + node.smaller + (node.value < value ? 1 : 0);
//            }
//        }
//    }

    public List<Integer> countSmaller(int[] nums) {
        N root = new N(Integer.MIN_VALUE, 0);
        for (int i = nums.length - 1; i >= 0; i --) {
            results.add(0, insert(root, nums[i]));
        }
        return results;
    }

    int insert(N uuid, int value) {
        N node = uuid;
        if (node.value == Integer.MIN_VALUE) {
            node.value = value;
            node.smaller = 0;
            return 0;
        } else {
            if (value < node.value) {
                node.smaller++;
                if (node.left == null) {
                    node.left = new N(Integer.MIN_VALUE, 0);
                }
                return insert(node.left, value);
            } else {
                if (node.right == null) {
                    node.right = new N(Integer.MIN_VALUE, 0);
                }
                return insert(node.right, value) + node.smaller + (node.value < value ? 1 : 0);
            }
        }
    }

    class N {

        N(int value, int smaller) {
            this.value = value;
            this.smaller = smaller;
        }

        int value, smaller;
        N left, right;
    }
}
```