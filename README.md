# Multi-Area IS-IS Routing Lab | GNS3

## Overview

This project demonstrates the implementation and verification of a multi-area Intermediate System to Intermediate System (IS-IS) routing network using six Cisco routers in GNS3.

The lab explores hierarchical routing, Level 1 and Level 2 adjacencies, shortest-path calculation, equal-cost multipath (ECMP), metric optimization, redundancy, and network failover.

## Network Topology

![Network Topology](topology/Network-Topology.png)

### IS-IS Area Design

| Router | IS-IS Area | Router Level |
|---|---|---|
| Khalid-R1 | 49.0001 | Level 1 |
| Khalid-R2 | 49.0001 | Level 1-2 |
| Khalid-R3 | 49.0002 | Level 2 |
| Khalid-R4 | 49.0003 | Level 1-2 |
| Khalid-R5 | 49.0003 | Level 1 |
| Khalid-R6 | 49.0004 | Level 1-2 |

### LAN Networks

- PC-1 LAN: 192.168.1.0/24
- PC-2 LAN: 192.168.3.0/24
- PC-3 LAN: 192.168.2.0/24

## Technologies Used

- Cisco IOS
- GNS3
- IS-IS Level 1 and Level 2
- Wide IS-IS Metrics
- SPF (Dijkstra's Algorithm)
- ECMP
- VPCS
- ICMP and Traceroute

## Lab Objectives

1. Configure IS-IS across multiple routing areas.
2. Establish and verify Level 1 and Level 2 adjacencies.
3. Analyze the IS-IS Link-State Database.
4. Understand SPF and shortest-path metrics.
5. Demonstrate equal-cost paths.
6. Manipulate interface metrics to influence route selection.
7. Test redundancy through router failure simulations.
8. Verify end-to-end connectivity between LANs.

## Experiment 1 — IS-IS Neighbor Discovery

Verify IS-IS adjacencies with:

```cisco
show isis neighbors
```

R3 establishes Level 2 adjacencies with R2, R4, and R6.

## Experiment 2 — ECMP

Initially, R2 calculates two equal-cost paths toward R4:

```text
R2 → R3 → R4 : Metric 20
R2 → R6 → R4 : Metric 20
```

The IS-IS topology table identifies both next hops.

## Experiment 3 — IS-IS Metric Optimization

To prefer R3 over R6, configure the R2 interface toward R6:

```cisco
interface GigabitEthernet2/0
 isis metric 40 level-2
```

After SPF recalculation:

```text
R2 → R3 → R4 : Metric 20
R2 → R6      : Direct metric 40
```

R2 can also reach R6 via R3 with a calculated metric of 20.

This demonstrates that SPF selects the lowest cumulative path cost, rather than necessarily choosing a directly connected neighbor.

## Experiment 4 — Redundancy and Failover

The lab includes redundant backbone links between R2, R3, R4, and R6.

During testing, stopping R6 allowed traffic to reach the destination through the remaining backbone:

```text
PC-1 → R1 → R2 → R3 → R4 → R5 → PC-3
```

Traceroute confirmed end-to-end reachability through the alternative path.

## Verification Commands

```cisco
show isis neighbors
show isis database
show isis database detail
show isis topology
show ip route isis
show clns interface
```

## Conclusion

This laboratory demonstrates how IS-IS creates a scalable, hierarchical link-state routing domain and how SPF responds to link metrics and topology changes.

The experiments provide practical insight into IS-IS Level 1/Level 2 operations, ECMP, routing metrics, redundancy, and network convergence.
