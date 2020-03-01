---
title: Uninformed Search
tags: [Artificial Intelligence]
mathjax: true
---

Fully observable, deterministic, discrete environment.

### Problem Formulation

- Goal Test

- Path Cost

### Tree Search

Start at the source node, keep searching until we reach a goal state.

**Frontier**: Nodes that we have seen but haven't explored yet

At each iteration, choose a node from the frontier, explore it, and add its neighbors to the frontier.

### Graph Search

A node that's been explored once will not be revisited.

Implementation

### Implementation: States vs. Nodes

##### State

Represents a **physical configuration**.

##### Node

A **data structure** constituting part of search tree.

It includes **state, parent node, action, and path cost g(n)**.

Two different nodes are allowed to contain same world state.

### Search Strategies

Any deterministic search algorithm is determined by the order of node expansion.

#### Evaluation Criteria

##### Completeness

Always find a solution if exists

##### Optimality

Find a least-cost solution

##### Time complexity

Number of nodes generated

##### Space complexity

Max number of nodes in memory

#### Problem Parameters

##### b

maximum # of successors of any node (may be $\infin$)

##### d

depth of **shallowest** (may not be optimal) goal node

##### m

maximum depth of search tree (may be $\infin$)

### Breadth-First Search

##### Idea

Expand shallowest unexpanded node

##### Implementation

Frontier is a FIFO queue, i.e., insert new successors at the end

| Complete | Yes (if b is finite)                    |
| -------- | --------------------------------------- |
| Optimal  | No (unless step cost = 1)               |
| Time     | $O(b) + O(b^2) + ... + O(b^d) = O(b^d)$ |
| Space    | Max size of frontier $O(b^d)$           |

**Space is the bigger problem (more than time)**

### Uniform-Cost Search

##### Idea

Expand least-path-cost unexpanded node

##### Frontier

Priority queue ordered by path cost $g$

**Equivalent to BFS if all step costs are equal**

| Complete | Yes (if all step costs are ≥ $\epsilon$)                     |
| -------- | ------------------------------------------------------------ |
| Optimal  | Yes (shortest path nodes expanded first)                     |
| Time     | $O(b^{1+\left \lfloor{C^*\over \epsilon}\right \rfloor})$ where $C^*$ is the optimal cost |
| Space    | $O(b^{1+\left \lfloor{C^*\over \epsilon}\right \rfloor})$    |

At every round we get at least a distance of $\epsilon$ closer to the goal

Reach nodes at distance $0$, $\epsilon$, $2\epsilon$, ..., $\left \lfloor{C^*\over \epsilon}\right \rfloor\epsilon$ of goal; total $\left\lfloor{C^*\over\epsilon}\right\rfloor+1$ steps

At step $k$ (depth $k$ at most): keep ≤ $b^k$ nodes in frontier

### Depth-First Search

##### Idea

Expand deepest unexpanded node

##### Implementation

Frontier = LIFO stack, i.e. insert successors at the front

| Complete | No on infinite depth graphs                                  |
| -------- | ------------------------------------------------------------ |
| Optimal  | No                                                           |
| Time     | $O(b^m)$, $m$ is last level, may not be the shallowest goal level |
| Space    | $O(bm)$ (can be $O(m)$)                                      |

When checking a node $v$, we push at most $b$ descendants to stack.

We do so at most $m$ times => $O(bm)$ space.

We do not really need to push all $b$ descendants to the stack, can only push one.

### Depth-Limited Search

##### Idea

Run DFS with depth limit $l$, i.e., do not search at depth greater thant $l$.

Same guarantees as DFS, with $l$ instead of $m$

### Iterative Deepening Search

##### Idea

Perform DLSs with increasing depth limit until goal node is found

**Better if state space is large and depth of solution is unknown**

| Complete | Yes (if b is finite)       |
| -------- | -------------------------- |
| Optimal  | No (unless step cost is 1) |
| Time     | $O(b^d)$                   |
| Space    | $O(bd)$ (can be $O(d)$)    |

