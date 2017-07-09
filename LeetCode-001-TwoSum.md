---
title: LeetCode-001-TwoSum
date: 2017-01-09 14:19:17
tags: LeetCode
---
###LeetCode-001  Two Sum
1. 题目描述

   给定一个数组nums和一个目标整数target，判断nums中是否含有两个数，他们的数值之和等于target，如果存在则返回这两个数的索引值。

   <!-- more -->

   ![LC-001](https://raw.githubusercontent.com/JackPanda8/ImageHosting/master/LC001-TwoSum.PNG)

[LC-001](https://leetcode.com/problems/two-sum/)
2. 题目分析

   - 因为题目中求的是数值的和，而返回的结果是索引值，所以第一感觉是要建立key-value的hashmap，以空间换取时间，时间复杂度为o(n)。

   - 找两数之和为target，反过来就是找对于nums中的第i个数，是否有另外一个nums中的数其值为target - nums[i]。

     所以要对nums建立HashMap map，遍历一遍看map.containsKey(target - map.get[i])是否为true。

3. 优化

   两个数肯定要一前一后地进入到map中，所以可以在向map中put数据的时候就进行一次判断看map.containsKey(target - map.get[i])是否为true。

4. Java代码

   ```java
   public class Solution {
       public int[] twoSum(int[] nums, int target) {
           int[] result = new int[2];
           Map<Integer, Integer> map = new HashMap<Integer, Integer>();

           for(int i =0; i < nums.length; i++) {
               if(map.containsKey(target - nums[i])) {
                   result[1] = i;
                   result[0] = map.get(target- nums[i]);
                   return result;
               }

               map.put(nums[i], i);
           }

           return result;
       }
   }
   ```

   ​

> stay hungry, stay foolish. --steve jobs.