---
title: LeetCode-002-Add-Two-Numbers
date: 2017-01-11 22:41:58
tags: LeetCode
---

### LeetCode-002 Add Two Numbers

1. 题目描述
   给定两个非负整数，将其按照个位->十位->百位..的逆序形式作为输入，要求输出两个数之和的逆序形式。例如输入为 3->5->7和4->6->5，输出为7->1->3->1。

   <!-- more -->![LC-002](https://raw.githubusercontent.com/JackPanda8/ImageHosting/master/LC002-AddTwoNumbers.PNG)
   [LC-002](https://leetcode.com/problems/add-two-numbers/)

2. 题目分析
   - 第一反应是转成两个整数在让其相加然后再逆序转回来，但是很明显这样的时间复杂度太高。既然给出了逆序的数，可以想办法直接对其进行加法运算。加法无非就是相应位置上的数字相加然后再加上前一位传过来的进位进行加一操作。
   - 核心技巧：**每一位都在重复一个操作——相同位置数字之和对10取余+前一个位置数字之和除以10的结果（0或者1）.**

3. Java源码

```java
import java.util.*;
import java.lang.StringBuilder;

public class Main {
    public static ListNode addTwoNumbers(ListNode list1, ListNode list2) {
        ListNode result = new ListNode(0);
        ListNode start = result;
        int sum = 0;
        while(list1 != null || list2 != null) {
            sum = sum/10;//精髓在这里。整数位与位之间的加法与进位：利用整数的除法，
            //不足十的数除以十得到0，下一位进位为0，多于十的数除以十也是得到下一位的进位
            if(list1 != null) {
                sum += list1.val;
                list1 = list1.next;
            }
            if(list2 != null) {
                sum += list2.val;
                list2 = list2.next;
            }
            result.next = new ListNode(sum % 10);
            result = result.next;
        }
        if(sum/10 == 1) {
            result.next = new ListNode(1);
        }
        return start.next;
    }

    public static void main(String[] args) {
        String input = new  String("(2 -> 4 -> 3) + (5 -> 6 -> 4)");
        String output = "";
        String firstList, secondList;
        String[] splitResult = input.split("\\+");
        firstList = splitResult[0];
        secondList = splitResult[1];
        firstList = firstList.replaceAll("\\(|\\)|\\-|\\>| ", "");
        secondList = secondList.replaceAll("\\(|\\)|\\-|\\>| ", "");

        ListNode list1 = new ListNode(0);
        ListNode list2 = new ListNode(0);
        ListNode node1 = list1;
        ListNode node2 = list2;
        for(int i = 0; i < firstList.length(); i++) {
            list1.val = firstList.toCharArray()[i] - '0';
            list1.next = (i != firstList.length() - 1)? (new ListNode(0)):null;
            list1 = list1.next;
        }
        list1 = node1;
        for(int i = 0; i < secondList.length(); i++) {
            list2.val = secondList.toCharArray()[i] - '0';
            list2.next = (i != secondList.length() - 1)? (new ListNode(0)):null;
            list2 = list2.next;
        }
        list2 = node2;

        ListNode result = Main.addTwoNumbers(list1, list2);
//        System.out.println(result.val);

        while(result != null) {
            if(result.next != null) {
                output = output + (new Integer(result.val)).toString() + " -> ";
            } else {
                output = output + (new Integer(result.val)).toString();
            }
            result = result.next;
        }
        System.out.println(output);
    }
}

class ListNode {
    int val;
    ListNode next;

    public ListNode(int val) {
        this.val = val;
    }
}
```

> stay hungry, stay foolish. --steve jobs.

