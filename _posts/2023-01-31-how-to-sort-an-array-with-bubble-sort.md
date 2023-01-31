---
layout: post
title: How to sort an array with Bubble sort 
date: 2023-01-31 23:14 +0100
tags: [arrays, sorting]
categories: [Algorithms]
---

## Introduction

I think this one was the first algorithm I've learned in college. The performance of this algorithm degrades quickly as
the number of items you need to sort grows. However, this algorithm gives you a good idea of
what algorithm thinking is.

## How it works?

The main idea of this algorithm is to divide the array into two logical partitions (**sorted** and **unsorted**) and,
traverse **unsorted** partition verifying if the item (`a[i]`) is bigger than the other (`a[j]`) right next to it. If
that condition is **true**, the items are `swapped` and the sorted partition grows from right to left.

Consider an unsorted array with the given elements: 20, 35, -15, 7, 55, 1, -22.

We will define two indexes to keep track of the logical partitions aforementioned:

* **Last unsorted Partition Index** &rarr; starts at the end of the array (length - 1). Used for traversing the array
  from
  right to left,
* **Current Index** &rarr; starts at the beginning of the array (i = 0). Used for traversing the array from
  left to right

See a snapshot of the array after each iteration over the unsorted partition. Notice that the bigger numbers are placed
ordered from right to left.

| Iteration # | Sorted Partition Index | Unsorted Partition Index | Array Snapshot           |
|-------------|------------------------|--------------------------|--------------------------|
| 1           | 0                      | 6                        | -22 20 -15 7 35 1 **55** |
| 2           | 1                      | 5                        | -22 1 -15 7 20 **35 55** |
| 3           | 2                      | 4                        | -22 1 -15 7 **20 35 55** |
| 4           | 3                      | 3                        | -22 -15 1 **7 20 35 55** |
| 5           | 4                      | 2                        | -22 -15 **1 7 20 35 55** |
| 6           | 5                      | 1                        | **-22 -15 1 7 20 35 55** |

You will also notice that some items find their positions before the end of the algorithm. This happens because the algorithm does many swaps and eventually some items will find the correct position before the end of the algorithm.

## Characteristics

* In-place algorithm. Means that the ordering is done without the need of using more arrays or other data structures.
* O(n^2) time complexity - quadratic
* Algorithm degrades quickly as the number of elements in the array grows

## Example

````java
package io.ellisonalves.bubblesort;

public class BubbleSortApp {
    public static void main(String[] args) {
        int[] intArray = {20, 35, -15, 7, 55, 1, -22};

        for (int lastUnsortedIndex = intArray.length - 1; lastUnsortedIndex > 0; lastUnsortedIndex--) {
            for (int currentIndex = 0; currentIndex < lastUnsortedIndex; currentIndex++)
                if (intArray[currentIndex] > intArray[lastUnsortedIndex])
                    swap(intArray, currentIndex, lastUnsortedIndex);
        }

        printArray(intArray);
    }

    public static void swap(int[] array, int i, int j) {
        if (i == j) {
            return;
        }
        int temp = array[i];
        array[i] = array[j];
        array[j] = temp;
    }

    private static void printArray(int[] intArray) {
        for (int j : intArray)
            System.out.print(j + " ");

        System.out.println();
    }
}
````

## Conclusion
This is a very simple algorithm that helps you on understanding the basics of how to sort arrays.
For now, I want to you to keep posted because I want to explore more this subject.