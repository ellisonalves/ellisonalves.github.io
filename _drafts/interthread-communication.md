---
layout: post
title: Inter thread communication
date: 2023-01-01 22:01
tags: [Multi-threading]
categories: [Java]
mermaid: true
---

# Introduction

Inter-thread communication in Java is the mechanism which allows multiple threads to communicate between themselves in order to co-operate to finish their tasks.

## Memory management of threads
* Threads have their own stack memory
* Threads share the heap memory
* Processes run in a separate memory space

### Stack memory
* Local variables
* Method arguments
* Method calls

### Heap memory
* Referenced Objects


## Synchronization
Allows threads sharing resources.
### Intrinsic lock (Monitor)
* Every Java object has it
* A thread that needs excllusive and consistent access to an object's fields has to acquire the object's intrinsic lock before accessing them, and then release the intrinsic lock when it's done with them
* Because of the monitor, just one thread can execute the same synchronized method at the same time
* When a thread acquire the intrinsic lock, no matter if it is at class or object leve, the next thread will have to wait for the first one. Even if the threads use different resources.

## Atomic Operation
* It is an operation that gives the same result, no matter how many threads interfere in the process.
* Works as one single unit


