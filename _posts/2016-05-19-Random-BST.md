---
layout: post
title: Randomized Binary Search Trees
published: true
project: false
---
This post will be about using Randomization for constructing [Binary Search Trees (BST)](https://en.wikipedia.org/wiki/Binary_search_tree) whose worst case height must be $$O(\log n)$$. This post is inspired from a question I was asked in my B.Tech Grand Viva.

The problem we are solving here is this:
Given $$n$$ distinct integers in an array, construct a BST in expected $$O(n \log n)$$ time, whose worst case height must be $$O(\log n)$$.

There are deterministic ways to construct height balanced BSTs. Popular methods taught in undergraduate curriculum are [AVL trees](https://en.wikipedia.org/wiki/AVL_tree) and [Red-Black Trees](https://en.wikipedia.org/wiki/Red%E2%80%93black_tree). But these data structures involve complex operations which are quite difficult to remember.

One could also sort the array in $$O(n \log n)$$ time and recursivley use the medians as pivots to construct the required tree using this [algorithm](https://articles.leetcode.com/convert-sorted-array-into-balanced/). The recurrence relation for constructing the tree would be:

$$ T(n) = 2 \cdot T(\frac{n}{2}) + O(1)$$

This gives it a time complexity of $$O(n)$$, but since sorting is a pre-requisite for this, the complexity will be $$O(n \log n)$$.

We could remove the sorting step and directly employ a median finding algorithm such as [Quickselect](https://en.wikipedia.org/wiki/Quickselect), or something more complex such as [Median of medians](https://en.wikipedia.org/wiki/Median_of_medians). Assuming we use the stronger, median of medians algorithm, the recurrence relation would be:

$$ T(n) = 2 \cdot T(\frac{n}{2}) + O(n)$$

This gives us the same time complexity of $$O(n \log n)$$.

By using medians to construct the BST, we would always be constructing the same tree. It will not be randomized. Although using Quickselect would make it a [Las Vegas algorithm](https://en.wikipedia.org/wiki/Las_Vegas_algorithm) giving it a randomized flavor.

What if we could get away with using something other that the median as a pivot. Recall the [Partitioning](https://en.wikipedia.org/wiki/Quicksort#Algorithm) step in quicksort which takes $$O(n)$$ time.

>Partitioning: reorder the array so that all elements with values less than the pivot come before the pivot, while all elements with values greater than the pivot come after it (equal values can go either way). After this partitioning, the pivot is in its final position. This is called the partition operation.

Lets call an element GOOD if after partitioning the array of length $$n$$ using it, the element lies in between the indices $$n/3$$ and $$2n/3$$ (lies in the middle one third). A subroutine to find a GOOD element could be:
	
    Choose an element at random from the array
    While chosen element is not GOOD:
    	Choose another element at random
    Return chosen element
    
Since there are $$n/3$$ GOOD elements in an $$n$$ element array, the probability of finding one is $$1/3$$. The expected runtime of the above subroutine will be:

$$E[time] = n \cdot \frac{1}{3} + 2n \cdot \frac{2}{3} \cdot \frac{1}{3} + 3n \cdot \left( \frac{2}{3} \right) ^{2} \cdot \frac{1}{3} + 4n \cdot \left( \frac{2}{3} \right) ^{3} \cdot \frac{1}{3} + ....$$

$$E[time] = \frac{n}{2} \cdot \sum_{i=1}^{\infty} i \cdot \left( \frac{2}{3} \right)^{i} $$

You can recognize the summation from my [previous post](https://sudeepraja.github.io/Hello-World/). It evaluates to:

$$E[time] = \frac{n}{2} \cdot \frac{\frac{2}{3}}{(1-\frac{2}{3})^2} = 3n$$

A GOOD element can be found in $$O(n)$$ time. The recurrence for constructing the tree would be:

$$T(n) = T(\frac{(1+c)n}{3}) + T(\frac{(2-c)n}{3}) + O(n), 0\le c \le 1$$

$$T(n) \le 2 \cdot T(\frac{2n}{3}) + O(n)$$

The time complexity for constructing the tree would be $$O(n \log n)$$. The worst case height of the tree would be $$ \log_{3/2} n = O(\log n)$$.

The algorithm presented above is Las Vegas. Is there a [Monte Carlo Algorithm](https://en.wikipedia.org/wiki/Monte_Carlo_algorithm) for constructing a BST of expected height $$O(\log n)$$?

The height of the tree depeneds on the sequence of insertions. There are $$n!$$ ways to insert $$n$$ elements of an array into a binary search tree (This is different from the number of distinct BSTs possible which is the n'th [Catalan Number](https://en.wikipedia.org/wiki/Catalan_number)). But only 2 of these would give a completley skew tree. A lot of permutations would result in trees which are well balanced. Number of permutations of $$1,2,...n$$ that result in a binary search tree with the minimum possible height is given by the sequence in [this OEIS page](https://oeis.org/A076615).

A very simple Monte Carlo algorithm would be:

	Create empty tree
    Randomly shuffle the array elements
    Insert elements in the order in which they appear in the array.
    
This algorithm will create a tree in $$O(n \log n)$$ time, whose expected height is $$O(\log n)$$.The Las Vegas algorithm will never output a skew tree, but the Monte Carlo algorithm might with very low probability.

Sophisticated randomized BSTs which perform well in face of random insertions and deletions are also present, but these are beyond the scope of this blog. Check out [Treaps](https://en.wikipedia.org/wiki/Treap) which uses random priorities and this [randomized binary search tree](https://algo.inria.fr/seminars/sem96-97/martinez.html), which uses random rebalancing.
