---
title: Models and Clocks
tags: [Parallel and Distributed Algorithm]
mathjax: true
---

### Model

#### Interleaving Model of Computation

Assuming the all events are instantaneous, that no two events are simultaneous, and that a shared physical clock is available, we can totally order all the events in the system.

##### Logical clock

A mechanism to generate a total order that **could have happened** in the system

##### Distributed computation

- A global sequence of events
- All events in a run are interleaved

#### Happened-before Model

If there is no shared physical clock, then we can observe a total order among events on a single processor but only a partial order between events on different processors.

The order for events on different processors is determined on the basis of the information flow from one processor to another.

##### Vector clock

Assigns timestamps to states (and events) such that the happened-before relationship between states can be determined by using the timestamps

##### Distributed computation

The **happened-before relation** $(\rightarrow)$ is the smallest relation that satisfies

- If $e$ occured before $f$ in the same process, then $e \rightarrow f$
- If $e$ is the send event of a message and $f$ is the receive event of the same message, then $e \rightarrow f$
- If there exists an event $g$ such that $(e \rightarrow g)$ and $(g \rightarrow f)$, then $(e \rightarrow f)$

If $\neg(e \rightarrow f) \and \neg(f \rightarrow e)$, we say that $e$ and $f$ are **concurrent** ($e \parallel f$).

### Distributed System

##### Absence of a shared clock

Impossible to synchronize the clocks of different processors precisely due to uncertainty in communication delays between them

##### Absence of shared memory

Impossible for any one processor to know the global state of the system

##### Absence of accurate failure detection

Impossible to distinguish between a slow processor and a failed processor for a distributed system where there is no upper bound on message delays

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

#### The constraint for logical clocks models

- the sequential nature of execution at each process
- the physical requirement that any message transmission requires a nonzero amount of time

An **accurate** physical clock is also a logical clock

If we **extend** the logical clock with the **process number**, then we6 get a total ordering on events

### Vector Clocks

Each event has a **vector of n integers** as its vector clock

v1 = v2 if all n fields same

v1 ≤ v2 if every field in v1 is less than or equal to the corresponding field in v2

v1 < v2 if v1 ≤ v2 and v1 ≠ v2

**Relation < here is not a total order**, i.e. possible to find v1 ≮ v2 and v2 ≮ v1

- Each process i has a local vector C
- Increment C[i] at each "local computation" and "send" event
- When sending a message, vector clock value V is attached to the message. At each "receive" event, C = pairwise-max(C, V); C[i]++;

If we know the processes the vectors came from, the comparison between two states can be made in constant time: $s \rightarrow t \Leftrightarrow (s.v[s.p] ≤ t.v[s.p]) \and (s.v[t.p] < t.v[t.p])$

#### Properties

Event s happens before t <=> the vector clock value of s is smaller than the vector clock value of t

#### Drawback

It requires $O(n)$ integers to be sent with every message.

### Direct-Dependency Clocks

- Each process i has a local vector C
- Increment C[i] at each "local computation" and "send" event
- When sending a message, local component $V[sender]$ is attached to the message. At each "receive" event, 
  - $C[sender] = max(C[sender], sentValue)$
  - $C[receiver]=max(C[receiver], sentValue) + 1$

$\forall s, t: s.p≠t.p: (s \rightarrow_d t) \Leftrightarrow (s.v[s.p]≤t.v[s.p])$

### Matrix Clocks

Each event has n vector clocks, one for each process

Then i th vector on process i is called process i's principle vector

Principle vector is the same as vector clock before

Non-principle vectors are just piggybacked on messages to update "knowledge"

##### For principle vector C on process i

- Increment $C[i][i]$ at each "local computation" and "send" event
- When sending a message, all n vectors are attached to the message
- At each "receive" event, let C be the principle vector of the sender. C = pairwise-max(C, V); C[i]++;

##### For non-principle vector C on process i, suppose it corresponds to process j

- At each "receive" event, let V be the vector corresponding to process j as in the received message. C = pairwise-max(C, V)

$\forall s, t: s.p≠t.p: s \rightarrow t \equiv s.M[s.p,:]<t.M[t.p,:]$

