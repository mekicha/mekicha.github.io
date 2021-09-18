---
layout: post
title: "Find first non-repeating character in a string"
categories: python algorithm interview
---

My solution to the interesting practice question from [codesignal](https://app.codesignal.com/interview-practice/task/uX5iLwhc6L5ckSyNC/description)

## Question statement

> Given a string s consisting of small English letters, find and return the first instance of a non-repeating character in it. If there is no such character, return '\_'

## First approach: use dict

From Python3.7, dicts are ordered by insertion. This is also true in Python3.6, as an implementation detai. See this [answer on StackOverflow](https://stackoverflow.com/a/39980744).

We can leverage this fact to keep a count of the letters, and then return the first letter that has count of `1`. Like so:

```python
def first_non_repeating_character(s):
    count = {}
    for c in s:
        count[c] = count.setdefault(c, 0) + 1

    for c in s:
        if count[c] == 1:
            return c
    return '_'
```

## Second approach: use a frequency table

Here, we make use of the fact that the string only contains small English letters. That means we can keep the counts in a 26-length array.
The code below is hopefully clear enough on how that works.

```python
def first_non_repeating_character(s):
    freq_a = [0] * 26

    for ch in s:
        index = ord(ch) - ord('a')
        freq_a[index] += 1

    for ch in s:
        index = ord(ch) - ord('a')
        if freq_a[index] == 1:
            return ch

    return '_'
```
