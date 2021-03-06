---
title: LeetCode-26-remove-duplicates-from-sorted-array
toc: true
mathjax: true
description: 本篇文章带来有关 "LeetCode-26-remove-duplicates-from-sorted-array" 我的求解过程
categories:
  - LeetCode
date: 2020-02-17 11:36:38
tags:
- 数组
- 双指针
- 快慢指针
---

# 刷题标记

- [x] 第一遍
- [ ] 第二遍
- [ ] 第三遍
- [ ] 第四遍
- [ ] 第五遍

# 求解过程

>给定一个排序数组，你需要在原地删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。
>
>不要使用额外的数组空间，你必须在原地修改输入数组并在使用 O(1) 额外空间的条件下完成。
>
>示例 1:
>
>给定数组 nums = [1,1,2], 
>
>函数应该返回新的长度 2, 并且原数组 nums 的前两个元素被修改为 1, 2。 
>
>你不需要考虑数组中超出新长度后面的元素。
>示例 2:
>
>给定 nums = [0,0,1,1,1,2,2,3,3,4],
>
>函数应该返回新的长度 5, 并且原数组 nums 的前五个元素被修改为 0, 1, 2, 3, 4。
>
>你不需要考虑数组中超出新长度后面的元素。
>说明:
>
>为什么返回数值是整数，但输出的答案是数组呢?
>
>请注意，输入数组是以“引用”方式传递的，这意味着在函数里修改输入数组对于调用者是可见的。
>
>你可以想象内部操作如下:
>
>// nums 是以“引用”方式传递的。也就是说，不对实参做任何拷贝
>int len = removeDuplicates(nums);
>
>// 在函数里修改输入数组对于调用者是可见的。
>// 根据你的函数返回的长度, 它会打印出数组中该长度范围内的所有元素。
>for (int i = 0; i < len; i++) {
>    print(nums[i]);
>}
>
>来源：力扣（LeetCode）
>链接：https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array
>著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
>
>编写 `public int removeDuplicates(int[] nums)` 函数

## 思路

由于数组是有序的，所以重复的元素在数组中必定是连续出现。可以用双指针遍历比较数组元素，将非重复的元素依次放到数组前半部分即可。

双指针遍历问题中，关键是处理好各个指针移动的条件。对于本题，双指针的移动可由如下类 C 伪码描述。

```c
i = 0;  // 记录数组中非重复元素的最后一个元素的下标
j = 0;  // 用于遍历数组，与下标为 i 的元素比较
if (nums[i] == nums[j]){     // 遇到重复元素
    j++;                     // 那么移动 j 指针，继续寻找非重复元素的下标
} else {                     // 遇到非重复元素
    nums[++i] = nums[j++];   // 将 j 下标的元素复制到 i 的下一个位置
}
```

## Java 实现

根据上述思路，很容易得到以下 Java 实现

```java
public int removeDuplicates(int[] nums) {
    int i = 0;  // 记录数组中非重复元素的最后一个元素的下标
    int j = 0;  // 用于遍历数组，与下标为 i 的元素比较

    // 利用双指针遍历比较数组元素
    while (j < nums.length) {
        if (nums[i] == nums[j]) {       // 遇到重复元素
            j++;                        // 那么移动 j 指针，继续寻找非重复元素的下标
        } else {                        // 遇到非重复元素
            nums[++i] = nums[j++];      // 将 j 下标的元素复制到 i 的下一个位置
        }
    }

    return i + 1;                       // 返回元素个数，因而 i 要加一
}
```

由于只涉及到一个循环，其**时间复杂度**为 $O(n)$ ;

由于只涉及到常数级别的额外空间使用，故**空间复杂度**为 $O(1)$ 

# 高手方案

参见[官方题解](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/solution/shan-chu-pai-xu-shu-zu-zhong-de-zhong-fu-xiang-by-/)，与我的思路类似。

# 总结

* **双指针**技巧，又称作**快慢指针**：通过两个遍历速度不同的指针，达到遍历过程中比较等操作。
* **双指针**的思路关键要处理好**遍历指针移动的条件**。这个问题解决好了，那么问题也就解决了。