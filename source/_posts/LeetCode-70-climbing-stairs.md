---
title: LeetCode-70-climbing-stairs
toc: true
mathjax: true
description: 本篇文章带来有关 "LeetCode-70-climbing-stairs" 我的求解过程
categories:
  - LeetCode
date: 2020-02-11 18:59:00
tags:
- 动态规划
- 斐波那契数列
---

# 刷题标记

- [x] 第一遍
- [ ] 第二遍
- [ ] 第三遍
- [ ] 第四遍
- [ ] 第五遍

# 求解过程

>假设你正在爬楼梯。需要 n 阶你才能到达楼顶。
>
>每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？
>
>注意：给定 n 是一个正整数。
>
>示例 1：
>
>输入： 2
>输出： 2
>解释： 有两种方法可以爬到楼顶。
>1.  1 阶 + 1 阶
>2.  2 阶
>示例 2：
>
>输入： 3
>输出： 3
>解释： 有三种方法可以爬到楼顶。
>1.  1 阶 + 1 阶 + 1 阶
>2.  1 阶 + 2 阶
>3.  2 阶 + 1 阶
>
>来源：力扣（LeetCode）
>链接：https://leetcode-cn.com/problems/climbing-stairs
>著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
>
>编写  `public int climbStairs(int n);` 函数

## 法一——递归（超时！）

### 思路一：原始递归

根据题意，很容易想到一种递归解决方案。

试想这样一个场景，从第 n 阶台阶下楼，每次要么下 1 阶，要么下 2 阶（易知这种场景和上述问题类似，并且结果相同）。那么，在到达第 i 层时，总方法数为
$$
climbStairs(i) = climbStairs( i - 1 ) + climbStairs( i - 1 )
$$
这个就是递归程序递推公式。

那么什么时候，递归程序终止呢？很显然，当 i = 0 或 i = 1 时，方法数只可能为 1。

这也就是递归的终止条件。

由此，可以给出其类 C 伪码描述，具体如下

```C
int climbStairs(int n){
    if ( n == 0 || n == 1 ){
        return 1;
    } else {
        return climbStairs( n - 1 ) + climbStairs( n - 1 );
    }
}
```

### 思路二：用“空间换时间”优化

递归程序由于有很多的冗余计算，会使得程序的执行时间随数据规模呈指数级上升，这是极其恐怖的。一种优化思路，便是利用**空间换时间**的方案优化。具体的，在本例中，即考虑将已经计算过的方案书缓存起来，供后续调用使用。

### 思路二的 Java 实现

```java
public int climbStairs(int n) {
    Map<Integer, Integer> cache 
        = new HashMap<>(); // 利用 HashMap 缓存数据

    if (n == 0 || n == 1) {
        return 1;
    } else {
        int n1, n2;

        if (cache.containsKey(n - 1)) {// 如果中间结果已在缓存
            // 直接从缓存中取得数据
            n1 = Integer.parseInt(cache.get(n - 1).toString());

        } else {
            // 否则，要计算
            n1 = climbStairs(n - 1);
            // 再存入缓存
            cache.put(n - 1, n1);

        }
        if (cache.containsKey(n - 2)) {// 与 n - 1 类似

            n2 = Integer.parseInt(cache.get(n - 2).toString());

        } else {

            n2 = climbStairs(n - 2);

            cache.put(n - 2, n2);

        }

        return n1 + n2;
    }
}
```

这个方案在 leetcode 上测试运行，发现当 n 达到 40以上，开始出现超时。看来光用递归解决不了问题。

看到本题标签有`动态规划`，于是查找资料，重新思考。

看到[这篇博客](https://blog.csdn.net/u013309870/article/details/75193592)，写的不错，并得到下面一种思路。

## 法二——非递归

### 思路

仔细运行法一中的程序，发现其方案数竟然是**斐波那契数列**，于是可以用斐波那契数列的非递归程序解决。也就是[这篇博客](https://blog.csdn.net/u013309870/article/details/75193592)中所提到的**“自底向上”**的思路。

### Java 实现

```java
public int climbStairs(int n) {
    // 法二 非递归（自底向上，相当于上楼）
    // 通过法一，我们可以观察到到达每层楼的方案序列，貌似构成了斐波那契数列
    // 利用斐波那契数列解决

    int n1 = 1;
    int n2 = 2;

    if (n == 1) {

        return n1;

    } else if (n == 2) {

        return n2;

    } else {

        int n3 = 0;

        for (int i = 3; i <= n; i++) {
            n3 = n1 + n2;
            n1 = n2;
            n2 = n3;
        }

        return n3;
    }
}
```

很惊喜的发现，该方案成功 AC，并且执行时间效率非常高。

# 高手方案

## 执行用时为 0 s 的范例

```java
public int climbStairs(int n) {
    // 解法：第n层需要的步数 = （第n-1层 + 1步） + （第n-2层 + 2步）

    if (n == 1) {
        return 1;
    }

    if (n == 2) {
        return 2;
    }

    int a = 1;
    int b = 2;
    int c = 3;

    for (int i = 4; i <= n; i++) {
        a = b;
        b = c;
        c = a + b;
    }

    return c;
}
```

由此可见，我的法二和高手范例几乎类似，这是十分美妙的。

## 官方方案

https://leetcode-cn.com/problems/climbing-stairs/solution/pa-lou-ti-by-leetcode/

最后两种数学方法给跪了

# 总结

* 本题涉及动态规划。能够利用动态规划解决问题，需要问题有以下特性
  * optimal substructure，即原问题的最优解可有子问题的最优解组合而成。
  * overlapping sub-problems，即可将原问题划分为若干规模较小但与原问题类似的子问题解决
* 动态规划的实现，可以有以下两种思路
  * 自顶向下思路（参见本题法一）
  * 自底向上思路（参见本题法二）
* 递归的优化，可以采用**空间换时间**的缓存方案

P.S. 部分总结意译了维基百科对于[动态规划](https://en.wikipedia.org/wiki/Dynamic_programming#Computer_programming)的解释