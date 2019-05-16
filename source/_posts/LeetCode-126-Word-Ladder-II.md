title: LeetCode.126 Word Ladder II
author: Matteo
date: 2019-05-16 18:31:03
tags:
---
[https://leetcode.com/problems/word-ladder-ii/](https://leetcode.com/problems/word-ladder-ii/)
##### 思路
* 这题可能用126的解法，区别在于这题需要记下所有的最短解，作下面几点修改
  1. queue中需要记下完整路径，每一层往queue中增加最短解的时候需要在新增list里增加当前word
  2. 每一层中前面使用的word也能用于后面的情况，但是后面层不会出现该层及之前使用的word，因为广度优先搜索，每个word第一次出现的肯定是最短路
  3. 当某一层中找到解之后，该层遍历结束后即结束寻找
* 虽然很慢，但还是AC了，后续再看找更优解法

##### 解法
```java
class Solution {
    public List<List<String>> findLadders(String beginWord, String endWord, List<String> wordList) {
        List<List<String>> results = new ArrayList<>();
        //endWord没找到说明没有相应路径
        if (!wordList.contains(endWord)) {
            return results;
        }
        Set<String> wordDict = new HashSet<>(wordList);
        wordDict.remove(beginWord);
        //queue中要记下完整的路径
        Queue<List<String>> queue = new LinkedList<>();
        //将start加入路径
        List<String> start = new ArrayList<>();
        start.add(beginWord);
        queue.offer(start);
        while (!queue.isEmpty()) {
            //一层可能有很多解法，该层中前面使用的word也能用于后面的情况
            Set<String> levelUsed = new HashSet<>();
            int size = queue.size();
            // 控制size来确保一次while循环只计算同一层的节点，有点像二叉树level order遍历
            boolean hasResult = false;
            for (int j = 0; j < size; j++) {
                List<String> curr = queue.poll();
                String currWord = curr.get(curr.size() - 1);
                // 循环这个词从第一位字母到最后一位字母
                for (int i = 0; i < endWord.length(); i++) {
                    // 循环这一位被替换成25个其他字母的情况
                    for (char letter = 'a'; letter <= 'z'; letter++) {
                        StringBuilder newWord = new StringBuilder(currWord);
                        newWord.setCharAt(i, letter);
                        if (endWord.equals(newWord.toString())) {
                            curr.add(endWord);
                            results.add(curr);
                            hasResult = true;
                            //该层中前面使用的word也能用于后面的情况，但是后面层不会出现该层及之前使用的word，因为广度优先搜索，每个word第一次出现的肯定是最短路径
                        } else if (wordDict.contains(newWord.toString()) || levelUsed.contains(newWord.toString())) {
                            levelUsed.add(newWord.toString());
                            wordDict.remove(newWord.toString());
                            List<String> n = new ArrayList<>(curr);
                            n.add(newWord.toString());
                            queue.offer(n);
                        }
                    }
                }
            }
            if (hasResult) {
                break;
            }
        }
        return results;
    }
}
```