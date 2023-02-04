---
layout: post
title: How to sort an array with Insertion Sort?
date: 2023-02-04 02:57
tags: [arrays, sorting]
categories: [Algorithms]
mermaid: true
---

## Introduction

The insertion sort still executes in O(N^2) time, but sort performs better than
**Bubble Sort** and **Selection Sort**. It's an elementary sort algorithm, not
too complex, and it's often used as the final stage of more sophisticated sorts,
such as quicksort.

## How it works?

Once more, the array is divided into two logical partitions: `sorted partion` and `unsorted partition`.
Before starting, we assume we have a logical partition of size 1. As this partition just have one item,
it is automatically ordered. How to do that? Simple:

```java
int[]intArray={20,35,-15,7,55,1,-22};
for(int unsortedIndex=1;unsortedIndex<intArray.length;unsortedIndex++)
```

In the snippet above, we have:

1. The unsorted array
2. The following code: `for (int unsortedIndex = 1 ...`.

Notice we are passing through the unsorted array starting at the position 1. That means we consider the
position 0 our sorted partition already sorted! As we have a sorted partition with one element, the goal of the
Insertion Sort algorithm consists in growing the `sorted partition` from left to right using the following steps:

1. Get a new element from the unsorted partition
2. Grow the sorted partition to accommodate the new element (We are not really growing because it is an in-place
   algorithm, but we are growing it logically)
3. Shift the sorted partition items to right until find a position to **insert** the new element.
4. The new element will be inserted where `newElement < array[index - 1]`

Let's see the evolution of the sorting process after each completion of the outer loop:

| Number of Iteration | Snapshot of the array    |
|---------------------|--------------------------|
| 1                   | **20 35** -15 7 55 1 -22 |
| 2                   | **-15 20 35** 7 55 1 -22 |
| 3                   | **-15 7 20 35** 55 1 -22 |
| 4                   | **-15 7 20 35 55** 1 -22 |
| 5                   | **-15 1 7 20 35 55** -22 |
| 6                   | **-22 -15 1 7 20 35 55** |

## Characteristics

* In-place algorithm
* O(n^2) time complexity or quadratic
* It will take:
    * 100 steps to sort 10 items (10 x 10)
    * 10.000 steps to sort 100 items (100 x 100)
    * 1.000.000 steps to sort 1000 items (1000 x 1000)
* Stable algorithm &rarr; the original order of duplicated items will be preserved.

## Implementation

```java
package io.ellisonalves.insertionsort;

public class InsertionSortApp {
    public static void main(String[] args) {
        int[] intArray = {20, 35, -15, 7, 55, 1, -22};
        for (int unsortedIndex = 1; unsortedIndex < intArray.length; unsortedIndex++) {
            int newElement = intArray[unsortedIndex];
            int sortedIndex;
            for (sortedIndex = unsortedIndex; sortedIndex > 0 && newElement < intArray[sortedIndex - 1]; sortedIndex--)
                intArray[sortedIndex] = intArray[sortedIndex - 1]; // shift numbers to right

            intArray[sortedIndex] = newElement;
            printArray(intArray);
        }
    }

    private static void printArray(int[] intArray) {
        for (int j : intArray)
            System.out.print(j + " ");

        System.out.println();
    }

}
```

## Conclusion

As always, I encourage you to modify this example and find out other ways of implementing the same logic. That approach
is excellent when we are learning any kind of Algorithms.

Hope it helps you!

Stay tuned for more posts regarding algorithms!