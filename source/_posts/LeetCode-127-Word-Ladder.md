title: LeetCode.127 Word Ladder
author: Matteo
tags: []
categories:
  - LeetCode
date: 2019-05-16 16:10:00
---
[https://leetcode.com/problems/word-ladder/](https://leetcode.com/problems/word-ladder/)
##### 思路
* 开始用dfs递归，不出所望超时了，后来想着用动态规划，做了一上午没有一点头绪。算法实在太难了，真心不适合我😭
> 因为要求最短路径，如果我们用深度优先搜索的话必须遍历所有的路径才能确定哪个是最短的，而用广度优先搜索的话，一旦搜到目标就可以提前终止了，而且根据广度优先的性质，我们肯定是先通过较短的路径搜到目标。另外，为了避免产生环路和重复计算，我们找到一个存在于字典的新的词时，就要把它从字典中移去。这么做是因为根据广度优先，我们第一次发现词A的路径一定是从初始词到词A最短的路径，对于其他可能再经过词A的路径，我们都没有必要再计算了。
[https://segmentfault.com/a/1190000003698569](https://segmentfault.com/a/1190000003698569)
* 代码
```java
public class Solution {

    public int ladderLength(String beginWord, String endWord, Set<String> wordDict) {
        Queue<String> queue = new LinkedList<String>();
        // step用来记录跳数
        int step = 2;
        queue.offer(beginWord);
        while(!queue.isEmpty()){
            int size = queue.size();
            // 控制size来确保一次while循环只计算同一层的节点，有点像二叉树level order遍历
            for(int j = 0; j < size; j++){
               String currWord = queue.poll();
                // 循环这个词从第一位字母到最后一位字母
                for(int i = 0; i < endWord.length(); i++){
                    // 循环这一位被替换成25个其他字母的情况
                    for(char letter = 'a'; letter <= 'z'; letter++){
                        StringBuilder newWord = new StringBuilder(currWord);
                        newWord.setCharAt(i, letter);
                        if(endWord.equals(newWord.toString())){
                            return step;    
                        } else if(wordDict.contains(newWord.toString())){
                            wordDict.remove(newWord.toString());
                            queue.offer(newWord.toString());
                        }
                    }
                } 
            }
            step++;
        }
        return 0;
    }
}
```