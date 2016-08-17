---
layout: post
title: "Algorithm Thoughts: Largest Rectangular Area in Histogram"
tags:
  - Algorithm
---

Given a list of N non-negative integers representing the histogram's bar heights, find the area of largest rectangle in the histogram.

There are linear solutions to this problem, which are a bit tricky. I'm just writing this down for future reference.

<!--more-->

The idea is to use a stack to store the index whose value(height) is smaller than or equal to current height. This makes sure the elements in the stack is in an increasing order. When the histogram starts to decline, pop the stack and calculate area that ends with the popped height. The width is calculated by the popped index and top element in the stack. All the elements that has smaller height will be popped and then the current index will be pushed. The returning value will be compared/picked everytime a area size calculated.

```python
def largestRectangleArea(heights):
    res, stk = 0, []
    for i,v in enumerate(heights):
        while stk and heights[stk[-1]] > v:
            h = heights[stk.pop()]
            width = i if not stk else i - 1 - stk[-1]
            res = max(res, h*width)
        stk.append(i)
    return res
```