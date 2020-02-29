---
title: Consistency Conditions
tags: [Parallel and Distributed Algorithm]
---

### Consistency

##### Definition

Consistency specifies **what behavior is allowed** when a shared object is accesses by multiple processes

##### Specification

- No right or wrong

- Be sufficiently strong - otherwise the shared object cannot be used in a program
- Can be implemented (efficiently)

##### Why do we care about consistency?

Mutual exclusion is actually a way to ensure consistency (based on some consistency definition)

### Operation

A single **invocation/response pair** of a single method of a single shared object by a process

Two **invocation** events are the same if **invoker, invokee, parameters** are the same - inv(p, read, X)

Two **response** events are the same if **invoker, invokee, response** arethe same - resp(p, read, X, 1)

### History

A history H is a sequence of invocations and responses ordered by **wall clock time**

For any invocation in H, the corresponding response is required to be in H

Each execution of a parallel system corresponds to a history, and vice versa

### Sequential History and Concurrent History

#### Sequential History

##### A history H is sequential if

Any invocation is always **immediately followed by** its response

i.e. No interleaving

#### Concurrent History

Otherwise called **concurrent**

### Sequential Legal History and Subhistory

#### Sequential Legal History

##### A sequential history H is legal if

All responses satisfies the sequential semantics of the data type

**Sequential semantics**: The semantics you would get if there is only one process accessing that data type.

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

A history H is sequentially consistent if it is equivalent to some **legal sequential history** S that **preserves process order**

### Linearizability

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

### Local Property

Linearizability is a local property in the sense that H is linearizable iff for any **object** x, H|x is linearizable

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

### Registers

Register is a kind of abstract data type: A single value that can be read and written

An register is called **atomic** if the implementation always ensures linearizability of the history

An register is called **??** is the implementation always ensures sequential consistency of the history

#### Regular

A register is called **regular** if 

- When a read **does not overlap** with any write, the read returns the value written by **one of the most recent writes**
- When a read **overlaps with one or more writes**, the read returns the value written by **one of the most recent writes** or the value written by **one of the overlapping writes**

**Most recent writes**: the write whose response time is the latest

#### An atomic register must be regular

An atomic register always ensures linearizibility. Thus it is equivalent to some legal sequential history S, and S preserved the external order in H. This ensures that read always return the value written by one of the most recent writes or if there is overlapping, can also return the vlaue written by one of the overlapping writes.

##### Regular and sequential consistency do not imply each other

#### Safe

A register is called **safe** if 

- When a read **does not overlap** with any write, then it returns the value written by **one of the most recent writes**

- When a read **overlaps with one or more writes**, it can return **anything**

##### A regular register must be safe

##### Safe and sequential consistency do not imply each other