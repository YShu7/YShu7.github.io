---
title: Synchronization Primitives
tags: [Parallel and Distributed Algorithm]
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
- A queue of blocked processer - initially empty

```
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

The functions are executed atomically

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
- wait-queue

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

#### Java-style Monitor

**P1** takes over the execution

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

Avoid deadlock => Avoid cycles or have a total ordering of the chopsticks

![image-20200229160028007](/assets/images/pd2-1.png)

![image-20200229160154377](/assets/images/pd2-2.png)

This is called a **wait-for graph**, we use it to check if there is a cycle

In this graph, **vertex** is representing **resources**.

### The Producer-Consumer Problem

A circular buffer of size n

A single producer and a single consumer

##### Why circular buffer?

1. Producer and consumer speed is different
2. Not constant

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

