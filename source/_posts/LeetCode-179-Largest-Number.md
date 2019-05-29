title: LeetCode.179 Largest Number
author: Matteo
date: 2019-05-28 18:27:53
tags:
---
[https://leetcode.com/problems/largest-number/](https://leetcode.com/problems/largest-number/)

##### 题意
* 求int数组拼接的最大值
##### 解法
* 先转成String，然后排序。排序里先按位比较，当2个数字开头相等但长度不等，则比较这两个数字拼接结果
```java
class Solution {
    public String largestNumber(int[] nums) {
        List<String> ns = Arrays.stream(nums).mapToObj(new IntFunction<String>() {
            @Override
            public String apply(int value) {
                return String.valueOf(value);
            }
        }).collect(Collectors.toList());
        Collections.sort(ns, new Comparator<String>() {
            @Override
            public int compare(String o1, String o2) {
                return compare1(o1, o2);
            }
        });
        StringBuilder builder = new StringBuilder();
        for (String n: ns) {
            builder.append(n);
        }
        return builder.toString().replaceAll("^0+", "0");
    }

    int compare1(String s1, String s2) {
        int i = 0;
        while (i < s1.length() && i < s2.length()) {
            if (s1.charAt(i) > s2.charAt(i)) {
                return -1;
            } else if (s1.charAt(i) < s2.charAt(i)) {
                return 1;
            } else {
                i++;
            }
        }
//        这里可能会产生824，8247这样的情况，需要取拼接后大的那个
//        if (s1.length() > s2.length()) {
//            return -s1.charAt(i) + s2.charAt(i-1);
//        } else if (s1.length() < s2.length()) {
//            return s2.charAt(i) - s1.charAt(i-1);
//        } else {
//            return 0;
//        }
        return (s2 + s1).compareTo(s1 + s2);
    }
}
* 经过尝试发现按位比较可以去掉，直接(s2 + s1).compareTo(s1 + s2)即可
```
##### 更好的解法
* 先记下，太复杂了
```java
class Solution {
    public String largestNumber(int[] nums) {
        if (nums.length <= 0) return "";
        
        int min = 0;
        int max = nums.length - 1;
        int[] aux = new int[max + 1];
        
        mergesort(nums, aux, min, max);
        
        StringBuilder sb = new StringBuilder();
        
        for (int i = 0; i <= max; i++) {
            if (sb.length() <= 0 && nums[i] == 0 && i < max) continue;
            sb.append(String.valueOf(nums[i]));
        }
        return sb.toString();
    }
    
    public void mergesort(int[] a, int[] aux, int min, int max) {
        if (min >= max) return;
        
        int mid = min + (max - min) / 2;
        mergesort(a, aux, min, mid);
        mergesort(a, aux, mid + 1, max);
        
        merge(a, aux, min, mid, max);
    }
    
    public void merge(int[] a, int[] aux, int min, int mid, int max) {
        for (int x = min; x <= max; ++x) {
            aux[x] = a[x];
        }
        
        int i = min;
        int j = mid + 1;
        
        for (int k = min; k <= max; k++) {
            if (i > mid) {
                a[k] = aux[j++];
            } else if (j > max) {
                a[k] = aux[i++];
            } else if (more(aux, j, i)) {
                a[k] = aux[j++];
            } else {
                a[k] = aux[i++];
            }
        }
    }
    
    // a[i] is "more" than a[j]
    public boolean more(int[] a, int i, int j) {
        int vi = a[i];
        int vj = a[j];
        int[] iarr = new int[(String.valueOf(vi)).length()];
        int[] jarr = new int[(String.valueOf(vj)).length()];
        if (jarr.length == iarr.length) {
            return vi > vj;
        }
        
        int idx = iarr.length - 1;
        while (vi > 0) {
            iarr[idx] = vi % 10;
            vi /= 10;
            --idx;
        }
        idx = jarr.length - 1;
        while (vj > 0) {
            jarr[idx] = vj % 10;
            vj /= 10;
            --idx;
        }
        
        idx = 0;
        
        if (iarr.length > jarr.length) {
            int istart = 0;
            int i_idx = 0;
            int j_idx = 0;
            while (istart + i_idx < iarr.length) {
                if (iarr[istart + i_idx] > jarr[j_idx]) {
                    return true;
                } else if (iarr[istart + i_idx] < jarr[j_idx]) {
                    return false;
                } else {
                    if (j_idx == jarr.length - 1) {
                        j_idx = 0;
                        istart += jarr.length;
                        i_idx = 0;
                    } else {
                        j_idx++;
                        i_idx++;
                    }
                }
            }
            return (jarr[0] > jarr[j_idx]);
        } else {
            int jstart = 0;
            int i_idx = 0;
            int j_idx = 0;
            while (jstart + j_idx < jarr.length) {
                if (jarr[jstart + j_idx] > iarr[i_idx]) {
                    return false;
                } else if (jarr[jstart + j_idx] < iarr[i_idx]) {
                    return true;
                } else {
                    if (i_idx == iarr.length - 1) {
                        i_idx = 0;
                        jstart += iarr.length;
                        j_idx = 0;
                    } else {
                        j_idx++;
                        i_idx++;
                    }
                }
            }
            return (iarr[0] < iarr[i_idx]);
        }
    }
}
```