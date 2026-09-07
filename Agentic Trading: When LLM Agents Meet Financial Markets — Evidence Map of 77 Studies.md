
# Agentic Trading: When LLM Agents Meet Financial Markets – An Evidence Map of 77 Studies

> *A systematic review by Shenzhen University (May 2026) that audits 77 studies on LLM‑based trading agents. The core finding: while architectural innovations are flourishing, evaluation standards remain fragmented.*

---

## Key Points

This paper reframes LLM trading agents as **expert‑system decision pipelines** rather than black‑box prediction models. Through a systematic audit of 77 studies up to 9 March 2026, it reveals a sobering reality: despite rapid architectural experimentation, **comparable evaluation protocols, execution semantics, and reproducible artifacts remain the major bottlenecks**. The paper contributes three core deliverables: an evidence ledger, a reproducibility audit framework, and a reporting checklist.

---

## Core Research Content

### Problem Definition

What is the fundamental difficulty facing the field of LLM trading agents? The paper gives a sharp answer: **protocol incomparability**.

Although many studies claim that LLM agents achieve superior trading performance, there is no consensus on critical methodological choices such as data splits, cost modelling, and execution semantics. After a rigorous screening, the authors retain **77 studies** from 92 initial candidates, but only **19** satisfy the minimum empirical boundary of "action output + closed‑loop evaluation". The remaining 58 are kept as background and design references.

### Innovative Methods

The methodological novelty lies in three dimensions:

1. **Audit‑oriented evidence map** – Instead of a narrative summary, the paper constructs a codifiable, traceable evidence ledger. Each study is systematically coded along seven dimensions (MR‑1 to MR‑7), covering data & universe construction, temporal splits & data leakage, action semantics, execution & costs, leakage auditing, artifact reproducibility, and multi‑agent & adaptability. Each dimension is labelled as *Mandatory*, *Recommended*, or *Optional* depending on the study type.

2. **Reproducibility levels (R0–R3)** – A four‑level scale is introduced to quantify the verifiability of each study:
   - **R0**: only result descriptions, no code or data
   - **R1**: partial code or data, insufficient for full reproduction
   - **R2**: complete code and data, results can be reproduced under identical conditions
   - **R3**: complete code, data, and independent third‑party validation

3. **Architecture‑Capability‑Activity lens** – This is used as a working analytical lens (not a validated taxonomy) to organise the reviewed literature.

### Research Findings

The core empirical findings are a set of striking statistics:

| Metric | Proportion |
|--------|------------|
| Reported temporally consistent data splits | **2/19 (10.5%)** |
| Reported explicit trading cost models | **1/19 (5.3%)** |
| Recorded stock universe or survivor‑bias handling | **1/19 (5.3%)** |
| Reported execution timing or semantics | **11/19 (57.9%)** |
| Reproducibility level R0 (completely irreproducible) | **15/19 (78.9%)** |
| Achieved R3 reproducibility | **0/19 (0%)** |

These numbers expose a harsh reality: **the vast majority of studies in this field cannot be independently verified or reproduced**. In addition, evidence on adaptation mechanisms such as memory integration (2 studies), meta‑learning adaptation (1), and self‑reflection (1) is also sparse and fragmented.

### Practical Deployment Potential

Although the paper does not propose a new trading algorithm or system, its contributions have significant practical implications:

- The audit framework can be directly adopted by institutions for **internal assessment** of LLM trading systems before deployment.
- The reporting checklist (MR‑1 to MR‑7) can serve as an **industry reference standard** to guide researchers and engineers in designing, evaluating, and reporting LLM trading systems with consistent rigour.
- The systematic analysis of hallucination risks, look‑ahead bias, and latency‑accuracy trade‑offs provides a **practical risk checklist** for real‑world deployment.

---

## Technical Details

### Conceptual Framework of Agentic Trading Systems

The paper reframes an LLM trading agent as a clear **perception‑memory‑reasoning‑action** loop:

1. **Perception** – sensing market information (price data, news, financial reports, etc.)
2. **Memory** – storing and retrieving historical context and experience
3. **Reasoning** – making decisions based on perception and memory
4. **Action** – issuing executable trading instructions
5. **Adaptation** – adjusting behaviour based on market feedback

The paper stresses that this is **an auditable decision pipeline**, not a simple input‑output mapping.

### The Agency Spectrum

A "spectrum of agency" (Figure 1 in the paper) ranges from **prediction models** to **full ecosystems**:

- **Prediction models** (e.g., FinBERT, StockBERT) – perceive market information but lack decision‑making
- **Signal generators** (e.g., AlphaGen, FactorMiner) – add a decision layer but do not execute trades
- **Partial agents** (e.g., FinVis‑GPT without execution) – include memory but lack an action module
- **Trading agents** (e.g., FinAgent, TradingAgents) – close the perception‑memory‑reasoning‑action loop
- **Full ecosystems** (e.g., multi‑agent markets) – add adaptation and coordination capabilities

The paper's main empirical subset **only includes trading agents that cross the "action output + closed‑loop evaluation" boundary** – i.e., from the fourth category upward.

### Reproducibility Level Definitions (R0–R3)

As defined above, the four levels are:

- **R0** – no reproducible materials
- **R1** – partial materials
- **R2** – full materials, same‑environment reproduction possible
- **R3** – full materials plus independent validation

In the 19 primary empirical studies, **78.9% are rated R0, and 0% reach R3**.

### Computational Cost Estimation Formula

The paper proposes a simplified annual API cost formula:

```
Annual API Cost = D × N × C_req
```

Where:
- `D` = number of trading days per year
- `N` = number of decisions per day
- `C_req` = effective cost per request (including model pricing, prompt/completion token budgets, and tool‑call overhead)

The paper emphasises that empirical studies should disclose the model version, pricing snapshot date, token assumptions, tool usage, and the gross margin required to break even.

---

## Research Design

### Literature Search Scope

- **Time window**: 1 January 2022 – 9 March 2026
- **Sources**: ACM Digital Library (including ICAIF), IEEE Xplore, arXiv, SSRN, Google Scholar
- **Initial candidates**: 92 unique records after deduplication
- **Final included**: 77 studies (15 excluded during screening)

### Inclusion Criteria

Two‑level inclusion:

1. **Primary empirical subset (n=19)** – must satisfy both **action output** and **closed‑loop evaluation**
2. **Background and design context (n=58)** – do not meet the above criteria but provide relevant background

### Coding and Analysis

- Pre‑defined coding protocols were used to systematically annotate each study.
- An MR applicability matrix differentiates the seven MR items across study types (end‑to‑end trading, execution algorithms, component studies).
- Iterative calibration was performed via pilot coding of 10 representative papers, with disagreements resolved by author consensus.

---

## Comprehensive Analysis

### 1. The State of the Field – Prosperity with Underlying Flaws

The most startling finding is not the impressive performance claims, but the **severe lack of scholarly rigour**. With 78.9% of studies completely irreproducible (R0) and 95% not reporting trading cost models, many advertised "excess returns" may be nothing more than paper profits.

The authors' diagnosis is incisive: *"Architectural experimentation is expanding rapidly, while comparable evaluation protocols, execution semantics, and reproducible artifacts remain the current bottlenecks"* – we are busy building higher towers while forgetting to lay the foundations.

### 2. A Paradigm Shift in Methodology

The paper's greatest theoretical contribution is the redefinition of LLM trading agents from **black‑box prediction models** to **auditable expert‑system decision pipelines**. This shift has profound implications:

Traditional quantitative models ask "how accurate is the forecast?"; agentic trading systems ask "**is the decision process understandable, verifiable, and traceable?** " The authors explicitly note that *"LLM‑based agents can generate human‑readable rationales and interaction traces; however, such text is not guaranteed to be faithful to the true internal decision process"* – thus, meaningful auditability must rely on documented, time‑stamped tool calls, data snapshots, and execution logs.

### 3. A Warning on the Reproducibility Crisis

The issues uncovered are not unique to financial AI – psychology, medicine, and economics have all faced similar reproducibility crises. However, financial trading has a special urgency: **irreproducible research not only wastes academic resources but can mislead actual investment decisions, causing real economic losses**.

The discussion on look‑ahead bias is particularly insightful. When models inadvertently access future information during training or evaluation, unrealistic performance estimates result. This bias is especially insidious in time‑series settings – it can be introduced through improper cross‑validation (shuffling time order), technical indicators that require future data (e.g., "peeking" moving averages), or text sources that contain post‑event information.

### 4. Hallucination Risk – The Achilles' Heel of Financial AI

The paper systematically analyses the risk of LLM hallucinations in trading. Unlike many other domains, **errors in financial trading cannot be corrected ex post** – market actions may incur losses before any validation is possible. A vivid propagation chain is depicted: a fabricated earnings report can trigger wrong positioning → stop‑loss cascades → over‑positioning based on false confidence.

The paper also notes the limitations of mitigation measures: *"Binding outputs to verifiable sources (FinMCP, HydraRAG) shifts but does not eliminate the risk – source selection itself may be erroneous."* Current fact‑checking mechanisms operate at second‑level latencies, incompatible with millisecond‑level trading decisions.

---

## Practical Applications

### For Researchers

1. **Adopt the MR reporting checklist** – when designing studies on LLM trading systems, proactively check against MR‑1 to MR‑7 to ensure full disclosure of data splits, cost modelling, execution semantics, etc.
2. **Aim for R2 and above** – release complete code, data, and execution environments to enable independent verification.
3. **Strictly guard against look‑ahead bias** – use strict chronological splits and walk‑forward validation; ensure feature engineering and normalisation are fitted only on historical data.
4. **Disclose cost assumptions** – explicitly state model version, pricing snapshot date, token budgets, and break‑even analysis.

### For Practitioners

1. **Critically evaluate claimed "excess returns"** – before adopting any published LLM trading system, check whether it reports cost models, temporally consistent data splits, and execution semantics.
2. **Build internal audit frameworks** – use the MR framework to conduct systematic auditability assessments before deploying LLM trading systems.
3. **Monitor hallucination risks** – establish verification mechanisms for LLM trading outputs, especially for time‑sensitive signals.
4. **Compute economic feasibility** – use the paper's cost formula to assess whether LLM inference costs can be covered by expected profits.

### For Regulators

The paper notes that as agent autonomy increases, **governance and regulatory challenges will become more prominent**: *"The urgent research need is not to grant legal personhood to agents, but to establish auditable records, intervention rules, and sandboxed evaluation mechanisms."* Regulators should consider:

- Requiring LLM trading systems to retain complete decision logs and data provenance
- Establishing sandbox environments for systematic evaluation and stress testing
- Clarifying liability when hallucinated outputs cause client losses

---

## References

- **Original paper**: [Agentic Trading: When LLM Agents Meet Financial Markets](https://arxiv.org/pdf/2605.19337)  
- **Authors**: Yihan Xia, Panpan You, Taotao Wang, Fang Liu, Han Qi, Xiaoxiao Wu, Shengli Zhang (Shenzhen University)  
- **Submitted**: 19 May 2026  
- **arXiv ID**: 2605.19337  
- **Corpus ID**: 288653829
