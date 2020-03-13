---
title: Consistency Conditions
tags: [Parallel and Distributed Algorithm]
mathjax: true
---

### Consistency

##### Correctness conditions of executions

Which behaviors are correct when **multiple processes** invoke methods **concurrently** on a **shared object**

##### Current object

Allows **multiple processes** to execute its operations **concurrently**

##### Consistency

Consistency specifies **what behavior is allowed** when a shared object is accesses by multiple processes

##### Specification

- No right or wrong

- Be sufficiently strong - otherwise the shared object cannot be used in a program
- Can be implemented (efficiently)

##### Why do we care about consistency?

Mutual exclusion is actually a way to ensure consistency (based on some consistency definition)

The notion of consistency is also required when objects are replicated in a parallel or a distributed system

> There are two reasons for replicating objects:
>
> - Fault tolerance
>
>   If an object has multiple copies and a processor that contains one of the copies of the object goes down, the system may still be able to function correctly by using other copies.
>
> - Efficiency
>
>   Accessing a remote object may incur a large overhead because of communication delay. Suppose that we know that most accesses of the object are for read only. In this case, it may be better to replicate that object.

### Operation

A single **invocation/response pair** of a single method of a single shared object by a process

Two **invocation** events are the same if **invoker, invokee, parameters** are the same - inv(p, read, X)

Two **response** events are the same if **invoker, invokee, response** are the same - resp(p, read, X, 1)

### History

A history H is a sequence of invocations and responses ordered by **wall clock time**

For any invocation in H, the corresponding response is required to be in H

Each execution of a parallel system corresponds to a history, and vice versa

##### Definition

A history is an execution of a concurrent system modeled by a directed acyclic graph $(H, <_H)$, where $H$ is the set of operations and $<_H$ is an irreflexive transitive relation that captures the occurred before relation between operations.

$e<_Hf$ if $resp(e)$ occurred before $inv(f)$ in real time.

##### Equivalency

Composed of exactly the same set of invocation and response events.

### Sequential History and Concurrent History

#### Sequential History

##### A history H is sequential if

Any invocation is always **immediately followed by** its response

$<_H$ is a total order

i.e. No interleaving

Such a history would happen if there was only one sequential process in the system.

#### Concurrent History

Otherwise called **concurrent**

### Sequential Legal History and Subhistory

#### Sequential Legal History

##### A sequential history H is legal if

All responses satisfies the sequential semantics of the data type

**Sequential semantics**: The semantics you would get if there is only one process accessing that data type.

It meets the sequential specification of all the objects

#### Subhistory

##### Process p's process subhistory of H (H|p):

The subsequence of all events of p

Thus a process subhistory is always sequential (because one process can only have one invocation before response, it needs to wait for the response before it can proceed)

##### Object o's object subhistory of H (H|o):

The subsequence of all events of o

### Equivalency and Process Order

#### Equivalency

Two histories are equivalent if they have **exactly the same set of events**

Same events imply **all responses are the same**

Ordering of the events may be different

User only cares about responses

#### Process/Execution Order

Process order is a **partial order among all events**

For any two events of the **same process**, process order is the same as execution order

No other additional orderings

### Sequential Consistency

1. The operations of all the processors were executed in some sequential order
2. The operations of each individual processor appear in this sequence in the order specified by the program

A history H is sequentially consistent if it is equivalent to some **legal sequential history** S that **preserves process order**.

### Linearizability

A partial history H is linearizable if there exists a way of completing the history by appending response events such that the complete history is linearizable.

#### External order

A history H uniquely induces the following "<" partial order among operations

o1 < o2 iff response of o1 appears in H before invocation of o2.

**External order is a super set of process order**

#### Linearizability

The execution is equivalent to some execution such that each operation happens instantaneously at some point (called linearization point) between invocation and response

##### A history H is linearizable if

1. It is equivalent to some legal sequential history S, and
2. S preserved the external order in H (process order for sequential consistency)

#### Linearizability implies sequential consistency

Linearizability is arguable the **strongest form** of consistency people have ever defined.

#### Local Property

**Linearizability is a local property** in the sense that H is linearizable iff for all **object** x, H|x is linearizable

Sequential consistency is not a local property

**Proof**: H is linearizable if for any object x, H|x is linearizable

Construct a directed graph among all operations as following: A directed edge is created from o1 to o2 if

- o1 and o2 is on the same obj x and o1 is before o2 when linearizing H|x

  OR

- o1 < o2 in external order

> **Is it possible to have o1 -> o2 and o2 -> o1?**
>
> No. Because the linearization of a sequence is fixed and must preserve the external order.

Lemma: The resulting directed graph is **acyclic**. Any topological sorting of the above graph gives us a legal sequential history S, and the edges in the graph is a subset of the total order induced by S

H|x = o1 o2 is legal for x

H|y = o3 o4 is legal for y

S = o1 o3 o2 o4 is legal because o2's behavior will not change due to some value read in o3. The operations from different objects do not influence each other.

### Casual Consistency

Weaker than sequential consistency.

Allows for cheap read and write operations

##### A history H is causally consistent if

- for each process $P_i$, there is a legal sequential history $(S_i<s_i)$ where $S_i$ is the set of all operations of $P_i$ and all write operations in $H$, and $<s_i$ respects the following order
- Process order: If $P_i$ performs operation $e$ before $f$, then $e$ is ordered before $f$ in $S_i$
- Object order: If any process $P$ performs a write on an object $x$ with value $v$ and another process $Q$ reads that value $v$, then the write by $P$ is ordered before read by $Q$ in $S_i$

#### Sequential consistency implies causal consistency

We require only that there is a legal sequential history for every process and not one for the entire system