title: LeetCode.224 Basic Calculator
author: Matteo
date: 2019-06-04 16:46:38
tags:
---
[https://leetcode.com/problems/basic-calculator/](https://leetcode.com/problems/basic-calculator/)
##### 题意
* 实现一个带“（”，“）”，“+”，“-”运算符的简易计算器
##### 解法
* 不难但是好复杂
```java
class Solution {
    public int calculate(String s) {
        s = s.replaceAll("\\s*", "");
        int i = 0;
        Stack<String> stack = new Stack<>();
        while ( i < s.length()) {
            if (s.charAt(i) == '(') {
                //将(入栈
                stack.push("(");
                i++;
            } else if (s.charAt(i) == ')') {
                //将上一个结果出栈
                String current = stack.pop();
                //pop "("，弹出（
                stack.pop();
                //判断stack是否还有运算符和数字
                if (!stack.isEmpty()) {
                    String op = stack.pop();
                    String last = stack.pop();
                    if ("+".equals(op)) {
                        stack.push(String.valueOf(Integer.parseInt(last) + Integer.parseInt(current)));
                    } else {
                        stack.push(String.valueOf(Integer.parseInt(last) - Integer.parseInt(current)));
                    }
                } else {
                    //将当前结果入栈
                    stack.push(current);
                }
                i++;
            } else if (s.charAt(i) == '+') {
                //判断后一个是否是(，不是则计算与上一个的结果并入栈，否则将算符入栈
                if (s.charAt(i + 1) != '(') {
                    int end = getNextNumber(s, i + 1);
                    String current = s.substring(i+1, end);
                    String last = stack.pop();
                    stack.push(String.valueOf(Integer.parseInt(last) + Integer.parseInt(current)));
                    i = end;
                } else {
                    stack.push("+");
                    i+=1;
                }
            } else if (s.charAt(i) == '-') {
                if (s.charAt(i + 1) != '(') {
                    int end = getNextNumber(s, i + 1);
                    String current = s.substring(i+1, end);
                    String last = stack.pop();
                    stack.push(String.valueOf(Integer.parseInt(last) - Integer.parseInt(current)));
                    i = end;
                } else {
                    stack.push("-");
                    i+=1;
                }
            } else {
                int end = getNextNumber(s, i);
                //拿出当前数字
                String current = s.substring(i, end);
                //判断栈中是否还有其它算符
                if (!stack.isEmpty() && !"(".equals(stack.peek())) {
                    String op = stack.pop();
                    String last = stack.pop();
                    if ("+".equals(op)) {
                        stack.push(String.valueOf(Integer.parseInt(last) + Integer.parseInt(current)));
                    } else {
                        stack.push(String.valueOf(Integer.parseInt(last) - Integer.parseInt(current)));
                    }
                } else {
                    stack.push(current);
                }
                i = end;
            }
        }
        return Integer.parseInt(stack.pop());
    }

    public int getNextNumber(String s, int pos) {
        int end = pos;
        while (end < s.length() && s.charAt(end) >= '0' && s.charAt(end) <= '9') {
            end++;
        }
        return end;
    }
}
```