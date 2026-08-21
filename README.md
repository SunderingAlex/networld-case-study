# Unequal Information in Multi-Router Systems

My master's thesis, *Unequal Information in Multi-Router Systems: Effects of System Composition in Shared Environments*, studies how routing-system composition shapes outcomes when differently informed routers share a capacity-constrained environment. I designed and implemented NetWorld, a custom Python and NetworkX agent-based simulator, to conduct the study through controlled computational experiments.

## Research framework

A multi-router system (MRS) contains agents guided by multiple routers through a shared environment with finite movement resources. Each route changes the conditions encountered by other agents, connecting individual routing decisions to congestion, delay, and system-wide behavior. MRSs appear in road traffic, warehouse robotics, aviation, maritime routing, data-center networks, and workflow scheduling.

The experiment centers on unequal information between two routers that share one routing objective: minimizing each agent's delay. Local-Informed Pathfinding (LIP) uses a fuzzy snapshot of current edge occupancy. System-Informed Pathfinding (SIP) also receives the current locations and planned routes of agents in its own cohort. Varying the proportion assigned to each router creates a continuum of routing-system compositions.

The thesis follows four research questions:

1. How do population-wide routing outcomes change across routing-system compositions?
2. How do topology and occupancy shape those patterns?
3. How do SIP-cohort outcomes change as the SIP cohort grows?
4. How do LIP-cohort outcomes change as the surrounding composition changes?

## NetWorld model

NetWorld builds each environment as a connected directed graph with 64 nodes. Four topology families provide distinct connectivity structures: Periodic Grid, Radial Ring, Small World, and Block Model. Edge length varies between network instances, lane count varies within each network, and both contribute to finite edge storage and per-tick admission capacity.

Each run combines a fixed network with a fixed trip schedule. Every scheduled trip has a spawn tick, origin, and destination. Router assignments are applied per run from a stable agent ordering as the SIP proportion changes. A separate run state tracks active agents, edge loads, movement decisions, and arrivals as the simulation advances through discrete ticks.

Each tick follows six ordered tasks:

1. Instantiate scheduled agents.
2. Build the current traffic and routing snapshot.
3. Collect ranked movement proposals from stationary agents.
4. Arbitrate edge entry through a seeded conflict resolver.
5. Commit movements and update agent states.
6. Process arrivals and release occupied capacity.

Congestion develops through a spillback model. Agents reaching the end of an edge continue occupying its storage while they wait for admission to the next edge. These queues can propagate backward through the network. Total delay combines queue delay with detour, which measures travel beyond the shortest-path distance.

## Routing and decision logic

Both routers generate multi-step plans through modified A* search and use cached shortest-path distances as their heuristic. Their shared edge-cost components are free-flow travel time and a congestion penalty. SIP also applies a competition penalty based on predicted SIP entry demand.

LIP evaluates paths from the current traffic snapshot. SIP builds a time-indexed forecast from the locations and active plans of its cohort, discounts demand farther into the forecast horizon, and evaluates edges at the times agents expect to reach them. This forecast also supports planned waiting when a later movement offers a lower predicted cost.

Plan staleness checks, a minimum improvement threshold, a stuckness threshold, and bounded route comparisons regulate replanning. I calibrated the congestion and competition weights through a 73,500-run grid search covering 49 candidate pairs.

## Experimental design

The completed factorial contains:

- 4 topology families
- 30 network instances per topology
- 5 occupancy tiers
- 30 schedules per network-occupancy condition
- 11 SIP assignment proportions, from 0% through 100%
- 198,000 primary simulation runs

Every network-occupancy-schedule condition is reused across all 11 assignment proportions. A stable agent ordering preserves router assignments as the SIP cohort grows, supporting paired comparisons across the composition continuum.

Local `random.Random` instances use stable identifiers at the network, schedule, and run-state levels. These seeds govern topology generation, edge attributes, trip generation, router assignment, and conflict arbitration. Each output record carries the identifiers and experimental labels needed for reconstruction and grouped analysis.

## Metrics and analysis

NetWorld records run completion, travel time, queue delay, detour, reroutes, crowding exposure, progress stalls, ETA error, and several related measures. The analysis summarizes completed trips for the full population and for each router cohort, then compares trajectories across composition, topology, and occupancy. Mean, median, and 90th-percentile delay provide complementary views of central behavior and higher-delay trips.

## Selected findings

- Greater SIP assignment generally aligned with lower aggregate delay, with larger changes concentrated at higher assignment proportions.
- Topology and occupancy shaped the size and trajectory of composition effects.
- LIP agents frequently experienced spillover improvements in higher-delay outcomes. Reductions in queue delay accompanied increased detour.

## Study scope and research artifact

The study covers abstract graph environments, a spillback congestion model, two selfish routers sharing one objective, seeded admission arbitration, and bounded SIP forecasts. These choices support controlled comparison across routing-system compositions and environmental conditions. Future experiments can extend the model through additional information structures, router objectives, admission rules, topology families, occupancy levels, and human factors.

The complete thesis provides the literature review, equations, figures, condition-level results, formal limitations, ethics discussion, references, and appendices. A copy is available on request.

This repository currently serves as the public research case study. A future code release can package the simulator with documented dependencies, provenance, licensing, and reproducibility instructions.
