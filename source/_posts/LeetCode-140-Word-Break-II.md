title: LeetCode.140 Word Break II
author: Matteo
tags: []
categories:
  - LeetCode
date: 2019-05-24 13:24:00
---
##### 题意
* 给一个字符串和一个字符串数组，判断字符串是否能由数组的字符串组合而成，可以重复使用同时不必全部使用，并输出所有解
##### 思路
* 一开始想着跟上一题一样的解法，DP然后记录所有解，但是想想要输出所有解，递归应该可以，很意外的还是TLE了😫
```java
class Solution {

    List<String> results = new ArrayList<>();
    List<String> result = new ArrayList<>();

    public List<String> wordBreak(String s, List<String> wordDict) {
        search(s, wordDict, 0);
        return results;
    }

    void search(String s, List<String> wordDict, int start) {
        for (int i = 0; i < wordDict.size(); i++) {
            if (s.startsWith(wordDict.get(i), start)) {
                if (start + wordDict.get(i).length() == s.length() && s.endsWith(wordDict.get(i))) {
                    StringBuilder builder = new StringBuilder();
                    for (String word: result) {
                        builder.append(word).append(" ");
                    }
                    builder.append(wordDict.get(i));
                    results.add(builder.toString().trim());
                }
                result.add(wordDict.get(i));
                search(s, wordDict, start + wordDict.get(i).length());
                result.remove(result.size() - 1);
            }
        }
    }
}
```
##### 正确打开方式
* 采用DP时记录当前结果，假如直接DP会TLE，于是先判断是否能有解再DP记录当前结果。虽然能AC但是非常慢，肯定有更好的解法，后来尝试了先判断是否有解再采用递归，发现时间跟采用DP基本一致，问题应该是出在DP记录解的时候
```java
class Solution {

    Map<Integer, List<String>> map = new HashMap<>();

    public List<String> wordBreak(String s, List<String> wordDict) {
        //先判是否有解
        if (!canBreak(s, wordDict)) {
            return new ArrayList<>();
        }
        int length = s.length();
        //dp用来记录到dp[i]为止是否符合条件
        boolean dp[] = new boolean[length + 1];
        //dp[0]可以理解为空字符串肯定符合条件
        dp[0] = true;
        for (int i = 1; i <= length; i++) {
            //假如dp[j]符合条件，且s[j,i)也符合条件，那么dp[i]也是符合条件的
            for (int j = 0; j < i; j++) {
                String temp = s.substring(j, i);
                if (dp[j] && wordDict.contains(temp)) {
                    //获取当前位置的结果集
                    List<String> current = map.getOrDefault(i, new ArrayList<>());
                    //假如s[j,i)存在，刚直接加到结果集中
                    if (j == 0) {
                        current.add(temp);
                    } else {
                        //取得到j位置时的结果集，并在每个结果中增加当前temp
                        List<String> last = map.getOrDefault(j, new ArrayList<>());
                        for (int k = 0; k < last.size(); k++) {
                            current.add(last.get(k) + " " + temp);
                        }
                    }
                    map.put(i, new ArrayList<>(current));
                    dp[i] = true;
                }
            }
        }
        return map.get(length);
    }

    public boolean canBreak(String s, List<String> wordDict) {
        int length = s.length();
        boolean dp[] = new boolean[length + 1];
        dp[0] = true;
        for (int i = 1; i <= length; i++) {
            for (int j = 0; j < i; j++) {
                String temp = s.substring(j, i);
                if (dp[j] && wordDict.contains(temp)) {
                    dp[i] = true;
                    break;
                }
            }
        }
        return dp[length];
    }
}
```
##### 更好的解法
> 根据老夫行走江湖多年的经验，像这种返回结果要列举所有情况的题，十有八九都是要用递归来做的。当我们一时半会没有啥思路的时候，先不要考虑代码如何实现，如果就给你一个s和wordDict，不看Output的内容，你会怎么找出结果。比如对于例子1，博主可能会先扫一遍wordDict数组，看有没有单词可以当s的开头，那么我们可以发现cat和cats都可以，比如我们先选了cat，那么此时s就变成了 "sanddog"，我们再在数组里找单词，发现了sand可以，最后剩一个dog，也在数组中，于是一个结果就出来了。然后回到开头选cats的话，那么此时s就变成了 "anddog"，我们再在数组里找单词，发现了and可以，最后剩一个dog，也在数组中，于是另一个结果也就出来了。那么这个查询的方法很适合用递归来实现，因为s改变后，查询的机制并不变，很适合调用递归函数。再者，我们要明确的是，如果不用记忆数组做减少重复计算的优化，那么递归方法跟brute force没什么区别，大概率无法通过OJ。所以我们要避免重复计算，如何避免呢，还是看上面的分析，如果当s变成 "sanddog"的时候，那么此时我们知道其可以拆分成sand和dog，当某个时候如果我们又遇到了这个 "sanddog"的时候，我们难道还需要再调用递归算一遍吗，当然不希望啦，所以我们要将这个中间结果保存起来，由于我们必须要同时保存s和其所有的拆分的字符串，那么可以使用一个HashMap，来建立二者之间的映射，那么在递归函数中，我们首先检测当前s是否已经有映射，有的话直接返回即可，如果s为空了，我们如何处理呢，题目中说了给定的s不会为空，但是我们递归函数处理时s是会变空的，这时候我们是直接返回空集吗，这里有个小trick，我们其实放一个空字符串返回，为啥要这么做呢？我们观察题目中的Output，发现单词之间是有空格，而最后一个单词后面没有空格，所以这个空字符串就起到了标记当前单词是最后一个，那么我们就不要再加空格了。接着往下看，我们遍历wordDict数组，如果某个单词是s字符串中的开头单词的话，我们对后面部分调用递归函数，将结果保存到rem中，然后遍历里面的所有字符串，和当前的单词拼接起来，这里就用到了我们前面说的trick。for循环结束后，记得返回结果res之前建立其和s之间的映射，方便下次使用
[https://www.cnblogs.com/grandyang/p/4576240.html](https://www.cnblogs.com/grandyang/p/4576240.html)

```java
class Solution {

//    class Solution {
//        public:
//        vector<string> wordBreak(string s, vector<string>& wordDict) {
//            unordered_map<string, vector<string>> m;
//            return helper(s, wordDict, m);
//        }
//        vector<string> helper(string s, vector<string>& wordDict, unordered_map<string, vector<string>>& m) {
//            if (m.count(s)) return m[s];
//            if (s.empty()) return {""};
//            vector<string> res;
//            for (string word : wordDict) {
//                if (s.substr(0, word.size()) != word) continue;
//                vector<string> rem = helper(s.substr(word.size()), wordDict, m);
//                for (string str : rem) {
//                    res.push_back(word + (str.empty() ? "" : " ") + str);
//                }
//            }
//            return m[s] = res;
//        }
//    };


    public List<String> wordBreak(String s, List<String> wordDict) {
        HashMap<String, List<String>> m = new HashMap<>();
        return search(s, wordDict, m);
    }

    List<String> search(String s, List<String> wordDict, Map<String, List<String>> m) {
        if (m.containsKey(s)) {
            return m.get(s);
        }
        if (s.isEmpty()) {
            return Arrays.asList(new String[]{""});
        }
        //表示s的结果
        List<String> res = new ArrayList<>();
        for (String word : wordDict) {
            //如果s以word开头，那么结果就是s加上剩下的字符串的结果
            if (s.startsWith(word)) {
                //递归调用
                List<String> rem = search(s.substring(word.length()), wordDict, m);
                //假如剩下的结果不为空，刚证明为word开头有解，将结果放入res
                for (String str : rem) {
                    res.add(word + (str.isEmpty() ? "" : " ") + str);
                }
            }
        }
        m.put(s, res);
        return res;
    }
}

```