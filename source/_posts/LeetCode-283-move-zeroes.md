---
title: LeetCode.283.move-zeroes
toc: true
mathjax: true
categories:
  - LeetCode
date: 2020-02-09 09:47:36
description: 本篇文章带来有关 LeetCode.283.move-zeroes 我的求解过程
tags:
- 数组
- 双指针
---

# 刷题标记

- [x] 第一遍
- [ ] 第二遍
- [ ] 第三遍
- [ ] 第四遍
- [ ] 第五遍

# 求解过程

>给定一个数组 nums，编写一个函数将所有 0 移动到数组的末尾，同时保持非零元素的相对顺序。
>
>示例:
>
>输入: [0,1,0,3,12]
>输出: [1,3,12,0,0]
>说明:
>
>必须在原数组上操作，不能拷贝额外的数组。
>尽量减少操作次数。
>
>来源：力扣（LeetCode）
>链接：https://leetcode-cn.com/problems/move-zeroes
>著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
>
>编写函数：`public void moveZeroes(int[] nums)`

## 法一——遍历和移位同时进行（Fail）

### 思路

* 遍历 `nums`
  * 若是 0 ，所有向前移
  * 若非 0 ，下一个

### 代码实现

遍历需要一个循环，移位需要一个循环，将两个循环嵌套即可。详见下面的代码

```java
public void moveZeroes(int[] nums) {
    // 法一(fail)
    // 遍历 nums
    // 若是 0 ，所有向前移
    // 若非 0 ，下一个
    int count = 0; // 0 的个数
    for (int i = 0; i < nums.length - count; i++) {
        if (nums[i] == 0) {
            count++;
            for (int j = i; j < nums.length - count; j++) {
                nums[j] = nums[j + 1];
            }
            nums[nums.length - count] = 0;
        }
    }
}
```

### 时空复杂度分析

* 时间上，两个循环嵌套，每层至多循环 n 次，故时间复杂度为 $O(n)$
* 空间上，只定义了额外的几个辅助变量（ `count` 、`i`、`j` 等 ），故空间复杂度为 $O(1)$

### 存在的问题

* 所给解法虽然能过题中所给样例，但是遇到**连续 0**的情况，就会给出错误输出（如输入 [0, 0, 1]，本程序输出 [0, 1, 0]；实际上，应输出 [1, 0, 0]）。
* 回顾此解法，发现问题所在：内层循环在移位时，并没有考虑**连续 0**
* 可能的解决方案：在内层循环中再嵌套一层循环，用于找其后非零元素
* 预估其时间复杂度较高（$O(n^3)$），此法废弃

## 法二——自顶向下的思路（AC）

### 自顶向的下编程理念

* 将解决问题的过程，分成几个较大的步骤。从宏观层面上，验证思路的正确性

* 将上述每一步骤，各自抽象成一个函数，确定函数的形参列表和返回值，并按解题步骤在主控程序中排列好
* 转向细节层面，考虑每一个小步骤的功能如何实现

### 思路

* 第一遍遍历，得到 0 的个数
* 第二遍遍历，删掉所有 0 元素（伪删除，仅将非零元素前移）
* 在末尾补全 0

### 代码实现

#### 顶层实现

为得到 0 的个数，定义 `int countZeros(int[] nums)` 解决

为删掉所有 0 元素，定义 `void removeZeros(int[] nums)` 解决

为在末尾补全 0，定义 `void completeZeros(int[] nums, int count)` 解决

假设函数功能均按照预期运行，则在主控函数 `moveZeroes` 中可如下调用

```java
public void moveZeroes(int[] nums) {
    // 法二
    // 第一遍遍历，得到 0 的个数
    int count_0 = countZeros(nums);
    // 第二遍遍历，删掉所有 0 元素
    removeZeros(nums);
    // 在末尾补全 0
    completeZeros(nums, count_0);
}
```

#### 底层实现

##### int countZeros(int[] nums)

```java
private int countZeros(int[] nums) {
    int count = 0;

    for (int i = 0; i < nums.length; i++) {
        if (nums[i] == 0) {
            count++;
        }
    }

    return count;
}
```

##### void removeZeros(int[] nums)

```java
private void removeZeros(int[] nums) {
    int j = 0;
    
    for (int i = 0; i < nums.length; i++) {
        if (nums[i] == 0) {
            continue;
        } else {
            nums[j++] = nums[i];
        }
    }
}
```

##### void completeZeros(int[] nums, int count)

```java
private void completeZeros(int[] nums, int count) {
    for (int i = nums.length - 1; i >= nums.length - count ; i--) {
        nums[i] = 0;
    }
}
```

#### 完整代码

```java
public void moveZeroes(int[] nums) {
    // 法二
    // 第一遍遍历，得到 0 的个数
    int count_0 = countZeros(nums);
    // 第二遍遍历，删掉所有 0 元素
    removeZeros(nums);
    // 在末尾补全 0
    completeZeros(nums, count_0);
}

private void completeZeros(int[] nums, int count) {
    for (int i = nums.length - 1; i >= nums.length - count ; i--) {
        nums[i] = 0;
    }
}

private void removeZeros(int[] nums) {
    int j = 0;

    for (int i = 0; i < nums.length; i++) {
        if (nums[i] == 0) {
            continue;
        } else {
            nums[j++] = nums[i];
        }
    }
}

private int countZeros(int[] nums) {
    int count = 0;

    for (int i = 0; i < nums.length; i++) {
        if (nums[i] == 0) {
            count++;
        }
    }

    return count;
}
```

### 时空复杂度分析

* 时间上，调用了 3 个 串行的 $O(n)$ ，故时间复杂度为 $O(n)$
* 空间上，使用了常数个辅助变量，空间复杂度为 $O(1)$

# 高手方案

## 思路：双指针

* 遍历数组，不为 0 的元素移动数组前方，用 index下标记录。
* 遍历结束，对index值后的元素统一设为0

## Java 实现

```java
// 来自执行用时 0 ms 范例
public void moveZeroes(int[] nums) {
    int count = 0;
    int index = 0;
    for(int i=0; i<nums.length; i++){
        if(nums[i]==0)
            count++;
        else{
            nums[index] = nums[i];
            index++;
        }
    }
    for(int i=nums.length-count; i<nums.length; i++){
        nums[i] = 0;
    }
}
```



* **时间复杂度**为 $O(n)$
* **空间复杂度**为 $O(1)$



另一种容易理解的解法，具体参见[这个题解](https://leetcode-cn.com/problems/move-zeroes/solution/0-ms-zai-suo-you-java-ti-jiao-zhong-ji-bai-liao--2/)

# 总结

* **自顶向下**的编程理念
* 双指针优化思路