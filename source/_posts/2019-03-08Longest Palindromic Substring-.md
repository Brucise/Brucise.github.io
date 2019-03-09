---
title: Longest Palindromic Substring
date: 2019-03-08 18:07:12
tags: [leetcode,字符串]
---
Given a string s, find the longest palindromic substring in s. You may assume that the maximum length of s is 1000.
给定一个字符串s，找出其中的最大回文子串，假定字符串s最长为1000.

# 前言
这是一道leetcode上的字符串题目，下图是我提交的几种解法的耗时与内存，在通过的几种里，8ms和111ms是网上的解法，186ms和576ms是我自己的解法。我的两种解法的思路都一样，将对称中心设为单个字符（“a”）或相等的双字符（“aa”），然后分别向两边扩展，最开始耗时太久是因为每次我都是new了一个字符数组，后来改成直接在字符串上进行charat操作。这样直接将时间降为186ms。通过观察发现，8毫秒的解法与我的解法，思路一致，但是代码更为简洁。全程都是操作下标int类型，而我一开始就使用了substring操作。经过改正，我也使用下标int进行操作，时间降为10毫秒。
注意：
1. 字符串尽量用下标进行操作，尽量少调用substring。
2. 少去定义新的游标，尽量跟遍历时的i联系起来。
![解法耗时](/images/longgest.png)
# 10ms

``` java
package longestpalindrome;

/**
 * @author: chenhuang
 * @date: 2019/3/9
 * @description:
 */
public class SolutionV3 {
    public static void main(String[] args) {
        SolutionV3 solution = new SolutionV3();
        System.out.println(solution.longestPalindrome("vv"));
    }

    public String longestPalindrome(String s) {
        if (s == null || s.length() == 0) {
            return "";
        }
        int start, end;
        start = 0;
        end = 0;
        for (int i = 0; i < s.length(); i++) {
            int len1 = longHelp(s, i, i);
            if (len1 > end - start) {
                start = i - (len1 - 1) / 2;
                end = i + len1 / 2;
            }
            if (s.length() >= 2 && findTwins(s, i)) {
                int len2 = longHelp(s, i, i + 1);
                if (len2 > end - start) {
                    start = i - (len2 - 1) / 2;
                    end = i + len2 / 2;
                }
            }
        }
        return s.substring(start, end + 1);
    }

    private int longHelp(String s, int x, int y) {
        while (x >= 0 && y < s.length() && s.charAt(x) == s.charAt(y)) {
            x--;
            y++;
        }
        return y - x - 1;
    }

    private boolean findTwins(String s, int index) {
        for (int i = index; i < s.length() - 1; i++) {
            if (s.charAt(i) == s.charAt(i + 1)) {
                return true;
            }
        }
        return false;
    }
}
```
# 576ms

``` java
package longestpalindrome;

public class Solution {
    public static void main(String[] args) {
        Solution solution = new Solution();
        //576ms解法
        System.out.println(solution.longestPalindrome("cbbd"));
    }
    public String longestPalindrome(String s) {
        if (s == null || s.length() == 0) {
            return "";
        }
        char[] arr = s.toCharArray();
        char[] arrInterval = new char[s.length() * 2 + 1];
        for (int i = 0; i < arrInterval.length; i++) {
            arrInterval[i] = ' ';
        }
        for (int i = 0; i < s.length(); i++) {
            arrInterval[2 * i + 1] = arr[i];
        }
        return longestPalindromeHelp(arrInterval);
    }

    private String longestPalindromeHelp(char[] arr) {
        String longgest = String.valueOf(arr[1]);
        for (int i = 2; i < arr.length; i++) {
            char[] result = new char[arr.length];
            int p, q;
            p = i - (1 + i % 2);
            q = i + (1 + i % 2);
            while (p > 0 && q < arr.length) {
                if (arr[p] != arr[q]) {
                    break;
                }
                result[i] = arr[i];
                result[p] = result[q] = arr[p];
                p = p - 2;
                q = q + 2;
            }
            String current = String.valueOf(result).trim().replaceAll("[^0-9a-zA-Z]", "");
            if (longgest.length() <= current.length()) {
                longgest = current;
            }
        }
        return longgest;
    }
}
```
# 186ms
``` java
 package longestpalindrome;


public class Solution {
    public static void main(String[] args) {
        Solution solution = new Solution();
        System.out.println(solution.longestPalindrome("aaaa"));
    }

    public String longestPalindrome(String s) {
        if (s == null || s.length() == 0) {
            return "";
        }
        int p, q;
        StringBuilder stringBuilder = new StringBuilder(s);
        CharSequence longsub = new StringBuilder(s.substring(0, 1));
        for (int i = 0; i < stringBuilder.length(); i++) {
            p = q = i;
            CharSequence s1 = longHelp(stringBuilder, p, q);
            if (s1.length() > longsub.length()) {
                longsub = s1;
            }
            if (s.length() >= 2 && findTwins(s, i)) {
                p = i;
                q = i + 1;
                CharSequence s2 = longHelp(stringBuilder, p, q);
                if (s2.length() > longsub.length()) {
                    longsub = s2;
                }
            }
        }
        return longsub.toString();
    }

    private CharSequence longHelp(StringBuilder s, int x, int y) {
        int maxsub, left, right;
        maxsub = 1;
        CharSequence charSequence = s.subSequence(0, 1);
        while (x >= 0 && y < s.length() && s.charAt(x) == s.charAt(y)) {
            int cur = y - x + 1;
            left = x;
            right = y;
            x--;
            y++;
            if (cur > maxsub) {
                maxsub = cur;
                charSequence = s.subSequence(left, right + 1);
            }
        }
        return charSequence;
    }

    private boolean findTwins(String s, int index) {
        for (int i = index; i < s.length() - 1; i++) {
            if (s.charAt(i) == s.charAt(i + 1)) {
                return true;
            }
        }
        return false;
    }
}
```
# 8ms
``` java
class Solution {
  public String longestPalindrome(String s) {
    if (s == null || s.length() < 1) return "";
    int start = 0, end = 0;
    for (int i = 0; i < s.length(); i++) {
        int len1 = expandAroundCenter(s, i, i);
        int len2 = expandAroundCenter(s, i, i + 1);
        int len = Math.max(len1, len2);
        if (len > end - start) {
            start = i - (len - 1) / 2;
            end = i + len / 2;
        }
    }
    return s.substring(start, end + 1);
}

private int expandAroundCenter(String s, int left, int right) {
    int L = left, R = right;
    while (L >= 0 && R < s.length() && s.charAt(L) == s.charAt(R)) {
        L--;
        R++;
    }
    return R - L - 1;
}
}
```
# 111ms
``` java
class Solution {
  
       public String longestPalindrome(String s) {
      String res = "";
        int start, end;
        for(int i = 0; i<s.length(); i++){
            for(int j = 0; j<=i; j++){
                start = j; end = i;
                if((i-j+1)>res.length()){
                   while(s.charAt(start)==s.charAt(end)){
                        if(start == end || (end-start)==1){
                            res = s.substring(j, i+1);
                            break;
                        }
                        start ++; end --;
                    }
                }
            }  
        }
        return res;
    }
}
```