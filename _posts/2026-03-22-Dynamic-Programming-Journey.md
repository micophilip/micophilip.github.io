---
layout: post
tag: dynamic-programming
title: Refreshing my Dynamic Programming Skills
category: posts
excerpt_separator: <!--more-->
author: mico
date: 2026-03-22 21:36:42 UTC
---

I embarked on a new journey to refresh my understanding of dynamic programming.
<!--more-->

Even though I use dynamic programming every day without actually calling it that. Somehow, I dodged having to solve dynamic programming problems in tech interviews. Part of that started as a desire to put my LeetCode subscription to good use, and another is a genuine desire to stay "up-to-date" pretending that my good buddy Claude hadn't just deemed all this knowledge unnecessary.

## The Problem

I started with a LeetCode study plan for dynamic programming and the first problem was: "in how many distinct ways can you climb $n$ steps of stairs if you can only take either 1 step or 2 steps". I stared at it for a good 15 minutes, even after reading the hint a mere 45 seconds after reading the problem. The hint was awkwardly phrased: "To reach nth step, what could have been your previous steps? (Think about the step sizes)". Which to me sounded like just another way to frame the question and not really a hint to a possible solution. "what could have been your previous steps" is another way of saying "how many ways to get to the top of the stairs", and "think about the step sizes" was already part of the question, step size is either 1 or 2. OK, hint useless.

## Solution Journey

Then I followed a common pattern of writing the bruteforce solution by hand not by code. OK, to get to the 3rd step, I can either take 1 step followed by 2 steps, or I take 2 steps followed by 1 step, or I take 1 step, another 1 step and a final 1 step. OK still doesn't lead me anywhere. Except that the base solution is there's at least 1 way is always taking 1 step at a time, and at least one other way using 2 steps regardless if $n$ is even or odd. If it's even, it'll just be 2 steps all the way, if it's odd, it'll be 2 steps until the second-to-last step where I'll just take 1 step. But it doesn't change the number of ways.

## The Simple Realization

This led me to the basic realization that the solution involves doing "_something_" with $+1$ and add it to "_something_" with $+2$.

As I was reading the editorial solution, I already felt so much pride as soon as I saw that part of the solution was:

```python
dp[i] = dp[i-1] + dp[i-2]
```

Bingo, that's the "_something_" I was looking for! But why minus though in the indices $i-1$ and $i-2$ not plus? What's that array we are populating? Then I started reading the comments and I stumbled upon [this explanation](https://leetcode.com/problems/climbing-stairs/editorial/comments/604446/?parent=592775) that made things a little clearer. The author solved it manually to explain, but with a larger number $n=5$ instead of my $3$. That allowed them to go for a little longer, and it was obvious that to get to 5, you can either get to it from 3 (2 steps) or you get to it through 4 (1 step). And _that_ ladies and gentlemen was the reason for the subtraction! Through reading the explanation I also understood that order matters. In other words, 1 + 2 steps is a different way from 2 + 1 steps. 

## Reflection

Obviously as a software developer I always knew that dynamic programming is breaking problems into subproblems. But two main characteristics stood out as soon as I read the chapter on the topic from [Introduction to Algorithms](https://mitpress.mit.edu/9780262046305/introduction-to-algorithms/) book.

1. We are trying to find an optimal _value_ not necessarily an optimal _solution_. For the stairs example, this simply means we need to find the number of ways, not list the ways themselves.
2. These subproblems overlap. In other words, they are dependent solutions. In other words, you need to only compute them once. In other words, good old divide-and-conquer (recursion without memoization) will solve the same subproblem over and over and over.
3. "Programming" is not for "Coding". It means tabulating, or the dictionary definition "arrange according to a plan or schedule.". That is, storing the optimal value in a table.

For the $n=5$ example, this meant that the solution for the subproblem (getting to $n=3$) only needs to be figured out once. So the number of ways to get to $n=5$, is simply the sum of the ways to get to $n=3$, and the ways to get to $n=4$, because once I reach step 3 or 4 I know I just need to add 2 steps or 1 step respectively to get to 5.

For example, to get to $3$, I can do:
1. 1 $\rightarrow$ 1 $\rightarrow$ 1
2. 1 $\rightarrow$ 2
3. 2 $\rightarrow$ 1

That is, 3 different ways.

To get to $4$, I can do:

1. 1 $\rightarrow$ 1 $\rightarrow$ 1 $\rightarrow$ 1
2. 2 $\rightarrow$ 1 $\rightarrow$ 1
3. 1 $\rightarrow$ 2 $\rightarrow$ 1
4. 1 $\rightarrow$ 1 $\rightarrow$ 2
5. 2 $\rightarrow$ 2

But these will only get us to $3$ or $4$, we wanna get to $5$! This still does not change the number of ways.

To get to $5$, I can add 2 steps from the $n=3$ solution, and I can add 1 step from $n=4$ solution:

1. 1 $\rightarrow$ 1 $\rightarrow$ 1 $\rightarrow$ <span style="color:red">2</span>
2. 1 $\rightarrow$ 2 $\rightarrow$ <span style="color:red">2</span>
3. 2 $\rightarrow$ 1 $\rightarrow$ <span style="color:red">2</span>
4. 1 $\rightarrow$ 1 $\rightarrow$ 1 $\rightarrow$ 1 $\rightarrow$ <span style="color:red">1</span>
5. 2 $\rightarrow$ 1 $\rightarrow$ 1 $\rightarrow$ <span style="color:red">1</span>
6. 1 $\rightarrow$ 2 $\rightarrow$ 1 $\rightarrow$ <span style="color:red">1</span>
7. 1 $\rightarrow$ 1 $\rightarrow$ 2 $\rightarrow$ <span style="color:red">1</span>
8. 2 $\rightarrow$ 2 $\rightarrow$ <span style="color:red">1</span>

That is, still 8 ways, still the number of ways to get to $3$ $+$ number of ways to get to $4$. More generally, the number of ways to get to $n-2$ plus the number of ways to get to $n-1$.

In python, this simply meant:

```python
def ways_to_climb(n: int) -> int:
    if n == 1:
        return n # Base case

    dp = [0] * (n+1) # Initialize the optimal value array
    dp[1] = 1
    dp[2] = 2
    for i in range(3, n+1):
        dp[i] = dp[i-2] + dp[i-1]
    
    return dp[n]
```