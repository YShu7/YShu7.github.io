---
title: Leader Election
tags: [Parallel and Distributed Algorithm]
mathjax: true
---

### Leader Election on Anonymous Ring

**Anonymous Ring**: No unique identifiers

For low-end devices that do not have unique identifiers.

Impossible using deterministic algorithms.

- Initial state, algorithm on each node, each step are the same $\rightarrow$ Final state is the same

### Leader Election on Ring

Each node has a unique identifier

Nodes only send messages **clockwise**

Each node acts on its own

#### Chang-Roberts Algorithm

- A node send election message with its own id clockwise
- Election message is forwarded **if id in message larger than own message**
- Otherwise message discarded

- A node becomes leader if it sees its own election message (i.e. with **largest id**)

##### Performance

Communication is the bottleneck, therefore performance is often described as message complexity

**Best case**: $2n-1 = 1\times (n-1) +n$ messages

Clockwise increasing

**Worst case**: $n(n+1)/2 = 1+2+...+n$ messages

Clockwise decreasing

**Average**: $O(n\log(n))$

$\mathbb{E}[\sum x_k] = \sum \mathbb{E}[x_k]$

$\mathbb{E}[x_k]=\sum_{i=1}^k(i \cdot \Pr[x_k=i])$

Let $p={n-k \over n-1}$

$\Pr[x_k=1]={n-k \over n-1} ≥ p$: $x_k$ is the number of messages caused by node $k$. Total $n-1$ nodes other than node $k$, out of whick $n-k$ nodes have ids larger than $k$.

$\Pr[x_k=2 | x_k>1]={n-k \over n-2} ≥ p$: Total $n-2$ nodes left, out of which $n-k$ nodes are larger than $k$.

$\mathbb{E}[x_k] ≤ {1 \over p} = {n-1 \over n-k}$

$\sum_{k=1}^n \mathbb{E}[x_k]=n+\sum_{k=1}^{n-1} \mathbb{E}[x_k]$ 

> Because the largest node will always cause $n$ message, there is no node larger than it.

$<n+\sum_{k=1}^{n-1}{n-1\over n-k}$

$=n+(n-1)\sum_{k=1}^{n-1}{1 \over k}$

$= n + (n-1)O(\log n) = O(n\log n)$

### Leader Election on General Graph (n known)

#### Complete graph

- Each node send its id to **all other nodes**

- Wait until you receive $n$ ids
- Biggest id wins

$n$ must be known

#### Any connected graph

- **Flood** your id to all other nodes

  Send id to neighbours, neighbours then forward ids to their neighbours until the ids reach every node in the system

- Wati until you receive $n$ ids
- Biggest id wins

Can use protocol to calculate the number of nodes - spanning tree

##### Spanning Tree Construction

**Goal**: Each node knows its parent and children

Send child request to all its neighbours, if receive 2 child requests,  'say yes' to only one.

