title: LeetCode.166 Fraction to Recurring Decimal
author: Matteo
date: 2019-05-27 12:28:27
tags:
---
[https://leetcode.com/problems/fraction-to-recurring-decimal/](https://leetcode.com/problems/fraction-to-recurring-decimal/)
##### 题意
* 除法，用（）来表示重复部分
##### 解法
1. 计算整数部分，计下商和余数
2. 假定余数不为0，则余数*10，将余数和位置加入map中用于判断余数是否重复，并将商加入小数部分
3. 重复2，直至余数重复
4. 在分数部分中第一个重复位置插入（
```java
class Solution {
    //考虑到-2147483648的情况，不能直接abs
    public String fractionToDecimal(int numerator, int denominator) {
        StringBuilder sb = new StringBuilder();
        //先插入正负
        if ((numerator < 0 && denominator > 0) || (numerator > 0 && denominator < 0)) sb.append("-");
        //计算商
        long quotient = (long) numerator / denominator;
        //插入商
        sb.append(Math.abs(quotient));
        //余数
        long remainder = (long) numerator % denominator * 10;
        if (remainder != 0) {
            //余数不为零，插入小数点
            sb.append(".");
            //分数部分
            StringBuilder fraction = new StringBuilder();
            Map<Long, Integer> nums = new HashMap<>();
            // nums.add(numerator);
            int pos = 0;
            //以余数为key，插入出现位置
            nums.put(remainder, pos++);
            do {
                //计算商
                int q = (int) Math.abs(remainder / denominator);
                //插入商
                fraction.append(q);
                //生成新余数
                remainder = remainder % denominator * 10;
                //判断是否出现过
                if (nums.containsKey(remainder)) {
                    break;
                }
                //未出现则继续计算
                nums.put(remainder, pos++);
            } while (remainder != 0);
            if (remainder != 0) {
                //查找第一次出现相同余数的位置，插入（
                fraction.insert(nums.get(remainder), "(");
                fraction.append(")");
                sb.append(fraction.toString());
            } else {
                sb.append(fraction.toString());
            }
        }
        return sb.toString();
    }
}
```