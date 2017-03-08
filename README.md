# study to raft
A small study to the [RAFT consensus algorithm](https://en.wikipedia.org/wiki/Raft_(computer_science)).

## Introduction
Raft is a consensus algorithm designed as an alternative to Paxos. It was meant to be more understandable than Paxos by means of separation of logic, but it is also formally proven safe and offers some new features.[1] Raft offers a generic way to distribute a state machine across a cluster of computing systems, ensuring that each node in the cluster agrees upon the same series of state transitions. It has a number of open-source reference implementations, with full-spec implementations in Go, C++, Java, and Scala.

## List of Raft Implementations
There is a [list of implementations](https://raft.github.io/#implementations) available, like for example [Konrad Malawski's](https://github.com/ktoso) [akka-raft](https://github.com/ktoso/akka-raft).

## TL;DR
Raft is similar to Paxos, but paxos is optimized for throughput where Raft is optimized for consistency. Raft is also optimized for being extremely simple, as far as consensus algorithms go and therefor can be easier for the developer community to understand and so more systems can be engineered and maintained to be truly fault tolerable and consistent. 

## What is it all about?
It is all about agreement (consensus) on shared state. Multiple servers that agree on some shared state eg. that it is consistent. Its all about building consistent, fault tolerant storage systems. The basic idea is the availability and consistency of the data that is shared. 

In a cluster we work with multiple machines that are connected via a network. Also the number of nodes are an odd number like say 3, 5, and so on. 

There is also the concept of 'the majority'. The majority of a cluster determines whether or not the service that the cluster represents is available or not. For example, in a five node cluster you can loose two nodes and still have a majority and thus the service is available. But if you loose three nodes out of five, then you only have two nodes running, and you loose the majority and the service will effectively be in a not-available state. Using multiple nodes and the majority concept takes care of the service availability question. 

The "data consistency" question can be taken care of having the concept of a "majority agreement" of the shared state. When the majority agrees about the shared state, then the data must-be consistent else there is no agreement. So both the availability and the consistency of the shared state is managed by the majority. 

Consensus in thus the key to replication. The most common way to build a replicated system is to use the replicated state machine approach where every replica starts in the same initial state and you agree on the exact sequence of operations that you are going to apply and therefor all the replicas maintain the same state. 

- All replicas agree on the initial state of the system,
- All replicas agree on the sequence of deterministic operations. 	

## Ways to add Consensus to your system
You can code one yourself. You can add one by adding a library to your program like Paxos or Raft, or you can use a Consensus Service like Zookeeper, Etcd, Consul, etc that takes care of the shared state and you can ask for the shared state and always get a consistent view of that shared state.

## Raft Overview
1. Leader election
  - Select one of the servers to act as a cluster leader,
  - Detect crashes, choose a new leader,
  - Heartbeats and timeouts to detect crashes,
  - Randomized timeouts to avoid split votes,
  - Majority voting to guarantee at most one leader per term,
2. Log replication (normal operation)
  - Leader takes commands from the clients and appends them to its log,
  - Leader replicates its log to other servers and overwrites inconsistencies,
  - Built-in consistency check simplifies how logs may differ
3. Safety
  - Only a server with an up-to-date log can become leader,
  - Only elect leaders with all committed entries in their logs,
  - New leader defers committing entries from prior terms.
 
## Resources
- [The Raft Homepage](https://raft.github.io/)
- [The secret lives of data](http://thesecretlivesofdata.com/raft/)

## Video
- [(0'54 hr) Raft, In Search of an Understandable Consensus Algorithm - Diego Ongaro](https://www.youtube.com/watch?v=LAqyTyNUYSY)
- [(1'00 hr) Designing for Understandability: The Raft Consensus Algorithm - Professor John Ousterhout](https://www.youtube.com/watch?v=vYp4LYbnnW8)
- [(0'19 hr) Why we choose the Raft consesus algorithm - Robert Wojciechowski](https://www.youtube.com/watch?v=pN0MlDIMFjY)

## Other Protocols
- [(1'06 hr) Paxos lecture - John Ousterhout](https://www.youtube.com/watch?v=JEpsBg0AO6o)
- [(0'57 hr) The Stellar Consensus Protocol - David Mazières](https://www.youtube.com/watch?v=vmwnhZmEZjc)
- [(0'03 hr)Bitcoin Solves The Byzantine Generals Problem](https://www.youtube.com/watch?v=ExTTxA0K9YY)