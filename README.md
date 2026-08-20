# NetWorld: Unequal Information in Multi-Router Systems

NetWorld is the custom multi-agent simulator I designed and implemented for my master's thesis, *Unequal Information in Multi-Router Systems: Effects of System Composition in Shared Environments*.

## Research question

The project asks what happens when agents sharing the same network do not have equal access to routing information.

The central issue is not simply whether a better-informed router performs better. It is how the composition of informed and less-informed routers changes congestion, delay, spillover effects, and system-wide behavior for everyone using the shared environment.

## What I built

- A custom multi-agent network simulator in Python and NetworkX
- Router agents with different levels of access to network information
- Graph-based pathfinding built around modified A* routing
- Congestion and route-conflict systems
- Seeded, reproducible simulation runs
- A controlled factorial experiment spanning network topology, occupancy, and router composition
- Analysis and visualization workflows for distributional and system-level outcomes

## Experimental scope

The completed study used 198,000 primary simulation runs. Rather than treating router intelligence as an isolated property, the experiment varied the surrounding system to measure both direct effects and consequences imposed on other agents.

## What the project demonstrates

- Translating an ambiguous systems question into a controlled computational experiment
- Designing agent behavior and graph-based simulation architecture
- Managing large, seeded experiment batches
- Analyzing outcomes across multiple interacting experimental factors
- Connecting implementation decisions to a formal research argument
- Communicating technical methods and limitations in a completed thesis

## Public-release scope

This repository is a public case study of completed research. The original research code and working materials remain private.

Any future source release will be a clean, separately validated extraction with documented dependencies, provenance, and licensing.
