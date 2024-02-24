---
layout: post
tag: easy
title: Longest Common Prefix
category: leetcode
excerpt_separator: <!--more-->
author: mico
date: 2023-10-01 14:18:10
---

Find the longest common prefix string amongst an array of strings.  

<!--more-->
The basic longest common prefix is an empty string, but we need to compare each word to a longest common prefix (LCP) candidate.

We initialize our candidate to be the first word in the list of strings, and the LCP to be an empty string.

To avoid hitting an index-out-of-range error, we loop through the shortest word of the two options: the LCP candidate, and the word we are currently processing.

When processing each word, we construct the prefix from the empty string, and keep adding the matching letters among the two words we are comparing: the LCP candidate, and the word at hand.

When we go through all the words, we return our LCP candidate, which is the latest LCP we constructed by adding common characters.

```python
def longestCommonPrefix(self, strs: List[str]) -> str:
  lcp_candidate = strs[0]
  lcp = ""

  for word in strs:
    shorter_word = lcp_candidate if len(word) > len(lcp_candidate) else word
    the_other_word = word if len(word) > len(lcp_candidate) else lcp_candidate

    for i, c in enumerate(shorter_word):
      if c == the_other_word[i]:
        lcp += c
      else:
        break
    lcp_candidate = lcp
    lcp = ""

  return lcp_candidate
```

## O-Notation:

We must process each word in the array with $$N$$ elements, for each element we compare our candidate to the current word but we only loop through the letters in our LCP candidate of size $$L$$. Therefore our worst case is $$\mathcal{O}(N \times L)$$ where $$L$$ is the size of our LCP candidate.

## Lessons Learnt:

The low memory performance is due to the initialization of `shorter_word` and `the_other_word` when processing each word in the list.
This can be optimized by checking whether our `word` starts with `lcp_candidate`, if it does not, we keep deleting the last letter from the `lcp_candidate` until it does. This also helps avoid re-constructing the LCP from scratch each iteration.

## Notes:
* [Link to Problem](https://leetcode.com/problems/longest-common-prefix/description/)
* Runtime beats 48.13% of `python3` submissions
* Memory usage beats 21.57% of `python3` submissions
