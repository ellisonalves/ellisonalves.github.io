---
layout: post
title: Understanding Big O notation with arrays
date: 2023-01-01 22:01
tags: []
categories: []
mermaid: true
---

## Intro

## Big O Notation

## Arrays
Let's go through on what an array is and how it works

### Continuous block in memory
The most important thing to understand about arrays is how they're stored in memory. They're stored as one contiguous block in memory. Basically, based on the size and type of array, the JVM will allocate the necessary block in memory, side by side, in order to store the data.

So, the result is one huge block and that's why we have to specify the length of the array when we create it. Then the JVM knows how much memory it has to allocate for that array.

As a consequence, arrays have a static length and can't be resized because it wouldn't be possible to guarantee that the information is organized in contiguous block of memory.

### Every element occupies the same amount of space in memory
For example, In Java, an int array will store only ints. Therefore, as an int is 4 bytes in size, every item will occupy this amount of memory. So, to put another example, an array of ints with 10 elements would occupy 4x10 bytes of memory.

That's an example of a primitive type. So, what if we are working with objects? When working with objects what is stored in the array is the object reference.

For example, an array of Person will have as elements the object references to Person. As the object references are always the same in size regardless of the type of object they're referring to, the array will continue storing the information into contiguos block in memory.

### The memory address of each element in the array is calculated
In the JVM, the memory address of any element of the array is calculated by the expresion `x + i * y`. Therefore, if we know the index of an element, the time to retrieve it will be the same regardless the position of the element in the array.

That calculation is possible because of the first two points: **1) arrays are one large continguos block in memory; 2) every element in the array occupies the same amount of space in memory** .