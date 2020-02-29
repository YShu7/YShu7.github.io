---
title: Message Ordering
tags: [Parallel and Distributed Algorithm]
---

### Causal Order

- If s1 **happened before** s2, and r1 and r2 are **on the same process**, then r1 must be before r2

> If on different process, then don't care

### Causal Ordering Protocol

Each process maintains n by n matrix M

- This is not the matrix clock
- M[i, j]: # of messages sent from i to j, as known by process i

If process i send a message to process j

- On process i: M[i, j]++;
- Piggyback M on the message

Upon process j receiving a message from process i with matrix T piggybacked

- Let M be the local matrix on process j

Deliver the message and set M = pairwise-max(M, T), if

- T[k, j] ≤ M[k, j] for all k ≠ i

  Implies that there is no message missing

- T[i, j] = M[i, j] + 1

  Ensure FIFO ordering of messages that are sent by process i

**Only compare receiver column**

#### Correctness Proof

s1, s2, r1, r2, where s1 happened before s2

Need to prove

- r1 is never delivered after r2

  ![image-20200229235213163](/assets/images/pd6-1.png)

  Assume Red delivered before Blue

  After delivering Red:

  M = ≥ x4 ≥ x1

  ​		≥ y4 > y1

  ​		0

  M never decreases: Blue can never be delivered any more, because the condition M[1, 3] = x1 - 1 cannot be satisfied

- both r1 and r2 will eventually be delivered

### Causal Ordering Broadcast Protocol

Each process maintains a message log for all messages delivered

- When sending a message, append the message to log and send the whole log
- Upon receiving a log, scan the log sequentially, for any message not seen before: Append the message to local log and deliver it

This method is practical because it is for broadcast messages, the overhead is smaller.

### Total Ordering of Broadcast Messages (Atomic Broadcast)

**Total ordering is only applicable to broadcast messages.**

All messages delivered to all processes in exactly the same order

Total ordering and causal ordering do not imply each other.

#### Using a coordinator for total order broadcast

A special process is assigned as the coordinator

To broadcast a message

- Send a message to the coordinator
- Coordinator assigns a sequence number to the message
- Coordinator forward the message to all processes with the sequence number
- Messages delivered according to sequence number order

##### Problem

- Coordinator is performance bottleneck
- What if coordinator fails

### Skeen's Algorithm for Total Order Broadcast

Each process maintains logical clock and a message buffer for undelivered messages

A message in the buffer is delivered / removed if

- All messages in the buffer have been assigned numbers
- This message has the smallest number

#### Claims

- All messages will be assigned message numbers

  No blocking

- All messages will be delivered

  Based on the assumption that all process **stop sending messages**

- If message A has a number smaller than B, then B is delivered after A

  ##### Proof

  - Suppose A is delivered on process 3 after B

  - Then A must have been placed in buffer after B was delivered

  > Because otherwise the max clock value will be the same, which will result in A and B having the same number, cannot guarantee that A is delivered after B

  - A must have a number larger than B

