---
layout: post
tag: easy
title: Valid-Parentheses
category: leetcode
excerpt_separator: <!--more-->
author: mico
date: 2024-02-24 14:40:58
---

Return whether a string is a valid set of parantheses.  
<!--more-->
A valid set of parantheses is defined as a closing bracket for every opening bracket, in order. Checking for valid parantheses gives a cue that this is a stack problem. We push every opening bracket to the stack, and when it is a closing bracket we pop and check if the popped item is its corresponding opening bracket. If it is not, we return `False`. After we check all the characters in the input string, we rether `True` if the stack is not empty. Otherwise, it implies that we have not seen a closing bracket for every opening bracket.

One major and obvious drawback of this algorithm, is that it does not use a map to store the closing brackets and their corresponding opening brackets. To achieve the same purpose, the algorithm uses a function. Which is not only redundant, but overly complex and harder to read. On the plus side, its memory usage beats 98.96% of `python` submissions 🤷🏻‍♂️

```python
def isValid(self, s: str) -> bool:
  is_open_paranthesis = lambda c: c == "("
  is_close_paranthesis = lambda c: c == ")"

  is_open_braces = lambda c: c == "{"
  is_close_braces = lambda c: c == "}"

  is_open_square = lambda c: c == "["
  is_close_square = lambda c: c == "]"

  stack = []

  for c in s:
    if is_open_paranthesis(c) or is_open_braces(c) or is_open_square(c):
      stack.append(c)
    elif is_close_paranthesis(c) and stack:
      if not is_open_paranthesis(stack.pop()):
        return False
    elif is_close_braces(c) and stack:
      if not is_open_braces(stack.pop()):
        return False
    elif is_close_square(c) and stack:
      if not is_open_square(stack.pop()):
        return False
    else:
      return False

  return not stack
```

## O-Notation:

We are only traversing the list once, a classic $$\mathcal{O}(n)$$. Space is also $$\mathcal{O}(n)$$ since we only have the stack that we keep appending to with `len(s)`, i.e., `n`, as the worst case scenario.

## Lessons Learnt:

* Use a map to represent constants such as opening brackets and their corresponding closing brackets.

## Notes:
* [Link to Problem](https://leetcode.com/problems/valid-parentheses/)
* Runtime beats 13.66% of `python3` submissions
* Memory usage beats 98.96% of `python3` submissions
