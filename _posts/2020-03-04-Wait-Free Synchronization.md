---
title: Wait-Free Synchronization
tags: [Parallel and Distributed Algorithm]
mathjax: true
---

### Wait-Free Synchronization

#### Lock Free

Synchronization mechanisms that do not use locks.

#### Wait Free

If lock-free synchronization also guarantees that each operation finishes in a bounded number of steps

#### Simplicity

The implementation of a concurrent object may use other simpler concurrent objects

- Whether it allows multiple readers or writers
- The consistency condition satisfied by the register

### Registers

Register is a kind of abstract data type: A single value that can be read and written

#### Atomic

An register is called **atomic** if the implementation always ensures **linearizability** of the history

An register is called **??** is the implementation always ensures sequential consistency of the history

#### Regular

A register is called **regular** if 

- When a read **does not overlap** with any write, the read returns the value written by **one of the most recent writes**
- When a read **overlaps with one or more writes**, the read returns the value written by **one of the most recent writes** or the value written by **one of the overlapping writes**

**Most recent writes**: the write whose response time is the latest

##### An atomic register must be regular

An atomic register always ensures linearizibility. Thus it is equivalent to some legal sequential history S, and S preserved the external order in H. This ensures that read always return the value written by one of the most recent writes or if there is overlapping, can also return the vlaue written by one of the overlapping writes.

##### Regular and sequential consistency do not imply each other

#### Safe

A register is called **safe** if 

- When a read **does not overlap** with any write, then it returns the value written by **one of the most recent writes**

- When a read **overlaps with one or more writes**, it can return **anything**

##### A regular register must be safe

##### Safe and sequential consistency do not imply each other