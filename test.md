# Title

**Multi‑Agent Collaboration & Memory Architecture for UAV Teams**
Draft v1 — Prepared for Odat (Agent Developer)

---

## Slide 1 — Motivation & Goals

* Coordinate 2–N UAVs to share perception, plans, and state under bandwidth, compute, and safety constraints.
* Design a *memory subsystem* that persists & disseminates facts (maps, tracks, tasks, logs) across air/ground.
* Target platforms: Jetson Nano (edge), SITL + MAVLink (PX4/ArduPilot), ROS 2 (DDS/Zenoh).
* Outcomes: architectural blueprint, SOTA references, and an adoption roadmap.

---

## Slide 2 — Collaboration & Memory: What to Cover (Aspects)

1. **Coordination & Task Allocation (MRTA)**
2. **Communication & Middleware** (DDS/ROS 2, MAVLink, bridging/routers)
3. **Shared Memory**: maps, tracks, objects, task graphs (centralized vs. distributed)
4. **Collaborative Perception & Mapping** (bandwidth-aware fusion)
5. **Learning & Adaptation** (MARL + CTDE; policy sharing)
6. **Safety, Verification & Runtime Monitoring**
7. **Security & Trust** (authz/authn, enclaves, message signing)
8. **Scalability & Systems** (edge–mesh–cloud, resiliency, fault-tolerance)
9. **Human‑in‑the‑Loop & Ops** (debugging, logging, playback, KPIs)

---

## Slide 3 — 1) Coordination & Task Allocation (MRTA)

* Problem: assign tasks (search, track, relay, delivery) to heterogeneous UAVs under constraints.
* Design pattern: **centralized planner + decentralized execution** with local re‑auction on failures.
* Algorithmic families:

  * **Market/Auction**: CBBA/CBBA‑PR (bundle building, conflict resolution), contract net.
  * **Graph/ILP/TL**: MILP/Min‑Cost Flow; LTL/STL‑constrained planners.
  * **Heuristics + Receding horizon** for dynamic pop‑up tasks.
* Engineering:

  * Task API (schema): id, type, reward/cost, pre/post‑conditions, time windows.
  * Policies for preemption, handover, timeout, failover.

---

## Slide 4 — 2) Communication & Middleware

* **ROS 2 + DDS** as primary pub/sub; evaluate RMW (Fast‑DDS, CycloneDDS, *Zenoh‑DDS* gateway) for mesh links.
* **MAVLink** telemetry/control; MavSDK/MAVROS bridge to ROS 2 topics & actions.
* **Routers/Bridges**: DDS‑Router/Zenoh for domain bridging, NAT traversal, and bandwidth shaping.
* **QoS**: Reliability, history depth, deadline, lifespan tuned per topic (e.g., best‑effort for image, reliable for cmds).

---

## Slide 5 — 3) Memory Subsystem (What to Store & How)

* **Metric‑Semantic Map Layer**: pose graph, landmarks, semantics, uncertainty.
* **Tracks/Objects Layer**: targets, obstacles, friendlies; with identities & confidences.
* **Task & Plan Layer**: task graph, allocations, status, provenance.
* **Event Log Layer**: time‑ordered actions, alerts, health.
* **Representations**: Factor graphs (mapping), **CRDT** sets/maps (tasks/events), knowledge graph (semantics).
* **Consistency**: eventual (gossip/CRDT) for non‑critical data; strong/atomic for arm/disarm & safety‑critical ops.

---

## Slide 6 — 4) Shared Mapping & Perception

* **Distributed SLAM**: summarize & exchange submaps (keyframes/landmarks) to avoid double‑counting.
* **Bandwidth‑aware**: send *only* informative regions/features; prioritize frontiers/targets.
* **Map merge**: inter‑robot loop closures; robust data association under asynchrony.
* **Storage**: local rolling cache on Jetsons; periodic uplink to ground node for persistence & replay.

---

## Slide 7 — 5) Learning & Adaptation (Multi‑Agent RL)

* **CTDE**: centralized training, decentralized execution; share critic or value factorization.
* **Policy sharing** across similar drones; per‑vehicle adapters for dynamics.
* **Comms‑aware policies**: learn what/when to communicate; penalize bandwidth.
* **Safe exploration**: shields/CBFs; curriculum via SITL before flight.

---

## Slide 8 — 6) Safety, Verification & Runtime Monitoring

* **Hard safety layer**: control barrier functions (CBFs) for inter‑UAV collision avoidance.
* **Runtime Verification**: monitors enforce temporal rules (e.g., “stay > d, < v near no‑fly zone”).
* **Health checks**: heartbeat, link quality, pose sanity; automatic loiter/RTL on violations.
* **Mission replay**: record bag logs, dashboards (alerts, near‑misses, rule violations).

---

## Slide 9 — 7) Security & Trust

* **ROS 2 enclaves (SROS2)**: identity, permissions, encryption (DDS‑Security).
* **Signed agent messages**; per‑topic ACLs; audit logs.
* **Protocol interop**:

  * *MCP* for tools/data access (agents↔resources),
  * *A2A* for agent↔agent capability discovery & collaboration (future‑proofing).

---

## Slide 10 — 8) Scalability & Systems (Edge⇄Mesh⇄Cloud)

* **Edge**: on‑board VIO/SLAM, detection, CBFs; lossy best‑effort streams.
* **Mesh**: Zenoh/DDS router; priority queues; topic throttling; content filtering.
* **Ground/Cloud**: global map fusion, task planner, metrics store, dashboards.
* **Failure modes**: partition‑tolerant CRDTs; degrade to solo mode; resume sync on rejoin.

---

## Slide 11 — 9) Human‑in‑the‑Loop & Ops

* Operator console: map, tracks, allocations, health, bandwidth.
* One‑click *Takeover* & *Handoff*; explainable decisions (why this task/route?).
* Tooling: rosbag2, playback, unit tests in SITL; CI on mission scenarios.

---

## Slide 12 — Reference Architectures (Patterns)

* **Pattern A — Central Brain**: centralized planner + distributed SLAM summaries.
* **Pattern B — Federated**: local planners + periodic auctions; CRDT task ledger.
* **Pattern C — Swarm/Gossip**: no central brain; stochastic consensus; robust but limited guarantees.
* Choose by comms reliability, safety criticality, and team size.

---

## Slide 13 — Recommended Stack (For 2 UAVs + 2 Jetsons)

* **OS/Runtime**: Ubuntu + ROS 2 Humble.
* **RMW**: start Fast‑DDS/Cyclone; trial *Zenoh* bridge on mesh links.
* **Airlink**: MAVLink→MAVROS; ROS 2 topics/actions.
* **Mapping**: Kimera‑Multi/LAMP‑2.0 on ground; VIO on edge.
* **Tasking**: CBBA planner on ground; re‑auction on loss.
* **Memory**:

  * Map = factor graph store;
  * Tasks/Events = CRDT store;
  * Knowledge = lightweight KG (KnowRob or custom).
* **Safety**: CBF layer + RV monitors; geo‑fence.

---

## Slide 14 — Data Contracts (Examples)

* **/uav/pose**{t, SE(3), cov}; **/uav/objects**{id, class, bbox, conf};
* **/tasks**{id, type, reward, window, pre/post, status} (CRDT set+map);
* **/map_summary**{keyframes, landmarks, info}; **/alerts**{rule, severity, who, when}.

---

## Slide 15 — KPIs & Benchmarks

* Task completion %, latency/jitter, map ATE/RPE, bandwidth (kbps), packet loss %, near‑misses, rule violations, MTBF.
* Bench suites: SITL missions; comms dropouts; multi‑uav detection; map merge stress; RV/CBF intrusion tests.

---

## Slide 16 — Roadmap (90 Days)

* **Weeks 1–2**: bring‑up ROS 2/MAVROS; logging; SITL endpoints (14540→50051, 14541→50052).
* **Weeks 3–4**: CBBA baseline; task API; simple frontiers.
* **Weeks 5–6**: Kimera‑Multi/LAMP‑2.0 integration; bandwidth throttling.
* **Weeks 7–8**: CBF safety layer; RV rules; failure drills.
* **Weeks 9–10**: Zenoh/DDS‑Router trials; metrics dashboard.
* **Weeks 11–12**: Field test; post‑mortem; iterate.

---

## Slide 17 — Risks & Mitigations

* **Bandwidth collapse** → content filters, Where‑to‑communicate, adaptive QoS.
* **Time sync drift** → NTP/PTP; time‑stamped messages; delay compensation.
* **Map inconsistency** → summarized factors; anti‑factor/CI; conflict resolution.
* **Safety incidents** → hard min‑separation via CBF; RV guards; RTL fallback.

---

## Slide 18 — Appendix: Representative Papers (by Aspect)

**MRTA & Planning**: CBBA/CBBA‑PR; Gerkey & Matarić taxonomy; STL/LTL planning; scalable STL fleet ops.
**Comms**: ROS 2 RMW comparisons (Fast‑DDS/Cyclone/Zenoh); DDS Routers; MAVLink.
**Shared Mapping**: DDF‑SAM (decentralized data fusion); Kimera‑Multi (distributed metric‑semantic SLAM); LAMP 2.0.
**Collaborative Perception**: Where2comm (bandwidth‑aware fusion for cars/drones).
**Learning**: MADDPG; QMIX; MAPPO (CTDE).
**Safety/Verification**: CBF surveys/tutorials; STL multi‑robot planning; runtime verification for MAS & DRL.
**Security/Trust**: SROS2 (DDS‑Security, enclaves); FIPA ACL; MCP/A2A protocols.

---

## Slide 19 — Thank You

* Backup slides: extended bibliography & links available on request.
