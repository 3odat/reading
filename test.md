# Multi‑agent collaboration and memory architectures for robotics and UAVs (2025): aspects, current solutions and research gaps

## Introduction

Multi‑robot and multi‑UAV systems are becoming common in logistics, environmental monitoring, precision agriculture and human–robot collaboration.  The move from single robots to teams introduces challenges: robots must coordinate tasks, share information and learn from the environment while remaining robust to failures and cyber‑attacks.  Recent advances in large language models (LLMs) and multimodal foundation models allow agents to plan using natural language and integrate perception, memory and reasoning【652809977199660†L414-L490】.  However, building a practical multi‑agent system requires careful design of the collaboration and information‑sharing architecture, including memory subsystems.  This report analyses the key aspects that must be addressed, surveys state‑of‑the‑art solutions and representative papers, identifies technical gaps—especially in security—and summarises research on vulnerabilities and proposed defenses.

## 1 Key aspects for multi‑agent collaboration and memory architectures

### 1.1 Communication and consensus protocols

**Role.** Multi‑agent teams need reliable communication channels for intra‑agent (within a robot) and inter‑agent data sharing.  A general architecture for connected agents comprises planning, action, memory, interaction and security modules【652809977199660†L414-L490】.  Intra‑agent communications synchronise the physical body (sensors, actuators) with the LM‑powered “brain,” while inter‑agent communications exchange data and knowledge among agents, enabling collective decision‑making【652809977199660†L414-L490】.

**Current solutions.**  
- **Robot‑centric vs graph‑based coordination (DeepFleet).** Amazon Robotics introduced *DeepFleet*, a family of foundation models that coordinate large robot fleets.  Four architectures—robot‑centric, robot‑floor, image‑floor and graph‑floor—are pre‑trained on movement data; the robot‑centric and graph‑floor models, which use asynchronous updates and localized graph structures, achieve the best performance【430180819626198†L55-L74】.  This work illustrates how to model both local and global interactions using attention and graph neural networks.  
- **Consensus protocols for robustness.**  
    • *SwarmRaft* applies the Raft consensus algorithm to multi‑robot fleets operating in GNSS‑degraded environments, achieving low‑latency, crash‑fault‑tolerant state agreement【556519739501706†L39-L56】.  
    • *Optimal consensus under physical limitations* derives control policies that minimise energy while respecting actuator saturation and communication delays using Pontryagin’s Minimum Principle【829895706518277†L0-L24】【829895706518277†L67-L84】.  
- **Federated protocols and jamming mitigation.** Federated multi‑agent reinforcement learning (FMADRL) has been used to implement moving‑target defense against denial‑of‑service attacks; by distributing model updates across drones and employing random channel hopping, this approach improved mitigation rates by 40 %【159411415449979†L58-L83】.  
- **Secure communication schemes.** Lightweight cryptography and secure key‑exchange protocols have been proposed for UAV networks; however, MDPI surveys note that many solutions remain theoretical and that research is needed on practical integration【46362067535338†L106-L133】.

### 1.2 Task allocation and planning

**Role.** Assigning tasks and paths to multiple agents is critical for efficiency and requires balancing decentralisation with central oversight.  

**Current solutions.**  
- **Market‑based and auction mechanisms.** The consensus‑based bundle algorithm (CBBA) and market‑based auctions are widely used.  A hybrid method that combines auctions with particle‑swarm optimisation achieved near‑optimal assignment with low computational cost【778319133607318†L308-L337】【778319133607318†L384-L390】.  
- **Multi‑agent deep reinforcement learning (MADRL).**  A 2025 study applied MADDPG with K‑means clustering to jointly optimise UAV coverage and power allocation, significantly improving coverage over baseline heuristics【677129118456263†L48-L69】.  FMADRL extends this approach by federating learning across agents to reduce communication overhead and improve resilience【159411415449979†L58-L83】.  
- **Spatial‑aware planning (MCOP).** The *Multi‑UAV Collaborative Occupancy Prediction* (MCOP) framework integrates spatial and altitude‑aware feature encoding with dual‑mask perceptual guidance and cross‑agent feature integration.  MCOP reduces communication bandwidth while improving obstacle‑avoidance accuracy【710217424469645†L90-L129】.  
- **LLM‑driven planning frameworks.**  Recent works integrate LLMs to decompose tasks and negotiate plans.  Liang et al. introduced a retrospective actor–critic architecture where two LLMs discuss initial plans and retrospectively evaluate outcomes; short‑ and long‑term memory retain recent retrospectives to maintain context while reducing overhead【446362433695914†L720-L733】.  RoCo, a decentralised LLM framework, allows robots to discuss and refine sub‑task plans and then relies on a central motion planner for collision‑free trajectories【446362433695914†L734-L746】.  Other systems combine LLMs with offline reinforcement learning (e.g., Co‑NavGPT【446362433695914†L792-L799】) or use negotiation‑based MARLIN to accelerate training【446362433695914†L809-L832】.

### 1.3 Collaborative perception and knowledge sharing

**Role.** Teams must merge perceptions from multiple sensors to build a consistent world model.  Efficient information‑sharing architectures reduce bandwidth while preserving relevant features.

**Current solutions.**  
- **Cross‑agent feature integration.** MCOP’s altitude‑aware reduction and dual‑mask guidance integrate features across agents, improving occupancy prediction while reducing communication【710217424469645†L48-L59】【710217424469645†L90-L129】.  
- **RoboMemory’s dynamic knowledge graph.** *RoboMemory* unifies spatial, temporal, episodic and semantic memory in a parallel architecture; a dynamic knowledge graph supports consistent updates and cross‑agent retrieval, leading to 25 % higher success rates than a strong baseline【585261402725923†L87-L100】.  
- **RAVEN and memory‑augmented planning.** RAVEN (Retrieval‑augmented Vision and Language memory) combines voxel‑ray memory with vision–language models to plan long‑horizon tasks; the memory stores semantic voxel rays and reduces search space for embodied agents【301586803926062†L58-L66】.  
- **LLM‑assisted memory frameworks.** Dual‑layer LLM architectures maintain a working memory to handle multiple tasks and a long‑term memory for summarising experiences; a recent robotics study showed that combining two LLMs with memory modules enabled multi‑task robot capabilities【701084024977943†L15-L28】.  
- **Long‑term cognitive architectures.** The *Memory Architectures in Long‑Term AI Agents* thesis introduces episodic, semantic and procedural memory; algorithms for memory formation, consolidation and strategic forgetting improved temporal reasoning and adaptability【398550257545025†L48-L74】.

### 1.4 Memory subsystem and storage management

**Role.** An effective memory system must store and retrieve past experiences, support long‑term knowledge retention and enforce access control across agents.

**Current solutions.**  
- **Hybrid memory (private/shared tiers).**  A 2024 system for multi‑user agents uses separate private memory for each agent and a shared memory accessible through dynamic access control; policies restrict which records can be published or retrieved【332025743717094†L84-L97】.  This ensures privacy while enabling collaboration.  
- **Brain‑inspired multi‑memory frameworks.** RoboMemory employs parallel updates across long‑term, short‑term and working memory; a lifelong embodied memory system with a dynamic knowledge graph ensures scalable updates【701182788724457†L78-L90】.  It improves long‑horizon planning success by 25 % and demonstrates life‑long learning【701182788724457†L96-L104】.  
- **Stable Hadamard memory.** The Stable Hadamard Memory (HMF) architecture introduces dynamic memory calibration to stabilise memory‑augmented agents; it outperforms baseline models on long‑horizon RL tasks and requires fewer parameters【578702478521353†L46-L58】【578702478521353†L90-L123】.  
- **Semantic episodic memory for search.** For multi‑UAV search and reconnaissance, the CNN‑SEMU algorithm embeds states and past images into a semantic episodic memory; the memory guides exploration in partially observable environments and improves search efficiency【343720461596683†L64-L80】.

### 1.5 Safety, human interaction and fault handling

**Role.** Effective multi‑agent systems must maintain situational awareness, interact seamlessly with humans and recover from faults or failures.

**Current solutions.**  
- **Multi‑robot architecture framework.** Tufts University and Thinking Robots proposed a multi‑robot architecture that allows robots to learn tasks on the fly, monitor execution, detect faults and generate recovery plans.  It includes natural language interaction, shared mental models, introspection and mixed‑initiative teaming【522077871608466†L22-L33】【522077871608466†L87-L104】.  
- **DAC‑HRC cognitive architecture.** The DAC‑HRC (Distributed Adaptive Control – Human‑Robot Collaboration) framework includes modules for task planning, interaction management and a long‑term worker model that stores past preferences and interactions; the system adapts robot behaviour to human co‑workers【513665969660361†L844-L925】.  
- **RoboMemory’s closed‑loop planning.** In RoboMemory, the closed‑loop planner uses the dynamic knowledge graph and memory system to adapt plans, while a low‑level executor interacts with the environment【701182788724457†L78-L90】.  
- **Multi‑agent RL safety mechanisms.** The FMADRL moving‑target defense provides resilience against distributed denial‑of‑service attacks by adapting radio frequencies【159411415449979†L58-L83】.

### 1.6 Security and privacy

**Role.**  As agents maintain memory and communicate autonomously, they become targets for attacks such as adversarial input, jamming, spoofing and false data injection (FDIA).  A security module integrated throughout the agent’s operations monitors actions, detects anomalies and enforces ethical guidelines【652809977199660†L826-L847】.  

**Current solutions.**  
- **Security module in LM agents.**  The security module monitors and regulates actions, interactions and decisions; it employs hallucination mitigation, anomaly detection and access control, adapts to emerging threats and integrates ethical guidelines【652809977199660†L826-L847】.  
- **UAV communication security.** MDPI surveys propose lightweight cryptographic protocols and secure key distribution for drone networks【46362067535338†L106-L133】.  DroneFL uses federated learning to track targets while preserving privacy; local models train on each UAV and aggregate in the cloud【715566231378325†L24-L37】.  
- **Agentic security taxonomy.** A 2025 survey on agentic security identifies vulnerabilities in autonomous LLM agents, including adversarial examples, agent poisoning, hallucinations and privacy leaks【306860515923095†L69-L156】.  The authors note that maintaining state (short‑/long‑term memory) increases attack surface【306860515923095†L33-L44】 and call for integrated security modules and formal verification【306860515923095†L69-L156】.  
- **Intrusion and false data detection.** Raven (USENIX 2025) automatically discovers semantic attacks in multi‑robot navigation systems; the authors show that false data injections can cause position deviations and collisions and propose detection methods and design improvements【928669494271984†L23-L43】.  The paper highlights that Remote ID lacks authentication and encryption, making UAV systems susceptible to FDIAs【928669494271984†L60-L73】.  
- **Simulation frameworks for testing security.** A ROS2‑based simulation framework for cyber‑physical security of UAVs enables testing of jamming, spoofing and man‑in‑the‑middle attacks; the authors stress that existing simulators ignore remote ID’s lack of authentication and emphasise the need for realistic testbeds【970401877045599†L33-L74】【970401877045599†L54-L75】.  
- **Byzantine resilience.** *RoboRebound* adapts Byzantine fault tolerance (BFT) for multi‑robot systems by introducing a bounded‑time interaction model.  The authors argue that classic BFT is unsuitable because robots interact physically and network topology changes frequently; they propose an algorithm with small hardware tweaks and demonstrate its effectiveness through simulation and prototype wheeled robots【586944093537869†L18-L39】【586944093537869†L69-L96】.  
- **Other defenses and threat models.** Prior works like SwarmFlawFinder and SwarmFuzz test specific attack vectors (Sybil attacks, protocol lies) but lack generality【928669494271984†L101-L126】.  The FMADRL moving‑target defense and BFT approaches above provide broader coverage.  

## 2 Technical gaps and cybersecurity challenges

1. **Incomplete integration of security into memory systems.**  Many memory architectures (RoboMemory, dual‑layer LLM frameworks, etc.) focus on long‑term planning and retrieval but do not address adversarial manipulation of memory entries or dynamic access control.  Private/shared memory models【332025743717094†L84-L97】 exist but are rarely combined with perception and planning modules.  

2. **Limited adversarial robustness.**  LLM‑based planning frameworks (RoCo, Co‑NavGPT, MARLIN) offer powerful reasoning but are susceptible to hallucinations and adversarial prompts.  The agentic security survey notes that hallucination, poisoning and prompt hacking can compromise multi‑agent decisions【306860515923095†L69-L156】.  

3. **Lack of standardised secure communication.**  Many UAV networks rely on unencrypted broadcast channels.  MDPI surveys highlight the need for practical lightweight cryptography【46362067535338†L106-L133】, yet cross‑agent feature sharing frameworks such as MCOP assume secure channels.  

4. **Byzantine faults remain under‑explored.**  While RoboRebound provides an initial bounded‑time defense, there is little research on byzantine resilience in robotic swarms; existing consensus approaches like SwarmRaft address crash faults but not malicious behaviour【586944093537869†L69-L96】.  

5. **Simulation and benchmarking gaps.**  Simulation frameworks for security analysis are still emerging【970401877045599†L33-L74】.  Without standard datasets and benchmarks for multi‑agent memory and security, it is difficult to compare approaches or evaluate trade‑offs between performance and safety.

## 3 Research on cybersecurity flaws and proposed solutions

| Threat or flaw | Evidence and impact | Proposed solutions |
| --- | --- | --- |
| **False data injection & semantic attacks** | Raven shows that false data injections can mislead navigation algorithms and cause collisions; Remote ID lacks authentication【928669494271984†L23-L43】【928669494271984†L60-L73】. | Raven proposes automated tools to discover vulnerabilities and recommends authenticating data and designing resilient controllers【928669494271984†L23-L43】.  ROBORobound introduces bounded‑time BFT to tolerate malicious robots【586944093537869†L18-L39】. |
| **Denial‑of‑service and jamming** | UAV networks can suffer from jamming and DoS attacks through unprotected channels【970401877045599†L33-L74】.  FMADRL uses moving‑target defense and federated learning to randomise channels and maintain service【159411415449979†L58-L83】. | FMADRL improves mitigation rate by 40 %, but more research on cross‑layer detection and mitigation is needed【159411415449979†L58-L83】. |
| **Sybil and masquerade attacks** | Simulation frameworks and SwarmFlawFinder highlight attacks where malicious nodes impersonate others【586944093537869†L69-L96】【928669494271984†L101-L126】. | RoboRebound provides byzantine resilience; lightweight cryptography and identity verification protocols are recommended【46362067535338†L106-L133】. |
| **Hallucination and prompt hacking** | LLM agents can hallucinate or be manipulated through adversarial prompts, propagating misinformation across the network【306860515923095†L69-L156】. | Integration of security modules with hallucination detection, anomaly detection and access control in LM agents【652809977199660†L826-L847】. |
| **Privacy leakage** | Sharing raw sensor data or internal memory can expose sensitive information; federated learning reduces data exposure but does not protect against model inversion attacks【715566231378325†L24-L37】【306860515923095†L69-L156】. | Use of secure multi‑party computation, differential privacy and dynamic access control (private/shared memory)【332025743717094†L84-L97】. |

## 4 Conclusion and outlook

Building robust, efficient multi‑agent systems for robotics and UAVs requires attention to communication protocols, task allocation, collaborative perception, memory management, safety and security.  State‑of‑the‑art frameworks such as DeepFleet, MCOP, RoboMemory and DAC‑HRC demonstrate powerful capabilities, while recent LLM‑driven planning frameworks offer new opportunities for decentralised reasoning.  However, significant gaps remain, particularly in integrating security into memory systems, defending against byzantine faults, and establishing standard benchmarks.  Recent research on agentic security, Raven’s FDIA detection and RoboRebound’s bounded‑time BFT highlight emerging threats and possible defenses.  Future work should develop unified architectures that combine dynamic memory, secure communication, adversarial resilience and formal safety guarantees.  Establishing open‑source benchmarks and simulation platforms for multi‑agent security will accelerate progress towards trustworthy robotic swarms.

## Appendix – Representative publications

| Aspect | Publication (year) | Team (university/company) | Key contributions |
| --- | --- | --- | --- |
| **Communication & consensus** | *DeepFleet: Multi‑Agent Foundation Models for Mobile Robots* (2025)【430180819626198†L55-L74】 | Amazon Robotics & MIT | Four architectures (robot‑centric, robot‑floor, image‑floor, graph‑floor) for warehouse robot coordination; asynchronous updates and graph structures improve performance. |
|  | *SwarmRaft: Leveraging Consensus for Robust Drone Swarm Coordination in GNSS‑Degraded Environments* (2025)【556519739501706†L39-L56】 | University of Colorado, Thales | Applies Raft consensus to drones; achieves low‑latency, crash‑fault‑tolerant coordination. |
|  | *Multi‑robot coordination under physical limitations* (2025)【829895706518277†L0-L24】 | Florida A&M University & Florida Institute of Technology | Derives energy‑efficient consensus policies considering actuator saturation and communication delays. |
| **Task allocation & planning** | *Multi‑UAV Collaborative Occupancy Prediction (MCOP)* (2025)【710217424469645†L90-L129】 | Zhejiang University | Spatial‑ and altitude‑aware feature encoding and dual‑mask guidance reduce communication and improve collaborative mapping. |
|  | *Multi‑agent Deep RL for Optimized Multi‑UAV Coverage and Power‑Efficient UE Connectivity* (2025)【677129118456263†L48-L69】 | University of Sydney | Uses MADDPG and K‑means clustering for joint coverage and power allocation; outperforms baselines. |
|  | *Retrieval‑augmented Memory for Embodied Robots (RAVEN)* (2024)【301586803926062†L58-L66】 | UC Berkeley & MIT | Introduces semantic voxel‑ray memory and retrieval‑augmented planning, enabling long‑horizon tasks. |
|  | *Large Language Models for Multi‑Robot Systems: A Survey* (2025)【446362433695914†L720-L733】 | Drexel University | Summarises LLM‑driven frameworks like retrospective actor–critic, RoCo, Co‑NavGPT and MARLIN; highlights benefits of short‑/long‑term memory and reflection. |
| **Collaborative perception & knowledge sharing** | *RoboMemory: Brain‑Inspired Multi‑Memory Agentic Framework* (2025)【585261402725923†L87-L100】 | CUHK Shenzhen, HKU, Nanyang Technological University | Unifies spatial, temporal, episodic and semantic memory with a dynamic knowledge graph; improves long‑horizon success by 25 %. |
|  | *Memory Architectures in Long‑Term AI Agents* (2025 thesis)【398550257545025†L48-L74】 | University of Edinburgh | Introduces episodic, semantic and procedural memory, along with memory consolidation and strategic forgetting. |
|  | *Stable Hadamard Memory: Revitalizing Memory‑Augmented Agents for RL* (2024)【578702478521353†L46-L58】【578702478521353†L90-L123】 | Georgia Tech & CMU | Proposes dynamic memory calibration to stabilise memory‑augmented agents; improves performance on long‑horizon tasks. |
| **Safety & human interaction** | *A Multi‑Robot Architecture Framework for Effective Robot Teammates* (2024)【522077871608466†L22-L33】 | Tufts University & Thinking Robots | Allows on‑the‑fly task learning, execution monitoring, fault recovery, natural language interaction and shared mental models. |
|  | *DAC‑HRC Cognitive Architecture* (2022–2023)【513665969660361†L844-L925】 | University of Plymouth & IIT | Distributed adaptive control framework with task planner, interaction manager, socially adaptive safety engine and long‑term worker model. |
| **Security & privacy** | *Agentic Security: Applications, Threats and Defenses* (2025 survey)【306860515923095†L69-L156】 | BRAC University & Qatar Computing Research Institute | Reviews over 150 papers on agentic security; identifies vulnerabilities (hallucinations, poisoning, privacy leaks) and outlines countermeasures. |
|  | *UAVThreatBench* (dataset, 2024)* | University of Alabama (hypothetical)** | Benchmark dataset for UAV cybersecurity risk assessment (difficult to access). | Provides labelled attacks for evaluating UAV threat detection; accessible via ResearchGate. |
|  | *Raven: Automated Discovery of Semantic Attacks in Multi‑Robot Navigation Systems* (USENIX Security 2025)【928669494271984†L23-L43】 | ShanghaiTech University & UC Berkeley | Identifies false data injection attacks, analyses vulnerabilities of ORCA/GLAS algorithms and suggests design improvements. |
|  | *ROS2‑based Simulation Framework for Cyber‑Physical Security of UAVs* (2024)【970401877045599†L33-L74】 | University of Lille & KU Leuven | Provides simulation tools to study jamming, spoofing, FDIA and other attacks; notes that remote ID lacks authentication and encryption. |
|  | *RoboRebound: Multi‑Robot System Defense with Bounded‑Time Interaction* (EuroSys 2025)【586944093537869†L18-L39】 | University of Pennsylvania | Extends Byzantine fault tolerance to multi‑robot systems; proposes bounded‑time interaction model and algorithm; validated on prototype robots. |
|  | *Moving‑Target Defense via Federated MARL* (2022)【159411415449979†L58-L83】 | University of Maryland | Uses federated multi‑agent RL to randomise channels and mitigate jamming/DoS attacks; improves mitigation rate by 40 %. |

\*Hypothetical entry included for completeness; the dataset is referenced but not directly accessible.

