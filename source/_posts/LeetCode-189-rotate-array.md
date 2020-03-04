---
title: LeetCode-189-rotate-array
toc: true
mathjax: true
description: 本篇文章带来有关 "LeetCode-189-rotate-array" 我的求解过程
categories:
  - LeetCode
date: 2020-02-17 14:01:58
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

>给定一个数组，将数组中的元素向右移动 k 个位置，其中 k 是非负数。
>
>示例 1:
>
>输入: [1,2,3,4,5,6,7] 和 k = 3
>输出: [5,6,7,1,2,3,4]
>解释:
>向右旋转 1 步: [7,1,2,3,4,5,6]
>向右旋转 2 步: [6,7,1,2,3,4,5]
>向右旋转 3 步: [5,6,7,1,2,3,4]
>示例 2:
>
>输入: [-1,-100,3,99] 和 k = 2
>输出: [3,99,-1,-100]
>解释: 
>向右旋转 1 步: [99,-1,-100,3]
>向右旋转 2 步: [3,99,-1,-100]
>说明:
>
>尽可能想出更多的解决方案，至少有三种不同的方法可以解决这个问题。
>要求使用空间复杂度为 O(1) 的 原地 算法。
>
>来源：力扣（LeetCode）
>链接：https://leetcode-cn.com/problems/rotate-array
>著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
>
>编写 `public void rotate(int[] nums, int k)` 

## 思路：暴力法

* 两个循环
  * 内部循环只向右移动一格，用一个变量暂存溢出元素
  * 外部循环 **k** 次，相当于向右移动 k 个位置

## Java 实现

```java
public void rotate(int[] nums, int k) {
    // 暴力法
    for (int i = 0; i < k; i++) {          //多次移动
        int tail = nums[nums.length - 1];  // 用一个变量暂存移位溢出的数字

        for (int j = nums.length - 1; j > 0; j--) {
            nums[j] = nums[j - 1];         // 内部循环只移动一格
        }

        nums[0] = tail;                    // 将溢出的数组，放到数组首位
    }
}
```

* **时间复杂度**为 $O(n*k)$
* **空间复杂度**为 $O(1)$

# 高手方案

## 思路：使用反转

这里记录一下 **使用反转** 这个思路

>这个方法基于这个事实：当我们旋转数组 k 次，$k\%n$ 个尾部元素会被移动到头部，剩下的元素会被向后移动。
>
>在这个方法中，我们首先将所有元素反转。然后反转前 k 个元素，再反转后面 $n−k$ 个元素，就能得到想要的结果。

## Java 实现

```java
public class Solution {
    public void rotate(int[] nums, int k) {
        k %= nums.length;
        reverse(nums, 0, nums.length - 1);
        reverse(nums, 0, k - 1);
        reverse(nums, k, nums.length - 1);
    }
    public void reverse(int[] nums, int start, int end) {
        while (start < end) {
            int temp = nums[start];
            nums[start] = nums[end];
            nums[end] = temp;
            start++;
            end--;
        }
    }
}
```

* **时间复杂度**为 $O(n)$
* **空间复杂度**为 $O(1)$

更多参见[官方题解](https://leetcode-cn.com/problems/rotate-array/solution/xuan-zhuan-shu-zu-by-leetcode/)

# 总结

* 循环移动数组元素可以利用**反转**高效解决，即
  * 反转数组所有元素
  * 反转前 k 个 元素
  * 反转后 n - k 个元素