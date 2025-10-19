
# Multi‑Agent Collaboration & Memory — SoTA, Gaps, Security, and Proposal (Appendix)

**Audience:** Robotics + AI systems researchers, security engineers  
**Scope:** 2020–2025 literature with emphasis on multi‑agent collaboration and the memory subsystem.

---

## 1) Architectural Aspects to Cover (Collaboration + Memory)
- **Coordination & Roles:** supervisor/peer patterns; plan–execute vs. debate/graph‑of‑thought; failure handling & escalation.
- **Communication & Interop:** RPC, pub/sub; **MCP** (tools/resources/prompts); **A2A** (agent↔agent coordination).
- **Memory Taxonomy:** *episodic* (events), *semantic* (facts), *procedural/skills*, *working/short‑term*; storage vs. retrieval vs. reflection.
- **Retrieval Pipeline:** chunking, embeddings, hybrid search (sparse+dense), temporal weighting, summarization/compaction.
- **Consistency & Provenance:** append‑only logs, fact signatures, source lineages, citation binding.
- **Access Control & Privacy:** agent identity, scopes, least privilege, policy enforcement, PII handling.
- **Observability & Evaluation:** traces, tool audit, fact‑level metrics, attack‑under‑load tests (red‑team).

---

## 2) State of the Art by Aspect (representative works)
- **Coordination / Orchestration** — **ReAct** (Princeton/Google, ICLR’23): interleave reasoning and actions for tool‑augmented tasks; improves QA and decision‑making.  
  **AutoGen** (Microsoft, COLM’24): composable conversable agents; open‑source multi‑agent runtime.  
  **MetaGPT** (ICLR’24 oral): SOP‑encoded roles reduce cascading hallucinations.  
  **Graph‑of‑Thoughts** (ETH, 2023): graph‑structured reasoning, quality↑, cost↓.
- **Memory / Information Sharing** — **Generative Agents** (Stanford, 2023): structured episodic memory + reflection for coherent long‑term behavior.  
  **MemGPT** (Berkeley, 2023): OS‑style virtual context with multi‑tier memory for long‑running sessions.  
  **MemLong/LongMem & similar (2023–2024):** external retrieval‑augmented long‑context methods.
- **Retrieval‑Augmented Models** — **REALM** (Google, 2020): joint retriever+LM; **RETRO** (DeepMind, 2022): nearest‑neighbor database boosts LM without scaling params.
- **Tool Use** — **Toolformer** (Meta, 2023): self‑supervised API‑call learning from few demonstrations.
- **Interoperability** — **MCP** (Anthropic, 2024→): standardized tools/resources/prompts surface; **A2A** (Google, 2025): cross‑vendor agent‑to‑agent protocol.

---

## 3) Technical Gaps (esp. Security)
- **Memory/Knowledge Poisoning:** untrusted corpora or peer‑shared ‘facts’ can corrupt retrieval and long‑term memory.
- **Provenance & Integrity:** most SoTA lacks signed facts and verifiable lineage; weak tamper‑evidence.
- **Tool Supply‑Chain Risk:** permissive tools, unclear specs, and lack of attestation lead to over‑privileged execution.
- **Identity & Authorization:** few works define strong agent identities, scoped capabilities, or policy brokers.
- **Consensus & Consistency:** shared memory lacks CRDT/Raft semantics; stale or conflicting beliefs persist.
- **Operational Guarding:** limited runtime monitors, anomaly detection, and attack‑aware evaluation suites.

---

## 4) Cybersecurity Studies & Guidance
- **OWASP GenAI Security / LLM Top‑10 (2024→):** prompt/indirect injection, insecure output handling, data poisoning, supply‑chain, excessive agency; practical mitigations and runbooks.  
- **Backdoor / Stealth Risks:** broader LLM security literature (e.g., “sleeper/backdoor” style attacks) warns about hidden behaviors persisting through fine‑tuning and guardrails—implications for agent memory/tooling.  
- **Industry Protocols:** **MCP** focuses on safe tool/resource exposure; **A2A** emphasizes secure inter‑agent exchange and capability discovery.

---

## 5) Proposed Countermeasures — **SAFER‑MAS** (Secure Attested Fact Exchange for Robust Multi‑Agent Systems)

**Design Priorities**
1. **Zero‑Trust Memory Fabric:** treat all inputs as untrusted; sign & verify every fact at ingress.  
2. **Min‑Privilege Tooling:** explicit capabilities, rate limits, side‑effect fences, human‑in‑the‑loop for high‑impact tools.  
3. **Tamper‑Evident Logs:** append‑only, hash‑chained mission logs; verifiable provenance on retrieval.  
4. **Policy‑as‑Code:** OPA/Rego‑style broker for tool calls & memory reads/writes.  
5. **Consensus for Shared Facts:** optional Raft/HotStuff backed acceptance for cross‑agent ‘shared truths’.  
6. **Active Defense:** red‑team agents, RAG‑poison detectors, LLM‑as‑judge sanity checks, canary prompts.

**Preliminary Solution Sketch**
- **Ingress Gateways** (per API/tool): validate schemas, run anti‑prompt‑injection filters, compute content hashes, attach source URIs and signatures (COSE/Ed25519).  
- **Memory Ledger** (per‑agent shard + federation): append‑only CBOR facts; Merkle roots checkpointed; efficient by‑reference retrieval with citation binding.  
- **Policy Broker:** decisions on `read_fact/write_fact/call_tool` with contextual attributes (agent role, mission stage, risk level).  
- **Tool Attestation & SBOMs:** verify tool images (cosign/SLSA‑like attestations); store SBOM digest alongside tool ID used in any fact.  
- **Observation & Audit:** trace every tool call + retrieved fact; dashboards show provenance graph & risk scoring; automated incident playbooks.

**Evaluation Plan**
- Attack success rate under: prompt injection, RAG poisoning, tool‑spec poisoning.  
- Latency overhead budget < 8% p95 intra‑host; < 20% cross‑host.  
- Tamper‑detection rate, false positives; mission success under attack; blocked dangerous tool attempts.

---

## Publications Table (CSV)
See: **mas_memory_security_publications.csv** (this folder).

