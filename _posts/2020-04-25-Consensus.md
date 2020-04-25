---
title: Consensus
tags: [Parallel and Distributed Algorithm]
mathjax: true
---

#### Goal

- **Termination**: All nodes (that have not failed) **eventually decide**
- **Agreement**: All nodes that decide should decide on **the same value**
- **Validity**: If all nodes have the same initial input, that value should be the only possible decision value. Otherwise nodes are allowed to decide on anything (except that they still need to satisfy the Agreement requirement)

### Consensus with Node Crash Failures/Synchronous

##### Failure model

- Nodes may experience **crash failure**
- Communication channels are **reliable**

##### Timing model

- Synchronous: Message delay has a **known upper bound** x, and node processing delay has a **known upper bound** y

- Implies the possibility of **accurate failure detection**

#### Inter-locked rounds

Where in each round,

- Every process **sends one message** to every other process

- Every process **receives one message** from every other process

- Every process does some local computation

Dummy messages can be used.

Messages sent during a round is received during that round

Accurate failure detection

##### via Clocks with Bounded Error

- Each process first performs a send (taking time no more that $t_1$).
- Message propagation and receiving the message no more than $t_2$.
- Local processing no more than $t_3$.

**Bounded error**: Consider an example where the clock on a certain process is twice as fast as an accurate clock.

###### To compensate

Everyone sets the timer duration to be $2(t_1+t_2+t_3)$

###### To start the next round at the same time

- If a process receives a round 2 message while in round 1, it immediately start round 2

- Everyone sets the timer duration to be $2((t_1+t_2+t_3) +t_1+t_2)$, since the round starting time at different processes may differ by $t_1+t_2$.

#### Protocol

![image-20200425191717297](/assets/images/pd8-1.png)

Need $f+1$ rounds to tolerate $f$ failures. $f$ needs to be input.

##### Proof

**nonfaulty**: A node is nonfaulty if it has not crashed by the end of round $r$

**good**: A round is good if there is no node failure during that round

With $f+1$ round and $f$ failures, there must be **at least on good round**.

At the end of any good round $r$, all non-faulty nodes during round $r$ have the same $S$

Suppose $r$ is a good round, and consider any round $t$ after round $r$. During round $t$, the value of $S$ on any non-faulty nodes does not change.

All nonfaulty processes at round $f+1$ will have the same $S$

### Consensus with Link Failures (Coordinated Attack Problem)

##### Failure model

- Nodes do not fail
- Communication channels may fail - may drop **arbitrary (unbounded)** number of messages

##### Timing model

- Synchronous: Message delay has a **known upper bound** x, and node processing delay has a **known upper bound** y

It is **impossible** to reach the above goal using a **deterministic** algorithm, since the communication channel can **drop all messages**.

![image-20200425193554451](/assets/images/pd8-2.png)

#### Weakened Validity

Assuming input either 0 or 1,

- If all nodes start with 0, decision should be 0

- If all nodes start with 1 and **no message is lost** throughout the execution, decision should be 1

- Otherwise nodes are allowed to decide on anything

It is still **impossible** to reach the above weakened goal using a **deterministic** algorithm

![image-20200425195404295](/assets/images/pd8-3.png)

Can remove the last message until all messages are dropped, while  decision is still 1.

#### Allowing Limited Disagreement

##### Goal

- **Termination**: All nodes eventually decide

- **Agreement**: All nodes decide on the same value with probability of $(1 – error_prob)$

- **Validity**: 

  - If all nodes start with 0, decision should be 0
  - If all nodes start with 1 and no message is lost throughout the execution, decision should be 1

  - Otherwise nodes are allowed to decide on anything

##### A Randomized Algorithm

- Algorithm has a predetermined number ($r$) of rounds

- P1 picks a random integer $bar \in [1...r]$ at the beginning

- The protocol allows P1 and P2 to each maintain a level variable (L1 and L2), such that
  - Level is influenced by the adversary
  - L1 and L2 can differ by at most 1

- bar, input and current level attached on all messages

- Upon P2 receiving a msg with L1 attached: L2 = L1 + 1, L1 maintained similarly

**Lemma**: L1 and L2 never decreases in any round, and at the end of any round, L1 and L2 differ by at most 1.

![image-20200425200440430](/assets/images/pd8-4.png)

###### Decision Rule

- At the end of the r rounds, P1 decides on 1 iff

  - P1 knows that P1’s input and P2’s input are both 1, and
  - L1 ≥ bar

  i.e. P1 will decide on 0 if it does not see P2's input

- At the end of the r rounds, P2 decides on 1 iff

  - P2 knows that P1’s input and P2’s input are both 1, and
  - P2 knows bar, and
  - L2 ≥ bar

  i.e. P2 will decide on 0 if it does not see P1's input **OR** it does not see bar

###### When does error occur?

**Case 1**: P1 see P2's input, but P2 does not see P1's input or bar.

L1 = 1, L2 = 0. Error occurs when bar = 1.

**Case 2**: P2 see P1's input and bar, but P1 does not see P2's input.

L1 = 0, L2 = 1. Error occurs when bar = 1.

**Case 3**: P1 see P2's input, and P2 see P1's input and bar. 

Lmax = max(L1, L2). Error occurs when bar = Lmax.

###### Correctness Proof

**Termination**: $r$ rounds

**Validity**:

- If all nodes start with 0, decision should be 0
- If all nodes start with 1 and no message is lost throughout the execution, decision should be 1
  - If no messages are lost, then L1=L2=r $\rightarrow$ P1 and P2 will decide on 1
- Otherwise nodes are allowed to decide on anything

**Aggreement**: With $1-{1\over r}$ probability