---
layout: post
title: Junit and Multi-Threading Scenarios
date: 2021-11-23 19:15 +0100
tags: [UnitTesting, MultiThreading]
categories: [Java]
mermaid: true
---

Not a long time ago I was exploring how to create an unit test for multi-threading scenarios without the need of using external libraries. I was able to achieve it by doing a pair programming session with a colleague and we had a lot of fun!
That experience was so cool that I decided to invest more time on this particular problem to refactor and share what I've learned. Let's start.

## The environment
The environment used for this study is:
* Java 17
* JUnit 5.8.1
* Gradle 7.3

## The problem
The problem consists in persisting a set of persons into an in-memory data structure and the only requirement is: the data structure can't have more than 10 persons.
This requirement will allow us to explore what happens to the shared resource (the data structure) when there are many threads reading and writing it.
Let's start exploring it without worrying about multi-threading requirements. The outcome would be a record to represent the person

```java
public record Person(String name) {
}
```

and a repository to handle persistence operations of persons:

```java 
public class PersonRepository {
    private final Set<Person> persons;
    public PersonRepository(Set<Person> persons) {
        this.persons = persons;
    }
    void persist(Person person) {
            if (persons.size() >= 10) {
                throw new IllegalStateException("The repository can not handle more than 10 persons");
            }
            persons.add(person);
    }
}
```

This implementation would work perfectly for a single thread environment but for a multi-threading environment we would experience inconsistencies due to the shared resource (Set<Person>) being read and written simultaneously without any access control policy.
Let's create a failing test, so it will be possible to verify that the code is not able to handle concurrent scenarios consistently.

```java 
class HandlingMultiThreadingScenarios {
    ExecutorService executorService;
    @BeforeEach
    void setUp() {
        executorService = Executors.newFixedThreadPool(3);
    }
    @AfterEach
    void tearDown() throws InterruptedException {
        executorService.shutdown();
        awaitTerminationAndShutdownNow();
    }
    @RepeatedTest(15)
    void shouldPersistPersonCorrectlyWithMultipleThreads() throws InterruptedException {
        persistManyPersonsConcurrently(15);
        awaitTerminationAndShutdownNow();
        assertEquals(10, PERSONS.size());
    }
    @Test
    void shouldPerformThePersistenceOfPersonsWithinTheGivenTimeout() {
        assertTimeout(Duration.ofSeconds(1), () -> persistManyPersonsConcurrently(20));
    }
    private List<Person> persistManyPersonsConcurrently(int numberOfPersons) {
        List<Person> personList = new ArrayList<>();
        for (int i = 0; i < numberOfPersons; i++) {
            final int indexCopy = i;
            executorService.submit(() -> personRepository.persist(new Person("Person n: " + indexCopy)));
        }
        return personList;
    }
    private void awaitTerminationAndShutdownNow() throws InterruptedException {
        boolean timeout = executorService.awaitTermination(100, TimeUnit.MILLISECONDS);
        if (timeout) {
            executorService.shutdownNow();
        }
    }
}
```

1. **@BeforeEach** and **@AfterEach** we create and shutdown an arbitrary number of threads before and after each test. This way we guarantee that every test will have the same initial setup.
2. The method persistManyPersonsConcurrently(int numberOfPersons) simulates the concurrent access to persist Persons. There is a for loop that will repeat N times the same operation and we will have up to 3 threads running simultaneously. As an analogy, think that the for loop represents a number of persons withdrawing money from the same account (shared resource) using 3 distinct ATMs (thread). 
As there are only 3 ATMs, it means that only 3 persons can be served at the same time.
3. **@Repeated(15)** Threads behave in an unpredictable way and for that reason the test must be repeated many times to make sure it is working consistently. There is no specific reason for repeating 15 times, just make sure that there are enough repetitions to verify the multi-threading behavior.
4. The test *shouldPersistPersonCorrectlyWithMultipleThreads()* has the following steps:
    * Simulate the concurrent persistence of 15 persons
    * Wait until all the threads are finished
    * Assert that we have only 10 persisted persons. That assertion is enough to guarantee the correctness of the program.

It is time to see how the failing test case behaves after few executions. (Remember, it has to fail consistently)
In the Figure 1 it is possible to prove that multi-threading is unpredictable because there are different results for the same code base. Now, it is time to work on the solution.

## The solution
It is necessary think about the correctness of the program when working with multi-threading applications. For this example, correctness means that the application should behave and give the same output as a single thread application would. In order to achieve that, each thread should access the shared resource atomically - one thread at time.
Therefore, a valid approach would be using the Lock interface:

```java
public class PersonRepository {
    private final Set<Person> persons;
    private final Lock lock;
    public PersonRepository(Set<Person> persons, Lock lock) {
        this.persons = persons;
        this.lock = lock;
    }
    void persist(Person person) {
        lock.lock();
        try {
            if (persons.size() == 10) {
                throw new IllegalStateException("The repository can not handle more than 10 persons");
            }
            persons.add(person);
        } finally {
            lock.unlock();
        }
    }
}
```

After adding the Lock object, the access to the shared resource is now synchronized and we have a success test!

## Conclusion
The objective of this post is to help people who are interested to getting started to write unit tests for multi-threading applications.

Hope you liked it. :)

The full code can be found on [github](https://github.com/ellisonalves/java-multithread-testing-ideas).