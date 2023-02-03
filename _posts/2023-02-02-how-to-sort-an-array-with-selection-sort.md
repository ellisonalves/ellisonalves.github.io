---
layout: post
title: How to sort an array with Selection Sort?
date: 2023-02-02 02:22
tags: [arrays, sorting]
categories: [Algorithms]
mermaid: true
---

## Introduction

The selection sort improves on the bubble sort by reducing the number of swaps necessary from O(n^2) to O(N). This
improvement can be significant for large records that must be physically moved around in memory. (Not the case in Java,
where references are moved, not entire objects).

The number of comparisons continues O(n^2).

## How it works?

This algorithm will pass through all the array comparing neighbors in order to find the largest one. Once this process
is finished, it will compare if the **selected** item is larger than the last item of the `unsorted partition`. If yes,
the numbers will be swapped, the `unsorted partition` will be decremented by one, meaning the `sorted partition` will
grow from right to left.

Here is a table showing this right to left movement happening:

| # of swaps | Array's Snapshot         |
|------------|--------------------------|
| 1          | 20 35 -15 7 -22 1 **55** |
| 2          | 20 1 -15 7 -22 **35 55** |
| 3          | -22 1 -15 **7 20 35 55** |
| 4          | **-22 -15 1 7 20 35 55** |

## Characteristics

* **In-place algorithm because**  &rarr; That means the algorithm doesn't need more than the initial amount of memory to
  sort the array;
* **O($$n^2$$) time complexity or quadratic** &rarr; For each element in the array we traverse N elements. In that case,
  the worst case would traverse the whole array twice in order to get it ordered. For example, it will take:
    * 100 steps to sort 10 items (10 x 10);
    * 10.000 steps to sort 100 items (100 x 100);
    * 1.000.000 steps to sort 1000 items (1000 x 1000);
* **Fewer swaps** &rarr; It doesn't require as much swapping as bubble sort because it only swaps when find the next
  largest element in the unsorted partition;
* **Usually performs better than bubble sort** &rarr; but it will depend on how the array being sorted is. For example,
  if you compare both worse cases it performs a bit better because this algorithm will probably do fewer swaps than
  bubble sort.
* **Unstable** &rarr; There is no guarantee that their original order relative to each other will be preserved. It is
  very possible that the second duplicated value will be placed in front of its twin;

## Implementation

```java
package io.ellisonalves.selectsort;

public class SelectionSortApp {
  public static void main(String[] args) {
    int[] intArray = {20, 35, -15, 7, 55, 1, -22};

    for (int lastUnsortedIndex = intArray.length - 1; lastUnsortedIndex > 0; lastUnsortedIndex--) {
      int largestNumberIndex = 0;
      for (int currentIndex = largestNumberIndex + 1; currentIndex <= lastUnsortedIndex; currentIndex++) {
        if (intArray[largestNumberIndex] < intArray[currentIndex]) {
          largestNumberIndex = currentIndex;
        }
      }
      swap(intArray, largestNumberIndex, lastUnsortedIndex);
    }
    printArray(intArray);
  }

  private static void printArray(int[] intArray) {
    for (int j : intArray)
      System.out.print(j + " ");

    System.out.println();
  }

  private static void swap(int[] array, int newLargestIndex, int lastUnsortedIndex) {
    if (newLargestIndex == lastUnsortedIndex)
      return;
    int temp = array[newLargestIndex];
    array[newLargestIndex] = array[lastUnsortedIndex];
    array[lastUnsortedIndex] = temp;

    printArray(array);
  }
}
```

## Conclusion

Notice that I've chosen for an implementation that grows the `sorted partition` from right to left, but I encourage you
to modify this example in order to make it grow from left to right.

Hope it helps you!

Stay tuned for more posts regarding algorithms!