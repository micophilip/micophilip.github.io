---
layout: post
tag: dynamic-programming
title: Dynamic Programming Series
category: posts
excerpt_separator: <!--more-->
author: mico
date: 2026-03-22 21:36:42 UTC
---

In an effort to keep this thing active, I thought of starting a new series on dynamic programming.
<!--more-->

I am not necessarily looking to provide yet another tutorial. There are a ton of resources online on the topic and probably a 30-minute back and forth with Claude can go a long way. I thought, however, it is not a bad way to keep this blog active and force me to write more. Who knows, maybe I can also provide a different perspective in the sea of resources on dynamic programming.

## The Problem

> How many distinct ways can you climb $n$ steps of stairs if you can only take either 1 step or 2 steps?

Following a common pattern of writing the bruteforce solution by hand not by code. To get to the 3rd step, I can either take 1 step followed by 2 steps, or I take 2 steps followed by 1 step, or I take 1 step, another 1 step and a final 1 step. The base solution is one way is always taking 1 step at a time, and at another way using 2 steps regardless if $n$ is even or odd. If it's even, it'll just be 2 steps all the way, if it's odd, it'll be 2 steps until the second-to-last step where I'll just take 1 step. But it doesn't change the number of ways. So there's at least 2 ways for $n>1$, and 1 way for $n=1$. What about the number of ways I can alternate the 1 and 2 steps? (as opposed to taking 1 step all the way or taking 2 steps all the way)

## Back to Basics

This leads to the basic realization that the solution involves doing "_something_" with $+1$ and add it to "_something_" with $+2$.

It is somehow basic knowledge that dynamic programming is breaking problems into subproblems. But two main characteristics stand out highlighted in the chapter on dynamic programming from [Introduction to Algorithms](https://mitpress.mit.edu/9780262046305/introduction-to-algorithms/) book.

1. We are trying to find an optimal _value_ not necessarily an optimal _solution_. For the stairs example, this simply means we need to find the number of ways, not list the ways themselves.
2. These subproblems overlap. In other words, you need to only compute them once and the good old divide-and-conquer (recursion without memoization) will solve the same subproblem over and over and over.
3. "Programming" is not for "Coding". It means tabulating, or the dictionary definition "arrange according to a plan or schedule.". That is, storing the optimal value in a table.

## Manual Solution

Let's take an example of $n=5$. We can get to 5th step from either the 3rd (because we can take 2 steps), or from the 4th (because we can take 1 step). So the number of ways to get to $n=5$, is simply the sum of the ways to get to $n=3$, and the numbero of ways to get to $n=4$, because once I reach step 3 or 4 I know I just need to add 2 steps or 1 step respectively to get to 5. But how many ways to get to 3?

To get to 3, I can do:
1. 1 $\rightarrow$ 1 $\rightarrow$ 1
2. 1 $\rightarrow$ 2
3. 2 $\rightarrow$ 1

That is, 3 different ways.

And how many ways to get to 4? I can do:

1. 1 $\rightarrow$ 1 $\rightarrow$ 1 $\rightarrow$ 1
2. 2 $\rightarrow$ 1 $\rightarrow$ 1
3. 1 $\rightarrow$ 2 $\rightarrow$ 1
4. 1 $\rightarrow$ 1 $\rightarrow$ 2
5. 2 $\rightarrow$ 2

But these will only get us to 3 or 4, we wanna get to 5! This still does not change the number of ways.

To get to 5, I can add 2 steps from the $n=3$ solution, and I can add 1 step from $n=4$ solution:

1. 1 $\rightarrow$ 1 $\rightarrow$ 1 $\rightarrow$ <span style="color:red">2</span>
2. 1 $\rightarrow$ 2 $\rightarrow$ <span style="color:red">2</span>
3. 2 $\rightarrow$ 1 $\rightarrow$ <span style="color:red">2</span>
4. 1 $\rightarrow$ 1 $\rightarrow$ 1 $\rightarrow$ 1 $\rightarrow$ <span style="color:red">1</span>
5. 2 $\rightarrow$ 1 $\rightarrow$ 1 $\rightarrow$ <span style="color:red">1</span>
6. 1 $\rightarrow$ 2 $\rightarrow$ 1 $\rightarrow$ <span style="color:red">1</span>
7. 1 $\rightarrow$ 1 $\rightarrow$ 2 $\rightarrow$ <span style="color:red">1</span>
8. 2 $\rightarrow$ 2 $\rightarrow$ <span style="color:red">1</span>

That is, still 8 ways, still the number of ways to get to 3 $+$ number of ways to get to 4. More generally, the number of ways to get to $n-2$ plus the number of ways to get to $n-1$. Credit to [this comment](https://leetcode.com/problems/climbing-stairs/editorial/comments/604446/?parent=592775) that inspired this explanation.

## Programmatically

In python, this simply meant:

```python
def ways_to_climb(n: int) -> int:
    if n == 1:
        return n # Base case

    dp = [0] * (n+1) # Initialize the optimal value array
    dp[1] = 1
    dp[2] = 2
    for i in range(3, n+1): # For the rest
        dp[i] = dp[i-2] + dp[i-1]
    
    return dp[n]
```