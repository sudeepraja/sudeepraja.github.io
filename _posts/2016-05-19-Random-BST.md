---
layout: post
title: Randomized Binary Search Trees
published: true
---
This post will be about using Randomization for constructing [Binary Search Trees (BST)](https://en.wikipedia.org/wiki/Binary_search_tree) whose expected height would be $$O(\log n)$$. This post is inspired from a question I was asked in my B.Tech Grand Viva.

The problem we are solving here is this:
Given $$n$$ distict integers in an array, construct a BST in expected $$O(n \log n)$$ time, whose expected height would be $$O(\log n)$$.

There are deterministic ways to construct height balanced BSTs. Popular methods taught in undergraduate curriculum are [AVL trees](https://en.wikipedia.org/wiki/AVL_tree) and [Red-Black Trees](https://en.wikipedia.org/wiki/Red%E2%80%93black_tree). But these data structures involve complex operations which are quiet difficult to remember.

One could also sort the array in $$O(n \log n)$$ time and recursivley use the medians to construct the required tree using this [algorithm](http://articles.leetcode.com/convert-sorted-array-into-balanced/). The recurrence relation for constructing the tree would be:

$$ T(n) = 2 \cdot T(\frac{n}{2}) + O(1)$$

This gives it a time complexity of $$O(n)$$, but since sorting is a pre-requisite for this, the complexity will be $$O(n \log n)$$.

We could remove the sorting step and directly employ a median finding algorithm such as [Quickselect[^footnote]](https://en.wikipedia.org/wiki/Quickselect), or something more complex such as [Median of medians](https://en.wikipedia.org/wiki/Median_of_medians).


