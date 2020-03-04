---
title: LeetCode-66-plus-one
toc: true
mathjax: true
description: 本篇文章带来有关 "LeetCode-66-plus-one" 我的求解过程
categories:
  - LeetCode
date: 2020-02-17 18:42:55
tags:
- 数组
---

# 刷题标记

- [x] 第一遍
- [ ] 第二遍
- [ ] 第三遍
- [ ] 第四遍
- [ ] 第五遍

# 求解过程

>给定一个由整数组成的非空数组所表示的非负整数，在该数的基础上加一。
>
>最高位数字存放在数组的首位， 数组中每个元素只存储单个数字。
>
>你可以假设除了整数 0 之外，这个整数不会以零开头。
>
>示例 1:
>
>输入: [1,2,3]
>输出: [1,2,4]
>解释: 输入数组表示数字 123。
>示例 2:
>
>输入: [4,3,2,1]
>输出: [4,3,2,2]
>解释: 输入数组表示数字 4321。
>
>来源：力扣（LeetCode）
>链接：https://leetcode-cn.com/problems/plus-one
>著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
>
>编写 `public int[] plusOne(int[] digits)` 函数

## 思路

这道题的思路可以简单描述为，模拟竖式加法运算。

## Java 实现

```java
public int[] plusOne(int[] digits) {

    // 思路：模拟竖式计算

    // 末位加一
    digits[digits.length - 1] = digits[digits.length - 1] + 1;

    // 处理进位
    for (int i = digits.length - 1; i > 0; i--) {
        if (digits[i] < 10) {
            break;
        } else {
            digits[i] = digits[i] % 10;
            digits[i - 1] = digits[i - 1] + 1;
        }
    }

    // 处理原有位数不够
    if (digits[0] >= 10) {

        int[] result = new int[digits.length + 1];
        result[0] = 1;
        result[1] = 0;

        for (int i = 1; i < digits.length; i++) {
            result[i+1] = digits[i];
        }

        return result;

    } else {

        return digits;

    }
}
```



* **时间复杂度**为 $O(n)$
* **空间复杂度**
  * 最好 $O(1)$ 
  * 最坏 $O(n)$ （最高位进位情况）