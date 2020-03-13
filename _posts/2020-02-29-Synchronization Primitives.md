---
title: Synchronization Primitives
tags: [Parallel and Distributed Algorithm]
mathjax: true
---

### Synchronization Primitives

##### Why do we need synchronization primitives?

To deal with busy wait problem

##### Synchronization Primitives

**OS-level APIs** that the program may call

- Semaphores
- Monitors

### Semaphores

Internally, each semaphores has

- A boolean value - initially true
- A queue of blocked processes - initially empty

```java
P():
if (value == false) {
	add myself to queue and block;
}
value = false;
V():
value = true;
if (queue is not empty) {
	wake up one arbitrary process on the queue
}
```

The functions are executed **atomically**

**Exactly one process** is waken up in V()

#### Using Semaphore for Mutual Exclusion

```
RequestCS() {P();}
ReleaseCS() {V();}
```

**No busy waiting**

### Monitors

Higher level compared to semaphore.

Java only has monitors. Every object in Java is a monitor (has a monitor lock)

Each monitor has two queues of blocked processes

- monitor-queue

  A queue of threads waiting for the lock associated with the monitor

- wait-queue

  A queue of threads waiting for some condition to become true

  > Java does not have condition variables. Associated with each object there is a single wait queue for conditions

![image-20200229160525660](/assets/images/pd2-3.png)

There are 3 special methods for using when **inside a monitor**:

`object.wait()`: Add myself to the wait-queue, exit monitor and then block

`object.notify()`: If wait-queue is not empty, pick one arbitrary process from wait-queue and unblock it

`object.notifyAll()`: If wait-queue is not empty, unblock all processes in wait-queue

### Two kinds of monitors

P0: `object.wait()`

P1: `object.notify()`

#### Hoare-style Monitor

**P0** takes over the execution

One of the threads that was waiting on the condition variable continues execution.

##### Advantage

The thread that was notified on the condition starts its execution without intervention of any other thread

On waking up, it can assume that the condition is true

```
if (!B) x.wait();
```

#### Java-style Monitor

**P1** takes over the execution

The thread that made the notify call continues its execution.

```
while (!B) x.wait();
```

No need to do context switch, more popular

### Other Ways of Using Monitors in Java

```Java 
public synchronized void myMethod() {}
=
public void myMethod() {
	synchronized(this) {}
}
```

Static methods can also be synchronized - the monitor clock is **class wide**

### Nested Monitor in Java

```Java
synchronized(ObjA) {
	synchronized(ObjB) {
		ObjB.wait(); // Here Java only realeases the monitor lock on ObjB and not the monitor lock on ObjA
	}
}
// Will not be able to get the monitor lock of ObjA
synchronized(ObjA) { // This piece of code will block and will not reach ObjB.notify() - deadlock!
	synchronized(ObjB) {
		ObjB.notify();
	}
}
```

### Dining Philosopher Problem (Dijkstra'65)

It's useful in bringing out issues associated with concurrent programming and symmetry.

#### Avoid deadlock

=> Avoid cycles or have a total ordering of the chopsticks

- Requiring one of the philosophers to grab forks in a different order
- Requiring philosophers to grab both the forks at the same time
- Assume that a philosopher has to stand before grabbing any fork. Allow at most four philosophers to be standing at any given time.

![image-20200229160028007](/assets/images/pd2-1.png)

![image-20200229160154377](/assets/images/pd2-2.png)

This is called a **wait-for graph**, we use it to check if there is a cycle

In this graph, **vertex** is representing **resources**.

### The Producer-Consumer Problem

A circular buffer of size $n$ in which $inBuf$ and $outBuf$ are incremented modulo $size$ to keep track of the slots for depositing and fetching items.

A single producer and a single consumer

#### Conditional Synchronization

It requires a process to **wait for some condition to become true** (such as the buffer to become non-empty), before continuing its operations

##### Why circular buffer?

1. Producer and consumer speed is different
2. Producer and consumer speed is not constant

```Java
void produce() {	
	synchronized(sharedBuffer) {
  	while(sharedBuffer is full) { // if there is only one producer, can use if
    	sharedBuffer.wait(); // when returning from wait(), sharedBuffer can be full again (different from P())
  	}
    boolean bufferWasEmpty = buffer.isEmpty();
    add an item to buffer;
    if (bufferWasEmpty)
      sharedBuffer.notify() // notification is lost if no process is waiting (different from V())
	}
}
void consume() {
  synchronized(sharedBuffer) {
    while(sharedBuffer is empty) {
      sharedBuffer.wait();
    }
    boolean bufferWasFull = buffer.isFull()
    remove an item from buffer;
    if (bufferWasFull)
      sharedBuffer.notify();
  }
}
```

### The Reader-Writer Problem

Multiple readers and writers are accessing a database

A writer must have exclusive access

But readers may simultaneously access the database

```Java
void writeDB() {
  synchronized(object) {
    while(numReader > 0 || numWriter > 0)
      object.wait();
    numWriter = 1;
  }
  // write to DB
  synchronized(object) {
    numWriter = 0;
    object.nofityAll(); // becasue there can be many readers blocked
  }
}
void readDB() {
	synchronized(object) {
    while(numWriter > 0) // must be while
      object.wait();
    numReader++;
  }
  // read from DB
  synchronized(object) {
    numReader--;
    object.notify(); // it must be a writer notified, can change to notifyAll() to wake up all writers, but only one of them can have the monitor lock
  }
}
```

#### The Starvation Problem

Writers may get starved if there is a continuous stream of readers, because `numReader` will not drop to 0

