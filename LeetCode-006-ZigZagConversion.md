---
title: LeetCode-006-ZigZagConversion
date: 2017-01-13 09:58:33
tags: LeetCode
---

### leetCode-006-ZigZagConversion

1. 题目描述

   将给定的字符串以“之”字的形状排列，然后一行一行地读取得到新的字符串输出。例如"PAYPALISHIRING"的之字形排列如下：
   <!-- more -->

   |   P   |       |   A   |       |   H   |       |   N   |
   | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
   | **A** | **P** | **L** | **S** | **I** | **I** | **G** |
   | **Y** |       | **I** |       | **R** |       |       |
   输出为"PAHNAPLSIIGYIR" 。现要求给出nRows，表示之字形排列时的行数（上面的nRows=3），输出转换后的字符串。

   > ![LC-006](https://github.com/JackPanda8/ImageHosting/blob/master/LC006-ZigZagConversion.png?raw=true)
   >  [LC-006](https://leetcode.com/problems/zigzag-conversion/)

   ​

2. 题目分析

   * 之字形排列过程是由许多的下降（第一列PAY）、斜上升（YPA）组成的，建立一个有nRows个元素的StringBuilder，在循环遍历的过程中不断地在相应位置进行字符串拼接。

3. Java源码

```java
public class Main {

    public  static String convert(String s, int nRows) {
        char[] c = s.toCharArray();
        int len = c.length;
        StringBuilder[] sb = new StringBuilder[nRows];
        for (int i = 0; i < sb.length; i++) sb[i] = new StringBuilder();

        int i = 0;
        while (i < len) {
            for(int j = 0; j  < nRows && i < len; j++) {
                sb[j].append(c[i]);
                i++;
            }

            for(int j = nRows - 2; j >= 1 && i < len
                    ; j --) {
                sb[j].append(c[i]);
                i++;
            }
        }
        for (int idx = 1; idx < sb.length; idx++)
            sb[0].append(sb[idx]);
        return sb[0].toString();
    }

    public static void main(String[] args) {
        System.out.println(Main.convert("PAYPALISHIRING", 4));
    }
}

```

> stay hungry, stay foolish. --steve jobs.