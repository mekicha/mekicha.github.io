---
layout: post
title: "Find first duplicate number in an array with minimal index"
categories: python algorithm interview
---

My solution to the interesting practice question from [codesignal](https://app.codesignal.com/interview-practice/task/pMvymcahZ8dY4g75q/description)

## Question statement

> Given an array a that contains only numbers in the range from 1 to a.length, find the first duplicate number for which the second occurrence has the minimal index. In other words, if there are more than 1 duplicated numbers, return the number for which the second occurrence has a smaller index than the second occurrence of the other number does. If there are no such elements, return -1.

## First approach: use a set

This is straightforward I will just show the code.

```python
def first_duplicate(arr):
    seen = set()
    result = -1
    for elem in arr:
        if elem in seen:
            result = elem
            break
        else:
          seen.add(elem)
  return result
```

## Second approach: use a frequency array

The key to this solution is contained in the first sentence of
the question, namely:

> an array a that **contains only numbers in the range from 1 to a.length**

This means we can build a frequency array by index from 0 to n, and return the first index that has more than one element.

The code is as follows:

```python
def first_duplicate(arr):
    freq = [0] * (len(arr) + 1)

    for elem in arr:
        freq[elem] += 1
        if freq[elem] > 1:
            return elem

    return -1
```
