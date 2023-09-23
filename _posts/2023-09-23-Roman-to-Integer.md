---
layout: post
tag: easy
title: Roman to Integer
category: leetcode
excerpt_separator: <!--more-->
author: mico
date: 2023-09-23 15:29:19
---

Given a roman numeral, convert it to an integer.  
<!--more-->

There are two main considerations for this problem: certain roman letters simply trigger an addition to the final result, while others (such as $$I$$ followed by $$V$$ or $$X$$ and $$X$$ followed by $$L$$ or $$C$$ and $$C$$ followed by $$D$$ or $$M$$) trigger a subtraction.  

Since when to subtract depends on the following letter, and to avoid looking ahead or doing multiple additions and subtractions (yuck 🤮), we place the subtraction-triggering letters in a "hold" position. If the following letter satisfies the subtraction condition (such as being either $$V$$ or $$X$$ if the hold letter is $$I$$), subtraction rules are applied accordingly. If not, we add the corresponding integer of the hold letter to the final result and reset the hold.  

If we are processing the last letter, and we are in a hold position, we add the hold-corresponding integer to the final result.

```python
def roman_to_int(s: str) -> int:
  roman_dict = {
    "I": 1,
    "V": 5,
    "X": 10,
    "L": 50,
    "C": 100,
    "D": 500,
    "M": 1000
  }

  holds = {
    "I": ["V", "X"],
    "X": ["L", "C"],
    "C": ["D", "M"]
  }

  hold = ""
  result = 0

  for i, c in enumerate(s):

    if hold and c in holds[hold]:
      to_add = roman_dict[c] - roman_dict[hold]
      result += to_add
      hold = ""
    elif hold:
      result += roman_dict[hold]
      if c in holds:
        hold = c
      else:
        result += roman_dict[c]
        hold = ""
    else:
      if c in holds:
        hold = c
      else:
        result += roman_dict[c]

    if i == len(s) - 1 and hold:
      result += roman_dict[hold]
  
  return result
```

## O-Notation:

Since we know that the roman numerals cannot be an arbitrary number of letters, our runtime is $$\mathcal{O}(1)$$. While our `holds` is a dictionary with a list as a value, it is negligible to the overall runtime. We can replace it by a simple condition (i.e., `if hold == "I" and (c == "V" or c == "X")`)

## Lessons Learnt:
* Always remember to return!
* Under the `elif hold` condition, I was simply setting the `hold = c if c in holds else ""`. This ignored the fact that I still need to add the current character to the result. For example, if a $$C$$ was in a hold position, and current character is a $$V$$, I still need to add the 5 to the result.
* I had a `continue` statement when `hold` needs to be set to `c`. This prevented me from adding the hold character to the final result if I am processing the last character in the roman numeral string. In this case, `continue` made that too late since we exited the loop and returned the result ignoring to add the hold character (Test Case: $$III$$)
* We can treat cases such as $$IV$$ and $$CM$$ as an entry to our dictionary. That way we can always take a look at two letters at a time. This will also greatly simplify our conditions.
* My first instinct was that since we are looking at each character in the input string once, our runtime would be $$\mathcal{O}(n)$$. However this would only be true if there is an arbitrary number of roman numerals, which is not the case. The maximum number of a valid roman numeral corresponds to 3,999 which can be represented as $$MMMCMXCIX$$ with $$n=9$$. Once we hit 4,000, which can be expressed as $$MMMM$$, we violate the principle of not repeating the same letter four times<sup>1</sup>. 
* Most imprtantly, this is more of a brute-force solution. The amount of conditions can be greatly simplified if we chose the look-ahead approach instead. That way I will not be checking the hold value constantly and adjusting it accordingly.

## Notes:
* [Link to Problem](https://leetcode.com/problems/roman-to-integer/description/)
* [Link to Submission](https://leetcode.com/submissions/detail/1057319893/)
* Runtime beats 41.23% of `python3` submissions
* Memory usage beats 72.91% of `python3` submissions

## References:
[1] [Roman Britain](https://www.roman-britain.co.uk/life-in-roman-britain/the-roman-numeric-system/#:~:text=The%20highest%20number%20that%20can,of%20the%20same%20type%20together)
