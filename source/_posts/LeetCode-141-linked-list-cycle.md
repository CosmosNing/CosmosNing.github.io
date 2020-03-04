---
title: LeetCode-141-linked-list-cycle
toc: true
mathjax: true
description: 本篇文章带来有关 "LeetCode-141-linked-list-cycle" 我的求解过程
categories:
  - LeetCode
date: 2020-02-12 22:49:58
tags:
- 双指针
- 遍历
- 链表
- 快慢指针
---

# 刷题标记

- [x] 第一遍
- [ ] 第二遍
- [ ] 第三遍
- [ ] 第四遍
- [ ] 第五遍

# 求解过程

>给定一个链表，判断链表中是否有环。
>
>为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。
>
> 
>
>示例 1：
>
>输入：head = [3,2,0,-4], pos = 1
>输出：true
>解释：链表中有一个环，其尾部连接到第二个节点。
>
>
>示例 2：
>
>输入：head = [1,2], pos = 0
>输出：true
>解释：链表中有一个环，其尾部连接到第一个节点。
>
>
>示例 3：
>
>输入：head = [1], pos = -1
>输出：false
>解释：链表中没有环。
>
>
>
>
>进阶：
>
>你能用 O(1)（即，常量）内存解决此问题吗？
>
>来源：力扣（LeetCode）
>链接：https://leetcode-cn.com/problems/linked-list-cycle
>著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
>
>编写 `public boolean hasCycle(ListNode head)` 函数

P.S.这道题，我审题出现了错误。原本以为要自己实现 `ListNode` 类，结果不需要。只要依据输入的链表编写 `hasCycle` 函数即可。

## 暴力思路

有环的链表，从头节点依次向下一个结点遍历，一定会出现死循环；而无环链表，就会正常跳出循环。

一种直接的思路时，依次遍历链表的每个结点，并把它缓存起来。之后碰到与缓存中相同的结点，说明链表有环；正常的循环终止，说明无环。

## Java 实现

```java
public boolean hasCycle(ListNode head) {

    List<ListNode> list = new ArrayList<>(); // 缓存结点

    while(head != null && head.next!=null){

        if(list.contains(head))              // 如果缓存中存在
            return true;                     // 直接返回 true

        list.add(head);                      // 否则，将该结点加入缓存
        head = head.next;                    // 继续遍历

    }

    return false;                            // 循环正常退出，非死循环，返回 false
}
```

由上述代码分析可得，该算法的**时间复杂度**为 O( n )，**空间复杂度**为 O( n )

根据题中描述，貌似存在只使用**常量级别**（ O(1) ）空间的算法，让我们来看看高手的方案。

# 高手方案

```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public boolean hasCycle(ListNode head) {
        if (head == null || head.next == null) {
            return false;
        }
        ListNode slow = head;
        ListNode fast = head.next;
        while (slow != fast) {
            if (fast == null || fast.next == null) {
                return false;
            }
            slow = slow.next;
            fast = fast.next.next;
        }
        return true;
    }
}
```

高手利用了**两个指针遍历**链表，但是却以**不同的速度**在遍历。如果链表中有环，那么在有限的循环中它们一定会相碰（即访问到同一个结点）；否则，一定会有一个指针（这个指针是快指针）提前到达链表尾部，从而返回无环的判断结果。

# 总结

* **双指针**但以**不同速度**遍历链表（类比田径比赛中的快慢运动员），可以判断链表是否有环，其空间复杂度为 O( 1 )