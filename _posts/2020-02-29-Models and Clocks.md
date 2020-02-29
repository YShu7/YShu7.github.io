---
title: Models and Clocks
tags: [Parallel and Distributed Algorithm]
---

### Software Clocks

Incur much lower overhead than maintaining (sufficiently accurate) physical clocks

#### Goal

- Capture **event ordering** that are visible to users who do not have physical clocks

- Capture happened-before relation

#### Happened-Before Relation

A partial order among events

Process order, send-receive order, transitivity

### Logical Clocks

Each event has a **single integer** as its logical clock

- Each process has a local counter C
- Increment C at each "local computation" and "send" event
- When sending a message, logical clock value V is attached to the message. At each "receive" event, C = max(C, V) + 1

#### Properties

Event s happens before t => the logical clock value of s is smaller than the logical clock value of t

The reverse may not be true

### Vector Clocks

Each event has a **vector of n integers** as its vector clock

v1 = v2 if all n fields same

v1 ≤ v2 if every field in v1 is less than or equal to the corresponding field in v2

v1 < v2 if v1 ≤ v2 and v1 ≠ v2

**Relation < here is not a total order**, i.e. possible to find v1 ≮ v2 and v2 ≮ v1

- Each process i has a local vector C
- Increment C[i] at each "local computation" and "send" event
- When sending a message, vector clock value V is attached to the message. At each "receive" event, C = pairwise-max(C, V); C[i]++;

#### Properties

Event s happens before t <=> the vector clock value of s is smaller than the vector clock value of t

### Matrix Clocks

Each event has n vector clocks, one for each process

Then i th vector on process i is called process i's principle vector

Principle vector is the same as vector clock before

Non-principle vectors are just piggybacked on messages to update "knowledge"

##### For principle vector C on process i

- Increment C[i] at each "local computation" and "send" event
- When sending a message, all n vectors are attached to the message
- At each "receive" event, let C be the principle vector of the sender. C = pairwise-max(C, V); C[i]++;

##### For non-principle vector C on process i, suppose it corresponds to process j

- At each "receive" event, let V be the vector corresponding to process j as in the received message. C = pairwise-max(C, V)

