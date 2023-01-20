---
layout: post
title: title
date: 2023-01-01 22:01
tags: []
categories: []
mermaid: true
---

# Introduction
Before talking about kafka it is important to first understand what is event streaming and what kind of problem it solves.
So let's start with an overview of Event Streaming and than we can quickly jump into Kafka.

# What is event streaming?
In short topics we could defined it as:
* It is the digital equivalent of the human body's nervous central system.
* Foundation of the 'always-on' world where business are increasingly software-defined and automated, and where the users of a software are more softwares.
* It is the practice of capturing data in real-time from:
    * Databases
    * Sensors
    * Mobile devices
    * Cloud services
    * Software apps in the form of streams of events

The data captured is stored for later retrieval. Some examples ares:
* Manipulating
* Processing
* Reacting to the event streams in real-time as well as respectively

Event streaming thus ensures a continuos flow and interpretation of data. The right information as at the right place, at the right time

## What are the use cases for event streaming?
1. Process payments and financial transactions in real time (Stocks, Banks, Insurance)
2. Tracking vehicles in real-times (Logitics and automotive industry)

# Kafka is a Streaming Platform. What does it means?
It combines 3 capabilities:
* Publish and subscribe to streams of events
* Store streams of events durably and reliably for as long as you want
* Process streams of events as they occur or retrospectively.

These capabilities are provided in a distributed, highly scalable, elastic, fault tolerant, and secure manner. Kafka can be deployed on bare metal hardware, VM, containers, on-premisses, cloud.

It is possible to manage or use a fully managed service

# How does it works?
* Consists of servers and clients
* Communication via high-performance TCP network protocol

## Servers
Run as a cluster of one or more servers that can span multiple data centers or cloud regions

**PUT SOME IMAGE HERE**

## Clients
* Allow the communication with kafka server
* Allow writing microservices that read, write, and process streams of events in parallel, at scale, and in a fault tolerant manner even in case of network problem or machine failures
* Can be used with many different programming languages such as Java, Scala, Go, Python, etc.

## Main concepts and terminology
There are some concepts and terminology to be aware of:

### Event
* Records of the fact that *something has happened*
* Form of writing data to kafka
* An event looks like very much to a key-value structure such as JSON. For example, an event called *Transaction Processed* could look like as:

```json
{
    "trasaction_processed" : {
        "key": "Alice",
        "value": "Made a payment of $200 to Bob",
        "timestamp": "Jun 25, 2022 at 2:06pm"
    }
}
```

### Producers
* Client application that publish events to Kafka. For example, an application that is responsible for processing payments and when its done doing its job it publishes the event "*transaction_processed*" to Kafka. Then Kafka will let other applications know that this event happened.
* Producers are completely agnostic of the existence of Consumers
* Scalability capabilities

### Consumers
* Client applications that subscribe to kafka topics in order to be notified when a given event happened and continue their work.
* Consumers are completely agnostic of the existence of Producers
* Scalability capabilities

### Topics
* Organizes the events inside of it
* Durable
* Analog to a files system
* Identifiable by name (e.g. payments_topic)
* Multi-producer and multi-subscriber: can have zero, one or many producers that unites events to it
* Events are not deleted after consumption
* You define for how long kafka should retain your events through a per-topic configuration setting
* Kafka's performance is effectively constant respect to data size
* Storing data for a long time is fine because kafka has performance constant
* Partioned
  * Means the topic is spread over a number of "buckets" located on different kafka brokers
  * Important for scalability
  * Allows client applications to both read and write the data from to many brokers at the same time
  * When a new event is published to a topic, it is actually appended to one of topic's partitions
  * Events with the same key are written to the same partition
  * Kafka guarantees that any consumer of a given topic or partition will always read that partition's events in exactly the same order as they were written

<< IMAGE of STORAGE  = BROKER>>

In the figure:
* Topic has 4 partitions
* Two producer clients (Independent from each other)
* Events with the same key are written to the same partition
* Both producers can write to the same partition
* Every topic can be replicated (fault toletrant and highly available). The replication can be:
  * Accross geo-regions
  * Data centers
  * Multiple Brokers have copy of the data
  * Replication is a factor of 3. There will always be three copies of your data


### Broker
* Receives messages from producers and stores them on disk keyed by unique offset
* Allows consumers to fetch messages by topic, partition and offset
* Can create a kafka cluster by sharing information between each other directly or indirectly using Zookeeper
* A Kafka cluster has exactly one broker that acts as the controller

