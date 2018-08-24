---
layout: '[layout]'
title: Longest Palindromic Substring
date: 2017-08-15 15:35:38
tags:
---
最早考虑的是遍历一遍字符串，以每个元素为middle，再向左向右判断元素是否相等，
同时还需要考虑中心元素为1个，还是2个两种情况。最终导致，效率低下，提交时最后一个test没有通过。

输入：“aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa”,这样就会执行n*(1+2+...n/2) = O(n^3)

从网上搜索后，找到几个常见的方法
<!-- more -->

# 1.动态规划
这里动态规划的思路是 dp[i][j] 表示的是从i 到 j 的字串，是否是回文串(Bool类型)

则根据回文的规则我们可以知道：

如果s[i] == s[j] 那么是否是回文决定于 dp[i+1][j-1]

当 s[i] != s[j] 的时候， dp[i][j] 直接就是 false。

动态规划的进行是按照字符串的长度从1 到 n推进的。

```cpp
public class Solution {
    boolean[][] dp;  

    public String longestPalindrome(String s)  
    {  
        if(s.length() == 0)  
        {  
            return "";  
        }  
        if(s.length() == 1)  
        {  
            return s;  
        }  

        dp = new boolean[s.length()][s.length()];  

        int i,j;  

        for( i = 0; i < s.length(); i++)  
        {  
            for( j = 0; j < s.length(); j++)  
            {  
                if(i >= j)  
                {  
                    dp[i][j] = true; //当i == j 的时候，只有一个字符的字符串; 当 i > j 认为是空串，也是回文  

                }  
                else  
                {  
                    dp[i][j] = false; //其他情况都初始化成不是回文  
                }  
            }  
        }  

        int k;  
        int maxLen = 1;  
        int rf = 0, rt = 0;  
        for( k = 1; k < s.length(); k++)  
        {  
            for( i = 0;  k + i < s.length(); i++)  
            {  
                j = i + k;
                if(s.charAt(i) != s.charAt(j)) //对字符串 s[i....j] 如果 s[i] != s[j] 那么不是回文
                {  
                    dp[i][j] = false;  
                }  
                else  //如果s[i] == s[j] 回文性质由 s[i+1][j-1] 决定
                {  
                    dp[i][j] = dp[i+1][j-1];  
                    if(dp[i][j])  
                    {  
                        if(k + 1 > maxLen)  
                        {  
                            maxLen = k + 1;  
                            rf = i;  
                            rt = j;  
                        }  
                    }  
                }  
            }  
        }  
        return s.substring(rf, rt+1);  
    }  
}
```
