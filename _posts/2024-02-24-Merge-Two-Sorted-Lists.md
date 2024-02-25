---
layout: post
tag: easy
title: Merge Two Sorted Lists
category: leetcode
excerpt_separator: <!--more-->
author: mico
date: 2024-02-24 18:28:38
---

Given two sorted linked lists, return a merged sorted linked list.
<!--more-->
The merged linked list is achieved through interweaving both linked lists. I solved this problem using recursion. Possibly because the LeetCode topic was marked as "Recursion" which inadvertently gave me an unsolicited hint.

Like any typical recursion problem, I started with the base case. In this case, the base case is when both lists are empty. In which case, we return `None`. While according to the class definition, `val` can never be `None` as it is initialized to 0, LeetCode compiler was throwing an error when comparing `val` of both lists, so added a check for both non-null values of both lists. If the head of the first list is smaller, we call the function recursively with the remainder of the first list along with the second list. We return a head node with first list's head value and the next set to the result of the recursive call. If the head of the second list is smaller, we do the opposite.

If only the first list is not null, we return its head and its next. Similarly, if only the second list is not null, we return its head and its next.

```python
class ListNode:
  def __init__(self, val=0, next=None):
    self.val = val
    self.next = next

def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
  if not list1 and not list2:
    # Done both lists
    return None

  if list1 and list2:
    if list1.val <= list2.val:
      result = self.mergeTwoLists(list1.next, list2)
      return ListNode(list1.val, result)
    else:
      result = self.mergeTwoLists(list1, list2.next)
      return ListNode(list2.val, result)
  elif list1:
    return ListNode(list1.val, list1.next)
  elif list2:
    return ListNode(list2.val, list2.next)
```

## O-Notation:

Since we are going through each list, time complexity would be $$\mathcal{O}(m+n)$$. Space complexity given the recursive call would also be $$\mathcal{O}(m+n)$$

## Lessons Learnt:
* By "splicing", the problem meant to not construct a new list, but instead modify the existing lists. As of the time of writing, it was only explicitly mentioned that we should not copy the value into a new list in the solution. Looks like others misunderstood that detail as my solution's memory usage beat 46.43% of python submissions!
* First problem to create a ton of test cases using randomized numbers before submission!

## Notes:
* [Link to Problem](https://leetcode.com/problems/merge-two-sorted-lists/description/)
* Runtime beats 56.18% of `python3` submissions
* Memory usage beats 46.43% of `python3` submissions
