---
title: Global Snapshot
tags: [Parallel and Distributed Algorithm]
mathjax: true
---

### Global Snapshot

##### Definition

An algorithm that captures a global state is called a global snapshot algorithm

##### Goal

Take a snapshot of the global computation

##### Difficulties

No process has access to the global state of the system

##### Time-Based Model

A snapshot of local states on n processes taken at exactly the same time

##### Happened-Before Model

A snapshot of local states that are all concurrent with each other, i.e. no two states have a happened-before relationship with each other

##### The global state in the time-based model is also a global state in the happened-before model

##### We choose to use the definition for the global state from the happened-before model

- Impossible to determine whether a given global state occurs in the time-based model without access to perfectly synchronized local clocks
- Program properties that are of interest are often more simply stated in the happened-before model than in the time-based model

### Consistent snapshot

A snapshot of local states on n processes such that the global snapshot **could have happended sometime in the past**

#### Global snapshot

A set of events such that if e2 is in the set and e1 is before e2 in **process order**, then e1 must be in the set

#### Consistent global snapshot

A global snapshot such that if e2 is in the set and e1 is before e2 in **send-receive order**, then e1 must be in the set

### Existence of Consistent Global Snapshot

Consider the ordered events e1, e2, ... on any given process

Theorem: For any positive integer m, there exists a consistent global snapshot S such that $e_i \in S$ for $i≤m$, $e_i \notin S$ for $i>m$

### Capturing a Consistent Global Snapshot

#### Communication Model

- No message loss
- Communication channels are unidirectional
- FIFO delivery on each channel

##### Ensuring FIFO:

- Each process maintains a message number counter for each channel and stamps each message sent
- Receiver will only deliver messages in order

#### Chandy&Lamport's Protocol: Taking local snapshots

##### Pre-condition

Channels are FIFO is essential to the correctness of the algorithm as explained later.

##### Difficulties

- We need to ensure that the recorded local states are mutually concurrent
- We also need a mechanism to capture the state of the channels

##### Each process is either

- Red (it has taken the local snapshot), OR
- White (it has not taken the local snapshot)

Protocol initiated by a single process by turning itself from White to Red

Once a process turns to Red, it immediately **send out Marker messages to all other processes**

Upon receiving Marker, a process turns Red

Next, we would like also to capture messages that are "on-the-fly" when the snapshot is taken

##### Overhead

Requires that a marker be sent along all channels, thus it has an overhead of $e$ messages, where $e$ is the number of unidirectional channels in the system.

![image-20200229220715317](/assets/images/pd5-1.png)

