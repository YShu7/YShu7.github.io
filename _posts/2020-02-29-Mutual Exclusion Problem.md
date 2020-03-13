---
title: Mutual Exclusion Problem
tags: [Parallel and Distributed Algorithm]
mathjax: true
---

### Mutual Exclusion Problem

##### Context: Shared memory systems

- Multi-processor computers
- Multi-threaded programs

### Critical Section

A section of the code that needs to be executed atomically.

A common means for unrelated object to communicate with each other.

RequestCS + ReleaseCS

##### Why do we need software solutions?

Locking is in OS level, it requires the support of OS. However, smaller device may not have OS.

### Properties

#### Mutual exclusion

**No more than one** process in the critical section

#### Progress

If one or more process wants to enter and if no one is in the critical section, then **one of them can enter** the critical section

#### No starvation

If a process wants to enter, it eventually can always enter

**If no starvation, then must progress.**

### Peterson's Algorithm

![image-20200229121929154](/assets/images/pd1-1.png)

A process enters the critical section only if either it is its turn to do so or if the other process is not interested in the critical section.

Works only for 2 processes, but can be extended to N processes by repeated invocation of the entry protocol.

#### Mutual exclusion

Two processes cannot be in the critical section at the same time.

##### Proof

Assume the algorithm is not mutual exclusion.

**Case 1**: `turn = 0` when both processes are in critical section

Therefore process 0 set `turn = 1` before process 1 set `turn = 0`.

For process 1 to enter the critical section, `wantCS[0] = false`.

However, process 0 has set `wantCS[0] = true`.

**Case 2**: `turn = 1` when both processes are in critical section

Therefore process 1 set `turn = 0` before process 0 set `turn = 1`.

For process 0 to enter the critical section, `wantCS[1] = false`.

However, process 1 has set `wantCS[1] = true`.

#### Progress

If one or more processes are trying to enter the critical section and there is no process inside the critical section, then **at least one** of the processes succeeds in entering the critical section.

##### Proof

Assume the algorithm is not progress.

**Case 1**: `turn = 0` when both processes are waiting.

Therefore process 0 set `turn = 1` before process 1 set `turn = 0`.

Therefore process 0 can enter the critical section

**Case 2**: Symmetric

#### No starvation

If a process is trying to enter the critical section, then it eventually succeeds in doing so.

##### Proof

**Case 1**: If process 0 is waiting, `wantCS[1] = true && turn = 1`

Process 1 is in critical region. When it exits, it will set `wantCS[1] = false`. 

Process 0 can enter the critical section now.

**Case 2**: Symmetric

### Lamport's Bakery Algorithm

1. Get a queue number frist
2. Get served when all process with lower number haven been served

```
ReleaseCS(int myid) {
	number[myid] = 0;
}

boolean Smaller(int number1, int id1, int number2, int id2) {
	if (number1 < number2) return true;
	if (number1 == number2) {
		if (id1 < id2) return true;
		else return false;
	}
	if (number1 > number2) return false;
}

RequestCS(int myid) {
	choosing[myid] = true;
	for (int j = 0; j < n; j++)
		if (number[j] > number[myid]) number[myid] = number[j];
	number[myid]++;
	choosing[myid] = false;
	
	for (int j = 0; j < n; j++) {
		while (choosing[j] == true);
		// Need to wait until the number is stable
		while (number[j] != 0 && Smaller(number[j], j, number[myid], myid));
	}
}
```

#### Advantages

Doesn't make any assumptions on atomicity of any read or write operation. Note that the bakery algorithm does not use any vairable that can be written by more than one process.

#### Disadvantages

- It requires $O(n)$ work by each process to obtain the lock even if there is no cntention
- it requires each process to use timestamps

#### Assertion

- If a process $P_i$ is in critical section and some other process $P_k$ has already chosen its number, then $(number[i], i) < (number[k],k)$
- If a process $P_i$ is in critical section, then $(number[i]>0)$

#### Mutual Exclusion

##### Proof

Assumen `Smaller(number[i], i, number[j], j)` when process i and j are both in critical section.

For process j to be able to enter the critical section, there are two conditions to be satisfied:

1. `choosing[i] = false`

2. `number[i] = 0`

For 1 to be satisfied:

**Case 1**: process i has not started to choose

However, since process k has already got its queue number, process i will end up with getting larger queue number.

**Case 2**: process i has finished chosen

`number[i] != 0`

#### Progress

Assument process i and j wants to enter the critical section, process i has smaller queue number

**Case 1**: `choosing[j] = true`

Process j will eventually set `choosing[j] = false`

Process j will then block by process i

Process i can enter the critical section

**Case 2**: `number[j] != 0 && Smaller(number[j], j, number[myid], myid)`

This is impossible by our assumption.

#### No starvation

If process i is waiting

**Case 1**: `choosing[j] = true`

Process j will eventually set `choosing[j] = false`

**Case 2**: `number[j] != 0 && Smaller(number[j], j, number[myid], myid)`

Process j will enventually finish and set `number[j] = 0`, process i will eventually has the smallest number.

### Busy Wait Problem

- Waste CPU cycles

We want to release the CPU to other processes - Need OS support

### Hardware Solutions

#### Disabling Interrupts

In a single CPU system, a process may **disable all the interrupts** before entering the critical section. This means that the process cannot be context-switched.

On exiting the critical section, the process enables interrupts.

> Context switching occurs when the currently running thread receives a clock interrupt when its current timeslice is over.

##### Disadvantages

- **Infeasible for a multiple-CPU** system in which even if interrupts are disabled in one CPU, another CPU may execute. Disabling interrupts of all CPUs is very expensive.
- Many system facilities such as clock **registers are maintained using hardware iterrupts**. If interrupts are disabled, then these registers may not show correct values.
- Can also kead to problems if the user process has a bug such as an infinite loop inside the critical section.

#### Instructions with Higher Atomicity

- testAndSet

  Reads and returns the old value of a memory location while replacing it with a new value. 

- swap

  Swap 2 memory locations in one atomic step.