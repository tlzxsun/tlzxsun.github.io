title: LeetCode.206 Course Schedule
author: Matteo
date: 2019-05-31 16:03:03
tags:
---
[https://leetcode.com/problems/course-schedule/](https://leetcode.com/problems/course-schedule/)

##### 题意
* 选修课程里，基本一门课程可能要先学习另一门课程才能学习，判断是否能完成学习所有课程
##### 暴力解法
* 遇到依赖时直接往递归求解被依赖的课程能否完成学习，完成后需要放入缓存，以备后续计算可能再次计算到这门课程。需要注意的一点就是循环依赖，没想出好的方法，直接在计算后续依赖时加入计数，因为依赖数最大为numCourses-1，当计算依赖次数到达numsCourses肯定是存在循环依赖。
```java
class Solution {
    HashMap<Integer, Boolean> status = new HashMap<>();
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        boolean result = true;
        for (int i = 0; i < numCourses; i++) {
            result &= isEnable(i, prerequisites, 0);
        }
        return result;
    }

    boolean isEnable(int course, int[][] prerequisites, int count) {
        if (count > prerequisites.length) {
            return false;
        }
        if (status.containsKey(course)) {
            return status.get(course);
        }
        boolean enable = true;
        for (int i = 0; i < prerequisites.length; i++) {
            if (prerequisites[i][0] == course) {
                enable &= isEnable(prerequisites[i][1], prerequisites, count + 1);
            }
        }
        status.put(course, enable);
        return enable;
    }
}
```
##### 图论😭😭😭
> 这道课程清单的问题对于我们学生来说应该不陌生，因为我们在选课的时候经常会遇到想选某一门课程，发现选它之前必须先上了哪些课程，这道题给了很多提示，第一条就告诉我们了这道题的本质就是在有向图中检测环。 LeetCode中关于图的题很少，有向图的仅此一道，还有一道关于无向图的题是[ Clone Graph 无向图的复制](http://www.cnblogs.com/grandyang/p/4267628.html)。个人认为图这种数据结构相比于树啊，链表啊什么的要更为复杂一些，尤其是有向图，很麻烦。第二条提示是在讲如何来表示一个有向图，可以用边来表示，边是由两个端点组成的，用两个点来表示边。第三第四条提示揭示了此题有两种解法，DFS和BFS都可以解此题。我们先来看BFS的解法，我们定义二维数组graph来表示这个有向图，一位数组in来表示每个顶点的[入度](http://en.wikipedia.org/wiki/Directed_graph#Indegree_and_outdegree)。我们开始先根据输入来建立这个有向图，并将入度数组也初始化好。然后我们定义一个queue变量，将所有入度为0的点放入队列中，然后开始遍历队列，从graph里遍历其连接的点，每到达一个新节点，将其入度减一，如果此时该点入度为0，则放入队列末尾。直到遍历完队列中所有的值，若此时还有节点的入度不为0，则说明环存在，返回false，反之则返回true。
[https://www.cnblogs.com/grandyang/p/4484571.html](https://www.cnblogs.com/grandyang/p/4484571.html)
```java
class Solution {
    HashMap<Integer, Boolean> status = new HashMap<>();
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        //表示该课程依赖其它课程的数量
        int in[] = new int[numCourses];
        //表示所有依赖key课程的课程
        HashMap<Integer, List<Integer>> graph = new HashMap<>();
        for (int i = 0; i < prerequisites.length; i++) {
            in[prerequisites[i][0]]++;
            List<Integer> list = graph.getOrDefault(prerequisites[i][1], new ArrayList<>());
            list.add(prerequisites[i][0]);
            graph.put(prerequisites[i][1], list);
        }
        Queue<Integer> queue = new LinkedList<>();
        for (int i = 0; i < numCourses; i++) {
            //将所有不依赖其他课程的课程加入队列
            if (in[i] == 0) {
                queue.offer(i);
            }
        }
        while (!queue.isEmpty()) {
            //从队列中取出不依赖其他课程的课程
            int value = queue.poll();
            //取出所有信赖该课程的课程
            List<Integer> list = graph.get(value);
            if (list != null) {
                for (Integer vv: list) {
                    //将依赖value课程的课程的依赖数减1，因为value课程已经完成学习
                    in[vv]--;
                    //假如vv课程的依赖数为0，说明该课程已经完成学习，将该课程加入queue中
                    if (in[vv] == 0) {
                        queue.offer(vv);
                    }
                }
            }
        }
        //所有能学习的课程及依赖他们的课程都学习完后假如仍有课程没有完成学习，那么说明存在循环依赖，不能完成所有课程的学习
        for (int i = 0; i < numCourses; i++) {
            if (in[i] != 0) {
                return false;
            }
        }
        return true;
    }
}
```