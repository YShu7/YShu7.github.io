---
title: Global Snapshot
tags: [Parallel and Distributed Algorithm]
---

Goal: Take a snapshot of the global computation

A snapshot of local states on n processes taken at exactly the same time

A snapshot of local states on n processes such that the global snapshot **could have happened** sometime in the past

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

Each process is either

- Red (it has taken the local snapshot), OR
- White (it has not taken the local snapshot)

Protocol initiated by a single process by turning itself from White to Red

Once a process turns to Red, it immediately **send out Marker messages to all other processes**

Upon receiving Marker, a process turns Red

Next, we would like also to capture messages that are "on-the-fly" when the snapshot is taken

![image-20200229220715317](/assets/images/pd5-1.png)

