---
title: LeetCode-11-container-with-most-water
toc: true
mathjax: true
description: 本篇文章带来有关 "LeetCode-11-container-with-most-water" 我的求解过程
categories:
  - LeetCode
date: 2020-02-13 22:03:05
tags:
- 数组
- 双指针
- 左右夹逼
---

# 刷题标记

- [x] 第一遍
- [ ] 第二遍
- [ ] 第三遍
- [ ] 第四遍
- [ ] 第五遍

# 求解过程

>给定 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0)。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。
>
>说明：你不能倾斜容器，且 n 的值至少为 2。
>
>
>
>图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。
>
> 
>
>示例:
>
>输入: [1,8,6,2,5,4,8,3,7]
>输出: 49
>
>来源：力扣（LeetCode）
>链接：https://leetcode-cn.com/problems/container-with-most-water
>著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
>
>编写 `public int maxArea(int[] height)` 函数

## 思路：暴力解法

很自然的一种暴力思路就是枚举每一种可能，计算题意中的面积；并且，在枚举过程中，不断比较，得到最大值。

## Java 实现

```java
public int maxArea(int[] height) {
    int result = 0;
    // 思路：二重循环
    for (int i = 0; i < height.length - 1; i++) {
        for (int j = i + 1; j < height.length; j++) {
            
            int minIndex = height[i] > height[j] ? j : i;
            int area = height[minIndex] * (j - i);

            if (area > result) {
                result = area;

            }
        }
    }
    return result;
}
```

由代码可知，程序包含二重循环，因而**时间复杂度**为 $O(n^2)$ 

# 高手方案

上述方案比较费时，来看一看高手的解法。

### 思路

* 利用**双指针**，分别初始在**起始**和**末尾**。
* 依题意，计算所围的面积
* 将**高度较短**的指针向中心移动，然后回到第 2 步 

P.S. 为什么是移动高度较短的指针？因为移动较高的指针，只会使得面积变小；而移动较低指针，可能会遇到较高指针（基于此假设，面积可能会变大）。

### Java 实现

```java
public int maxArea(int[] height) {
    // 作者：LeetCode
    // 链接：https://leetcode-cn.com/problems/container-with-most-water/solution/sheng-zui-duo-shui-de-rong-qi-by-leetcode/
    int maxarea = 0, l = 0, r = height.length - 1;
    while (l < r) {
        maxarea = Math.max(maxarea, Math.min(height[l], height[r]) * (r - l));
        if (height[l] < height[r])
            l++;
        else
            r--;
    }
    return maxarea;
}
```

由代码可知，**时间复杂度**为 $O(n)$ ，这是一个极大的优化

# 总结

* 解题小套路
  * 遇到较难的题目
    * 先想**能否暴力解决**，在此之后再优化，或者看别人思路。
    * 暴力无法解决，找找**解的规律**，即从基本情况开始，寻找其是否有**重复子问题**
* **双指针**在数组遍历中是一个常见套路，需要多多注意。

