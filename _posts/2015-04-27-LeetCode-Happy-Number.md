---
layout: default
title: LeetCode Happy Number
comments: true
---

###Happy Number([原题连接](https://leetcode.com/problems/happy-number/))


###描述
Write an algorithm to determine if a number is "happy".

A happy number is a number defined by the following process: Starting with any positive integer, replace the number by the sum of the squares of its digits, and repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1. Those numbers for which this process ends in 1 are happy numbers.

Example: 19 is a happy number

12 + 92 = 82

82 + 22 = 68

62 + 82 = 100

12 + 02 + 02 = 1

###Solution
这道题算是一道比较容易的题，关键在于发现如果某个数是unhappy的，那么一定位于某个循环节中[（证明见维基）](http://en.wikipedia.org/wiki/Happy_number)。

发现这点，就会考虑记录每次判断的过的数，如果后续再次出现，那么一定位于死循环中。

可以考虑用hash set来做。

另外，还有更巧妙的方法，避免O(n)的空间：借鉴判断单链表是否有环所用的快慢指针，slow指针一次走一步，fast指针一次走两步，如果有环，fast一定会赶上slow。


```python
def calc(n):
    res = 0
    while n > 0:
        res += (n % 10) * (n % 10)
        n /= 10
    return res

class Solution:
    # @param {integer} n
    # @return {boolean}
    def isHappy(self, n):
        slow, fast = n, n
        while True:
            slow = calc(slow)
            if slow == 1:
                return True
            fast = calc(calc(fast))
            if fast == 1:
                return True
            if slow == fast:
                return False
        return True
```
