title: LeetCode.232 Implement Queue using Stacks
author: Matteo
date: 2019-06-05 10:56:22
tags:
---
[https://leetcode.com/problems/implement-queue-using-stacks/submissions/](https://leetcode.com/problems/implement-queue-using-stacks/submissions/)
##### 题意
* 用Stack来实现队列的功能
##### 解法
* 采用两个Stack sPush和sPop来实现，push时加进sPush，pop时判断sPop是否为空，否则将sPush中的所有元素加进sPop，再从sPop中pop
```java
class MyQueue {

    Stack<Integer> push = new Stack<>();
    Stack<Integer> pop = new Stack<>();

    /**
     * Initialize your data structure here.
     */
    public MyQueue() {

    }

    /**
     * Push element x to the back of queue.
     */
    public void push(int x) {
        push.push(x);
    }

    /**
     * Removes the element from in front of queue and returns that element.
     */
    public int pop() {
        if (pop.isEmpty()) {
            while (!push.isEmpty()) {
                pop.push(push.pop());
            }
        }
        return pop.pop();
    }

    /**
     * Get the front element.
     */
    public int peek() {
        if (pop.isEmpty()) {
            while (!push.isEmpty()) {
                pop.push(push.pop());
            }
        }
        return pop.peek();
    }

    /**
     * Returns whether the queue is empty.
     */
    public boolean empty() {
        return push.isEmpty() && pop.isEmpty();
    }
}
```