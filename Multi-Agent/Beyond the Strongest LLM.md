# 📚 Literature Review — Per-Paper Summary

## 🧭 1) Bibliographic Information

* **Title:** Beyond the Strongest LLM: Multi-Turn Multi-Agent Orchestration vs. Single LLMs on Benchmarks
* **Authors & Affiliations:** Aaron Xuxiang Tian (Independent Researcher); Ruofan Zhang (Independent Researcher); Jiayao Tang (Arizona State University); Young Min Cho (University of Pennsylvania); Xueqian Li (Gradient); Qiang Yi (Gradient); Ji Wang (Gradient); Zhunping Zhang (Nanyang Technological University); Danrui Qi (Simon Fraser University); Zekun Li (University of California, Santa Barbara); Xingyu Xiang (Dropbox); Sharath Chandra Guntuku (University of Pennsylvania); Lyle Ungar (University of Pennsylvania); Tianyu Shi (Gradient; co-corresponding); Chi Wang (Google DeepMind; corresponding)
* **Year / Venue / DOI:** 2025 / arXiv (cs.AI) / — not specified —
* **URL(s):** [https://arxiv.org/abs/2509.23537v2](https://arxiv.org/abs/2509.23537v2)
* **Keywords (5–10):** multi-agent orchestration; consensus; voting; coordination strategies; herding; self-voting; GPQA-Diamond; IFEval; MuSR; LLM benchmarking
* **Field / Subfield:** Artificial Intelligence / Multi-agent LLM coordination and evaluation
* **Reference key (BibTeX/ID):** — not specified —

### 1.a Figure/Table Inventory (global list)

* [ ] “Table 1: Accuracy (%) on three benchmarks.” → 6. Results and Analysis → “Main comparison: orchestration vs. single-LLM baselines”
* [ ] “Figure 1: Effect of coordination strategies on GPQA-Diamond.” → 6. Results and Analysis → “Ablation outcomes on identity disclosure and visible tallies”
* [ ] “Table 2: Exact McNemar’s test p-values comparing orchestration with each LLM.” → 6. Results and Analysis → “Significance testing of paired accuracy differences”
* [ ] “Table 3: Coverage of correct answers by participating agents under orchestration.” → 6. Results and Analysis → “Headroom analysis: cases where consensus failed despite correct agents”

---

## 🧩 2) Research Context (Background)

**Problem / Gap**

* Single LLMs excel unevenly across tasks; it is unclear whether multi-turn, multi-agent orchestration consistently beats the strongest single model and how coordination design choices affect consensus quality.

**Motivation / Significance**

* Understanding when and how agent orchestration yields reliable gains informs practical systems that combine heterogeneous models for robust performance across diverse benchmarks.

**Related work (2–6 items)**

* Multi-agent debate/consensus mechanisms and orchestration frameworks (e.g., voting vs. consensus debate; orchestration via RL or blockchain-based robustness).
* Benchmarks for advanced reasoning and instruction following (GPQA-Diamond, IFEval, MuSR).
* Studies on sycophancy, herding, and coordination pathologies in agent collectives.

**Limitations in prior art**

* Sparse, systematic comparisons against *strong* single-LLM baselines.
* Limited ablations isolating the effects of identity disclosure and vote-tally visibility on group dynamics.

#### Figures/Tables placed here

* None.

---

## 💡 3) Research Question(s) or Hypotheses

* **Primary question:** Does multi-turn multi-agent orchestration match or surpass the strongest single LLM across diverse benchmarks?
* **Secondary objectives:** (i) Quantify how identity disclosure and vote-tally visibility shape self-voting, tie rates, and convergence; (ii) Measure the headroom between best-achievable outcomes (given agents’ candidate answers) and realized consensus.
* **Explicit hypotheses (if any):** — not specified —
* **Success/falsification criteria:** Higher or comparable accuracy versus the strongest single model; statistically significant differences via paired tests; clear behavioral shifts under strategy ablations.

#### Figures/Tables placed here

* None.

---

## ⚙️ 4) Methodology (Core of the Paper)

**Study type:** Empirical benchmarking with coordination-strategy ablations.
**Design:** Orchestration of four heterogeneous LLM agents over multiple turns with exclusive action choices (propose *or* vote), dynamic restarts on new proposals, and consensus via majority voting; final synthesis by the winning agent.

### 4.1 Framework / Model Overview

* Orchestrator coordinates agents ({a_i}) with per-agent state and a global coordination state.
* **Exclusive action constraint:** At each step, an agent must **either** (i) cast a vote for an existing answer *or* (ii) generate a new answer—never both in the same step (formalized with mutual exclusivity conditions).
* **Dynamic restart:** Any new answer invalidates current votes and triggers reevaluation with the updated candidate set.
* **Consensus:** Majority vote over the latest answer set selects the “winning” agent; ties broken by configured order; the winner synthesizes the final response.
* **Equations & formalism:**

  * Valid response constraint enforcing one of {vote, new_answer}.
  * Restart signal definition when any (N(a_i, c)) occurs.
  * Winner selection via ( \arg\max ) over valid votes with indicators invalidating votes pending restart.
  * (Purpose: define coordination rules, ensure exclusivity, and formalize consensus selection.)

### 4.2 Data / Environment

* **Benchmarks:** GPQA-Diamond (graduate-level, Google-proof multiple choice), IFEval (instruction following), MuSR (narrative reasoning; multiple choice).
* **Agents/Models:** Gemini 2.5 Pro, GPT-5, Grok 4, Claude Sonnet 4 (heterogeneous ensemble).
* **Ablations (on GPQA-Diamond):**

  * **Identity disclosure:** Anonymous vs. Identified Voting.
  * **Vote-tally visibility:** Hidden vs. Visible Tally (who voted for which answer + reasons).
* **Agent memory:** Agents are memory-less across tasks (names anonymization in Anonymous setting renders voting identity-agnostic).

### 4.3 Variables & Measures

| Variable                  | Type       | Description                                                                      |
| ------------------------- | ---------- | -------------------------------------------------------------------------------- |
| Accuracy                  | Outcome    | Proportion of tasks solved correctly (per target: orchestration vs. single-LLM). |
| Self-Voting Rate          | Behavioral | Fraction of votes an agent casts for its own answer.                             |
| First-voted Selected Rate | Behavioral | Tasks where the first-voted answer becomes the final consensus.                  |
| Consensus Tie Rate        | Behavioral | Fraction of tasks ending with no majority.                                       |
| Best-achievable coverage  | Diagnostic | Fraction where ≥1 agent produced a correct answer (headroom).                    |

### 4.4 Evaluation Metrics

* Primary: Accuracy (%).
* Statistical testing: Exact McNemar’s test for paired differences between orchestration and each single-LLM.

### 4.5 Tools / Software / Hardware

* LLM APIs for Gemini 2.5 Pro, GPT-5, Grok 4, Claude Sonnet 4; custom orchestration framework with **vote** and **new_answer** tools.
* Hardware/compute details: — not specified —.

#### Figures/Tables placed here

* None.

---

## 🧠 5) Key Contributions (as claimed by authors)

* C1: Systematic evaluation of a multi-turn, multi-agent orchestration framework against strong single-LLM baselines across GPQA-Diamond, IFEval, and MuSR. *(Empirical/System)*
* C2: Controlled ablations isolating identity disclosure and vote-tally visibility, revealing their quantitative impact on self-voting, tie rates, and convergence dynamics. *(Empirical)*
* C3: Headroom analysis showing gaps where at least one agent was correct but consensus failed, highlighting coordination failure modes and opportunities. *(Empirical/Analysis)*

#### Figures/Tables placed here

* None.

---

## 📈 6) Results and Analysis

**Main findings (2–4 bullets)**

* Orchestration **matches or exceeds** the strongest single LLM on two of three benchmarks and yields the best **average** accuracy (81.2%).
* On GPQA-Diamond, at least one agent produced the correct answer in **95.5%** of cases, yet orchestration achieved **87.4%**, indicating substantial headroom.
* Identity disclosure **increases self-voting** and **tie rates**; visible tallies **amplify herding**, raising the chance that the first-voted answer becomes final.

**Comparisons (baselines & fairness)**

* **Table 1:** Orchestration vs. single models (Accuracy %):

  * GPQA-Diamond: Orchestration 87.4 vs. best single 85.9 (Gemini).
  * IFEval: Orchestration 88.0 vs. best single 87.4 (GPT-5).
  * MuSR: Orchestration 68.3 vs. best single 69.6 (Gemini).
  * Average: Orchestration 81.2 vs. GPT-5 80.5; Grok 4 79.2; Gemini 73.8; Claude 64.9.

**Statistical validity & robustness**

* **Table 2:** McNemar’s tests report paired p-values; many differences are small/non-significant vs. strongest model on some datasets, but orchestration significantly outperforms weaker models in places (e.g., vs. Claude on multiple benchmarks).

**Limitations from results**

* **Table 3 / text:** Even when ≥1 (or ≥2) agents are correct, consensus can converge incorrectly (e.g., GPQA-Diamond: among errors, 64% had ≥1 correct agent; in 31% of those, ≥2 correct agents).
* Visible tallies can improve convergence speed but risk **premature consensus** via herding; identified voting skews selection toward dominant models and raises ties.

### Key Numbers (optional)

| Metric                      |          Proposed | Best Baseline | Δ (abs) | Δ (%) | Notes                           |
| --------------------------- | ----------------: | ------------: | ------: | ----: | ------------------------------- |
| Accuracy (GPQA-Diamond)     |              87.4 |          85.9 |    +1.5 |  +1.7 | Beats Gemini 2.5 Pro            |
| Accuracy (IFEval)           |              88.0 |          87.4 |    +0.6 |  +0.7 | Beats GPT-5                     |
| Accuracy (MuSR)             |              68.3 |          69.6 |    −1.3 |  −1.9 | Slightly below Gemini           |
| First-voted Selected (GPQA) |    67.8 (Visible) | 54.1 (Hidden) |   +13.7 |     — | Herding under visible tallies   |
| Self-Voting (GPT-5, GPQA)   | 88.4 (Identified) |   81.0 (Anon) |    +7.4 |     — | Identity disclosure effect      |
| Tie Rate (GPQA)             | 23.2 (Identified) |   14.1 (Anon) |    +9.1 |     — | More ties with identities shown |

#### Figures/Tables placed here

* “Table 1: Accuracy (%) on three benchmarks.”
* “Figure 1: Effect of coordination strategies on GPQA-Diamond.”
* “Table 2: Exact McNemar’s test p-values…”
* “Table 3: Coverage of correct answers by participating agents…”

---

## 🔍 7) Discussion / Insights

* **Field advancement:** Demonstrates that careful multi-agent orchestration can rival top single models while exposing design-sensitive dynamics (identity, visibility) that materially shape outcomes.
* **Practical vs. statistical significance:** Small absolute accuracy gains over the strongest single model may be practically meaningful given stability across datasets; significance varies by pairwise comparison.
* **Alternative explanations / threats to validity:** Gains may partially stem from model complementarity specific to the chosen four models/benchmarks; orchestration rules and tie-breaking may bias selections.
* **Ethical/societal implications:** Transparency about identity/visibility can alter group behavior; design choices might entrench dominance of particular models or spur herding toward incorrect answers.

#### Figures/Tables placed here

* None.

---

## 🧾 8) Critical Evaluation (Your Review)

### 8.1 Ratings (1–5)

| Criterion                      | Score | Comment                                                                             |
| ------------------------------ | :---: | ----------------------------------------------------------------------------------- |
| Novelty                        |   4   | Systematic head-to-head vs. strong single-LLMs + targeted ablations.                |
| Technical soundness            |   4   | Clear formalism; appropriate paired tests; well-defined behavioral metrics.         |
| Clarity / organization         |   4   | Clean separation of benchmarks vs. ablations; figures/tables support claims.        |
| Reproducibility / transparency |   3   | Protocols described; exact prompts/agent configs and compute details are limited.   |
| Impact / significance          |   4   | Timely evidence for orchestration as a viable alternative to single-model reliance. |
| Ethics / safety                |   3   | Notes on herding/self-voting; broader risk analysis could be deeper.                |
| Overall impression             |   4   | Strong, careful empirical study with actionable design insights.                    |

### 8.2 Strengths

* Cross-benchmark comparison against strong single models; not just weaker baselines.
* Clear, minimal orchestration protocol enabling precise ablations.
* Behavioral metrics (self-voting, tie rate, first-vote lock-in) reveal actionable levers.

### 8.3 Weaknesses / Threats

* Limited reporting on prompts, temperature, stopping rules, and system instructions per model.
* Generalization beyond the three benchmarks and the four chosen models is untested.
* Tie-breaking and restart heuristics might influence outcomes; alternative consensus rules not exhaustively compared.

### 8.4 Replicability checklist

* [ ] Code link & license
* [ ] Data availability (task lists / splits used)
* [ ] Configs/seeds/hyperparameters (sampling params, prompts)
* [ ] Environment versions (model API dates, framework versions)
* [ ] Scripts to reproduce figures/tables

---

## 🚀 9) Key Takeaways & Future Directions

* **One-sentence takeaway:** Multi-turn multi-agent orchestration can meet or exceed top single-LLM performance, but coordination design (identity/tally visibility) crucially shapes correctness, ties, and herding.
* **Top 3 insights:** 1) Orchestration achieves competitive or superior accuracy across benchmarks; 2) Identity disclosure raises self-voting and tie rates; 3) Visible tallies accelerate—and sometimes mislead—consensus via herding.
* **Open questions:** How do alternative consensus rules (e.g., weighted voting, confidence calibration, Bayesian aggregation) compare? Can we algorithmically detect and mitigate premature herding? What is the effect of scaling agent diversity or adding smaller specialists?
* **Follow-up actions:** Reproduce with released prompts/configs; test additional voting/aggregation schemes; probe robustness across more datasets and domain-specialized agents; quantify cost/latency trade-offs.

---
