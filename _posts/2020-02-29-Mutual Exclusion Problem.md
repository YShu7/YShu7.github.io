---
title: Mutual Exclusion Problem
tags: Parallel and Distributed Algorithm
---

### Mutual Exclusion Problem

##### Context: Shared memory systems

- Multi-processor computers
- Multi-threaded programs

### Critical Section

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

#### Mutual exclusion

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

##### Proof

Assume the algorithm is not progress.

**Case 1**: `turn = 0` when both processes are waiting.

Therefore process 0 set `turn = 1` before process 1 set `turn = 0`.

Therefore process 0 can enter the critical section

**Case 2**: Symmetric

#### No starvation

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

