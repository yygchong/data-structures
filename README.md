# Basic data structures and algorithms in Go [![Build Status](https://travis-ci.org/BTooLs/basic-data-and-algorithms.svg?branch=master)](https://travis-ci.org/BTooLs/basic-data-and-algorithms) [![codecov](https://codecov.io/gh/BTooLs/basic-data-and-algorithms/branch/master/graph/badge.svg)](https://codecov.io/gh/BTooLs/basic-data-and-algorithms)[![Go Report Card](https://goreportcard.com/badge/github.com/BTooLs/basic-data-and-algorithms)](https://goreportcard.com/report/github.com/BTooLs/basic-data-and-algorithms)
Learning Go and TDD while making efficient concurrent algorithms and data structures.

The package is meant to be used as a library. If you have any advice/tip please let me know (ex: open an issue).

#### !! Warning This library wasn't used in production (yet). !!

## Data structures
I will skip the data structures already implemented in the standard libraries (like linked lists).

### Stack [description](https://www.tutorialspoint.com/data_structures_algorithms/stack_algorithm.htm)
Basic stack (FILO) using the builtin linked list, can store any type, concurrency safe, no size limit, implements Stringer.

### Queue [description](https://www.tutorialspoint.com/data_structures_algorithms/dsa_queue.htm) - 
Basic queue (FIFO) using the builtin linked list, can store any type, concurrency safe (optional mutex), no size limit, implements Stringer.

### Hierarchical Queue [description](https://www.researchgate.net/figure/261191274_fig1_Figure-1-Simple-queue-a-and-hierarchical-queue-b) 
An **O(1)/O(1) priority queue** implementation for small integers, that uses an assembly of N simple queues.

It is optimized for large amount of data BUT with small value priorities ( < 1000 ). Can store any  type of elements/values.

**Priority: 0 (highest) - n (lowest)**

For best performance:
- use small priority values (0-100)
- *priorities should not have big holes (sparse, missing values)
- Enqueue ALL the elements before starting to Dequeue
- cannot be reused (when a queue is empty and removed, it cannot be recreated)


It is a map of [priority] = Queue (linked list). Queues are built with LinkedLists, I think slices could be faster, but when having millions of elements memory is more important

Inspired by papers:
- *Revisiting Priority Queues for Image Analysis, Cris L. Luengo Hendriks*
- *Hierarrchical Queues: general description and implementation in MAMBA Image library, Nicolas Beucher and Serge Beucher*

#### Hierarchical Queue benchmarks
This syncronous tests were done to demonstrate that Enqueue/Dequeue is O(1) regardless of the priority queue size. A queue is filled with N elements and equally distributed priorities. The data stored is 1 character.

Each pass consists of : 1 enqueue with increasing priority (0,1,2,3...255,0,1...) and 1 dequeue. K is the priority lowest value (0 - K). 

```bash
go test -bench=.
goos: windows
goarch: amd64
pkg: github.com/btools/basic-data-and-algorithms/src/data/lists
```

|K = 50 | | |
|---|:---:|:---:|
|N = 1000            |20000000               |66.7 ns/op|
|N = 100000          |10000000               |223 ns/op|
|N = 1000000         |10000000               |192 ns/op|
|N = 10000000        |10000000               |199 ns/op|
|N = 100000000       |10000000               |196 ns/op|


|K = 255 | | |
|---|:---:|:---:|
|N = 1000            |10000000               |105 ns/op|
|N = 100000          |10000000               |158 ns/op|
|N = 1000000         |10000000               |182 ns/op|
|N = 10000000        |10000000               |249 ns/op|
|N = 100000000       |10000000               |308 ns/op|