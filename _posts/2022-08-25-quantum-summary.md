---
title: Quantum computing in a nutshell
layout: post
date: 2022-08-25 20:40
headerImage: false
tag:
- Quantum
category: blog
author: sergiogimenez
description: Describing basic intuitions behind quantum computing basic concepts
---

Lately, I have been diving into quantum computing and it feels something completely different from the classical computing we are all used to. It's something hard to understand and for me, feels like magic. I'm not really aware of what's really going on under the hood, but I did my best to try to understand at least the intuition behind the concepts on which quantum computing builds.

## The very basics

### A single qubit

| Probability | Outcome |
| ----------- | ------- |
| $p$         | 0       |
| $1-p$       | 1       |

When we measure a qubit (read operation) we determine its state, i.e. it collapses either to 0 or 1.

Important consequences of a `read` operation:
 
* We can only read once.
* Reading is destructive: we cannot copy states.

### Quantum state made of $n$ qubits

Let's say we have a sequence of $n$ qubits. Now, if every qubit is 0 and 1 at the same time, we are going to have in a single classical register (that contains the $n$ qubits) $2^{n}$ results at the same time. In other words, in classical computing you need to generate the bit sequence you want in order to have them. In quantum computing, all these states are there at the same time, and you just pick the one you want.

An example of the previous explanation with 4 qubits / 4 bits.

![clipboard.png](inkdrop://file:Dh3DdsiFL)
*Lu, Xiaowei & Jiang, Nan & Hu, Hao & Ji, Zhuoxiao. (2018). Quantum Adder for Superposition States. International Journal of Theoretical Physics. 57. 10.1007/s10773-018-3779-2.*

### Entanglement

A vague intuition: When $n$ qubits are entangled, they suddenly become strongly correlated. When operations are applied to one qubit, at the same time something also applies to the other qubit.

### A Quantum Program

This probabilites we talked previously can be manipluated using gates. Then, as happens with linear circuits, programming with quantum can be understood as solving linear systems: we have a probability (qubit) that is modified by appliying linear transformations (gates).

### Why Quantum?
In very simple terms, quantum computing is not necessarily faster than classical computing, but due to quantum mechanics, quantum computing has some “special” properties that allow to do things that classical computers cimply can’t.

Some applications:
  * **Superdense coding**. Intuition: transport two classical bits using one qubit.
  * **Man in the Middle attack**. Intuition: just by reading a qubit, you're already collapsing its state. IOW, the attacker listening to a quantum conversation, won't see anything in clear. This somehow applies to Quantum Key Distribution.

## Why Quantum Networks:

* **QKD**. Quantum characteristics are used to assess the probability of the presence of an eavesdropper.
* **Sensors**:  Higher-precision clock synchronization.
* **Distributed Quantum Computation**: Individual quantum processors are networked to carry out quantum information processing tasks in a coordinated way.
 

## What is a Quantum Network?

### Basic Concepts

#### Teleportation

* Alice wants to send information to Bob.
* Alice has one qubit, and Bob has another one. 
* Alice's qubit and Bob's qubit are entangled, forming what is called a *Bell Pair*.
* In this situation, Alice's destroys its qubit by reading it. Instantly, Bob's qubit recreates the same state that was in Alice's qubit.

Things to note here:

* Essentially, all quantum communications require suporting classical communications.
  * Alice stores the qubit state in one classical bit (in a register), same does Bob. Quantum information cannot be stored.
  * Quantum teleportation is not dependant of the distance of sender/receiver, however, since qubits are encoded in photons and transmitted through an optical channel, we cannot comunicate faster than the speed of light.

#### Decoherence

Unfortunately, any physical operation (including simply storing a qubit) gradually degrades the state. Decoherence is the single most important technological fact driving quantum network implementations. We can counter this by using a form of error correction or detection.

Moving a photon large dinstances has a high decoherence, hence a high probability that the photon is degraded through the way to its destination. Since is hard to send a photon in long distances, quantum repeaters must be used.

Remember that quantum information cannot be copied, so we need some sort of amplifier that can ensure that the quantum information has a high fidelity. The technique that quantum repeaters use is called **entaglement swapping**. Basically, it allows to regenerate the signal without needing to copy or store the quantum information.

![clipboard.png](inkdrop://file:bLrRPisLx)

## Quantum Networks


### Distributed-EPR based (or first generation)

A quantum network, consists on establish a distributed entanglement, e.g., Bell Pairs between the endpoints that want to communicate between each other.

In order to guarantee **fidelity** between the communication, **purification** methods are used between distributed entanglements.

Glossary:
* **Fidelity**: It is the quality of a quantum state. IOW, the probability that a quantum state is the desired one once we measure it.
* **Purification**: Form of error correction historically favored in quantum repeater networks. The intuition behind is that this method sacrifices some quantum state to test the fidelity of the others.

#### Flows in Quantum Networks

In networking in general, a flow descibres an instance of a communication service. So, applications ask for flows to the network to exchange information with other applications. For example, a TCP flow is a TCP session between to hosts. IOW, to establish a TCP flow, endpoints need to perform a three way handshake.

In quantum, we can define a flow by a generation of an entanglement, e.g., Bell Pairs, no matter if those are for E2E enteanglement or for swapping.

![clipboard.png](inkdrop://file:JYHnE4Ybs)

#### Quality Metrics of a Flow

* **Fidelity**:It is the quality of a quantum state. IOW, the probability that a quantum state is the desired one once we measure it.
* **Rate**: Generation of entanglements, e.g., Bell Pairs per time unit.

### Packet Switched Approach (Cisco)

Recently, Cisco proposed this new approach, where they define a universal quantum network as one which can accomodate any application, and not just thos that rely on entanglement distribution.

Packet switching allows for a universal design for a next generation Internet where classical and quantum data share the same network protocols and infrastructure. 

They propose an ethernet frame that has a classical header, then the quantum payload and finally a classical trailer:

![clipboard.png](inkdrop://file:MEzEdpmmS)
Traffic could be multplexed by time or by wavelength:

![clipboard.png](inkdrop://file:cOGJejv4X)
### The approach to packet switching

* Near term approach: leave a guard time between payload and classical header. This guard time es equal to the sum of the processing time of the classical headers by the optical switches.
  
  ![clipboard.png](inkdrop://file:__NBSf7Hj)
  
* Assuming quantum nodes can store quantum payloads: There's no need of guard time. There is a memory that stores the quantum payloads till when the classic headers are processed.

  ![clipboard.png](inkdrop://file:RYCXyxGVc)