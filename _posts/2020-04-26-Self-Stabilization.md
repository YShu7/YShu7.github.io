---
title: Self-Stabilization
tags: [Parallel and Distributed Algorithm]
mathjax: true
---

A distributed algorithm is self-stabilizing if

- Starting from any (legal or illegal) state, the protocol will **eventually reach a legal state** if there are **no more faults**
- Once the system is in a legal state, it will **only transit to** other **legal** states **unless there are faults**

### The Rotating Privilege Problem

A ring of $n$ processes, each process can only communicate with neighbors.

There is a privilege in the system,

- At any time, **only one** node may have the privilege
- The privilege needs to “rotate” among the nodes so that **each node has a chance**

#### The Rotating Privilege Algorithm 

Each process $i$ has a local integer variable $V_i$, $0 ≤ V_i < k$ where $k ≥ n$

Always retrieve value $L$ of the **clockwise** neighbor.

##### For bottom process

```
if (L == V) then V = (V + 1) % k
```

##### For normal process

```
if (L != V) then V = L
```

### Legal States

System in legal state if exactly one process has the privilege and changes its value

**Lemma**: The following are legal states and are the only legal state

**Case A**: All $n$ values are the same

**Case B**: **Only two different values** forming two consecutive bands, and one band starts from the bottom process

#### Proof

Consider the value $V$ of the bottom process and the value $L$ of its clockwise neighbor

##### Case 1: V = L

Bottom process can make a move. 

No other process should be able to make a move. 

Case A

##### Case 2: V != L

Starting from the bottom process, find counter-clockwise the first normal process whose value is different from its clockwise neighbor. Such normal process must exist since $V != L$. 

This normal process can make a move. 

No other process should be able to make a move.

Case B

### Legal States => Legal States

The only action that can change the system state happens when:

- Bottom process: $V = L$, $V=(V+1) \mod k$

  Case A $\rightarrow$ Case B

- Normal process: $V!=L$

  Case B $\rightarrow$ Case A or B

#### Illegal States => Legal States

**Lemma 1**: Let P be a normal process, and let Q be P’s clockwise neighbor. If Q makes $i$ moves, then P can make at most $i+1$ move.

**Lemma 2**: Let Q be the bottom process. If Q makes $i$ moves, then system-wide there can be at most $i+(i+1)+(i+2)+...+(i+n-1)=ni+{(n-1)n \over 2}$ moves:

**Lemma 3**: Consider a sequence of $(n^2-n)/2+1$ moves in the system. The bottom process makes at least one move in the sequence.

**Lemma 4**: In any system state (regardless of legal or not), there is always at least one process that can make a move.

**Lemma 5**: Regardless of the starting state, the system eventually reach a state T where the bottom process has a **different value from** the values of **any other process**.

**Proof**: Let Q be the bottom process. If in the starting state Q has the same value as some other process, then there must be an integer $j (0 ≤ j ≤ k-1)$ that is not the value of any process.

- In any state, at least one process can make move (**Lemma 4**)
- Eventually, the number of moves will approach infinity
- Q moves once among every consecutive $(n^2-n)/2+1$ moves of the system (**Lemma 3**)
- Q will make infinite number of moves
- Q will eventually take $j$ as its value.

**Lemma 6**: If the system is in a state where the bottom process has a different value from all other process, then the system will eventually reach another state where **all processes have the same value**.

**Proof**: Let the bottom process be $P_1$, and let $P_1$’s value be $x$. Starting from the bottom process and counter-clockwise along the ring, let the blue processes be $P_2$, $P_3$, ..., $P_n$.

- Let $t$ be such that $P_1$ through $P_t$ all have the same value $x$, and $P_{t+1}$ through $P_n$ all have values different from $x$. 
- Initially $t=1$. We will prove that for any $t = s$, $t$ will become $s+1$ at some point of time. Hence eventually $t$ will be $n$, and we are done.
- Note that $P_{s+1}$ will take the value of $x$ once it takes an action. Denote this event as $E$. 
- Before event $E$, $P_{s+2}$ through $P_n$ can never take the value of x. 
- Also, since $P_n$ can never take the value of $x$ before event $E$, $P_1$ through $P_s$ can never change their value before event $E$. Hence, immediately after event $E$ happens, $t$ becomes $s+1$.

### Self Stabilizing Spanning Tree Algorithm

Each process maintains two variables:

- **parent**
- **dist**: distance to root

##### Root

dist = 0, parent = null

##### Others

- Request dist from all neighbors
- Set dist = 1 + min(dist from neighbors)

- Set parent = neighbor providing min. dist

#### Correctness Proof

##### Phase

The minimum time period where each process has executed its code at least once

##### Level

The length of the shortest path from process to the root

###### Properties

- A node at level X has at least one neighbor in level X-1
- A node at level X can only have neighbors in level X-1, X, and X+1.

**Lemma**: At the end of phase 1, $dist_1 = 0$ and $dist_i ≥ 1$ for any $i ≥ 2$

**Lemma**: At the end of phase $r$,

- Any process $i$ whose $A_i ≤ r-1$, $dist_i = A_i$

- Any process $i$ whose $A_i ≥ r$, $dist_i ≥ r$

**Proof**: Assume the lemma holds at phase $r$, now consider phase $r+1$.

- A process may take **multiple actions** in a phase.
- Processes may **take actions in parallel**.

Typical proof technique for proving self-stabilization:

- Step 1: Prove that the $t$ actions will not roll back what is already achieved so far by phase $r$ (**no backward move**)
- Step 2: Prove that at some point, **each** node will achieve more (**forward move**)
- Step 3: Prove that the $t$ actions will not roll back the effects of the forward move after the forward move happens (**no backward move after the forward move**)

![image-20200425225905388](/Users/ysq/Desktop/YShu7.github.io/assets/images/pd10-1.png)

**Claim 1**: The blue conditions hold ***throughout*** phase $r+1$.

**Proof**: Induction. Assume the statement for $t$ and consider action $(t+1)$ by some node $i$. 

> Cannot assume that action $(t+1)$ happends after the $t$ actions

- For $A_i ≤ r-1$, node $i$ has at least one neighbor $j$ at level $A_j = A_i-1$. 
- By the blue condition and inductive hypothesis, its $dist_j = A_i-1$. 
- Node $i$ only has neighbors at level $A_i-1, A_i, A_i+1$.
- By the blue condition and inductive hypothesis, their $dist$ will not $< A_i-1$.

- Hence node $i$ will set its $dist_i = A_i$

**Claim 2**: Each node $i$ will satisfy the red conditions **at some point** of time during phase $r+1$.

**Proof**: Node $i$ takes at least one action in phase $r+1$.

- For $A_i = r$, node $i$ has at least one neighbor $j$ at level $A_j = A_i-1 = r-1 ≤ r-1$. 
- By the blue condition, $dist_j = A_j = A_i - 1$
- Node $i$ only has neighbors at level $A_i-1, A_i, A_i+1$.
- By the blue condition, their $dist$ will not $< A_i-1$.
- Hence node $i$ will set its $dist_i = A_i$

**Claim 3**: For each node after it first satisfies the red condition, it will continue to satisfy the red condition for the remainder of phase $r+1$.

##### Conclusion

After $H+1$ phases, $dist_i = A_i$ on all processes, where $H$ is the length of the shortest path from the most far away process to the root.

After $H+1$ phases, the $dist$ and $parent$ values on all processes are correct.

- Each process has a single parent pointer except the root. So the graph has $n$ nodes and $n-1$ edges. Each process has a path to the root, thus the graph is connected. Hence it is a spanning tree.