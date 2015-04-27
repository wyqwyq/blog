---
layout: default
title: LeetCode Remove Linked List Elements
comments: true
---

###Happy Number([原题连接](https://leetcode.com/problems/remove-linked-list-elements/))


###描述
Remove all elements from a linked list of integers that have value val.

**Example**

Given: 1 --> 2 --> 6 --> 3 --> 4 --> 5 --> 6, *val* = 6

Return: 1 --> 2 --> 3 --> 4 --> 5

###Solution
这是一道比较容易的题，注意当链表所有元素都为val时的corner case和循环判断时注意不能用NULL*解引用（dereference）*。


```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    # @param {ListNode} head
    # @param {integer} val
    # @return {ListNode}
    def removeElements(self, head, val):
        dummyHead = ListNode(0)
        dummyHead.next = head
        cur = dummyHead
        while cur.next:
            p = cur.next
            while p and p.val == val:
                p = p.next
            cur.next = p
            if p:
                cur = p
        return dummyHead.next
```
