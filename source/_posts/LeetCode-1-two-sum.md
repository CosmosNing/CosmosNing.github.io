---
title: LeetCode-1-two-sum
toc: true
mathjax: true
description: 本篇文章带来有关 "LeetCode-1-two-sum" 我的求解过程
categories:
  - LeetCode
date: 2020-02-17 16:24:28
tags:
- 数组
- 哈希表
- 排序
---

# 刷题标记

- [x] 第一遍
- [ ] 第二遍
- [ ] 第三遍
- [ ] 第四遍
- [ ] 第五遍

# 求解过程

>给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。
>
>你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。
>
>示例:
>
>给定 nums = [2, 7, 11, 15], target = 9
>
>因为 nums[0] + nums[1] = 2 + 7 = 9
>所以返回 [0, 1]
>
>来源：力扣（LeetCode）
>链接：https://leetcode-cn.com/problems/two-sum
>著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
>
>编写 `public int[] twoSum(int[] nums, int target)` 函数

## 思路：暴力法

很容易想到一种暴力解法，即，

* 枚举每一种可能，按条件返回结果

## Java 实现

```java
public int[] twoSum(int[] nums, int target) {

    // 暴力解法：枚举每一种可能
    int[] result = new int[2];

    for (int i = 0; i < nums.length - 1; i++) {
        for (int j = i + 1; j < nums.length; j++) {
            if(nums[i] + nums[j] == target){
                result[0] = i;
                result[1] = j;

                return result;
            }
        }
    }


    return result;
}
```



* **时间复杂度**为 $O(n^2)$
* **空间复杂度**为 $O(1)$ 

# 高手方案

上述暴力法的时间效率实在是不堪入目。一种比较常规的优化思路便是**空间换时间**，来看一看[高手的解法](https://leetcode-cn.com/problems/two-sum/solution/liang-shu-zhi-he-by-leetcode-2/)。

## 思路：一遍哈希表

上述暴力法时间的低效主要是由于，程序只记住了两个待检验的数组下标，而重复遍历了数组很多次。其实，没必要重复遍历：**遍历的时候，存储起来**，就会使得时间效率提高。

对于本题，可以用**哈希表**，存储**数**和**下标**之间的关系。这样在通常情况下，只需 $O(1)$ 的时间，便可访问到哈希表中已存在的元素。从而使得整个程序的时间复杂度降到 $O(n)$ 。

## Java 实现

```java
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        if (map.containsKey(complement)) {               // 如果存在满足条件的数
            return new int[] { map.get(complement), i }; // 直接返回结果
        }
        map.put(nums[i], i);                             // 否则，存储到哈希表中
    }
    throw new IllegalArgumentException("No two sum solution");
}
```



* **时间复杂度**为 $O(n)$ 
* **空间复杂度**为 $O(n)$ 



# 类似题目：15.3sum

>给定一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？找出所有满足条件且不重复的三元组。
>
>注意：答案中不可以包含重复的三元组。
>
> 
>
>示例：
>
>给定数组 nums = [-1, 0, 1, 2, -1, -4]，
>
>满足要求的三元组集合为：
>[
>  [-1, 0, 1],
>  [-1, -1, 2]
>]
>
>来源：力扣（LeetCode）
>链接：https://leetcode-cn.com/problems/3sum
>著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 刷题标记

- [x] 第一遍
- [ ] 第二遍
- [ ] 第三遍
- [ ] 第四遍
- [ ] 第五遍

## 高手方案

这道题，我没什么思路，直接参考了[高手题解](https://leetcode-cn.com/problems/3sum/solution/hua-jie-suan-fa-15-san-shu-zhi-he-by-guanpengchn/)

```java
public List<List<Integer>> threeSum(int[] nums) {
    List<List<Integer>> ans = new ArrayList();
    int len = nums.length;
    if(nums == null || len < 3) return ans;
    Arrays.sort(nums); // 排序
    for (int i = 0; i < len ; i++) {
        if(nums[i] > 0) break; // 如果当前数字大于0，则三数之和一定大于0，所以结束循环
        if(i > 0 && nums[i] == nums[i-1]) continue; // 去重
        int L = i+1;
        int R = len-1;
        while(L < R){
            int sum = nums[i] + nums[L] + nums[R];
            if(sum == 0){
                ans.add(Arrays.asList(nums[i],nums[L],nums[R]));
                while (L<R && nums[L] == nums[L+1]) L++; // 去重
                while (L<R && nums[R] == nums[R-1]) R--; // 去重
                L++;
                R--;
            }
            else if (sum < 0) L++;
            else if (sum > 0) R--;
        }
    }        
    return ans;
}
```

# 总结

* **空间换时间**是一种常见的优化思路
* 另外，根据题意，**舍弃遍历过程中绝不可能的组合**，也是一种优化手段
* 已经**有序**的元素有着非常良好的性质，可以**先排序**，**再解题**。