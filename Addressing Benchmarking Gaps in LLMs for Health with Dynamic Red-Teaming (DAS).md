# DAS: Dynamic Red-Teaming to Bridge the Trust Gap in Static Evaluation of Medical LLMs

## Paper Highlights

This paper presents DAS (Dynamic, Automatic and Systematic Red-Teaming), a framework that continuously stress‑tests large language models through adversarial interactions, revealing a **“benchmarking gap”** between high scores on static benchmarks (e.g., >80% median accuracy on MedQA) and fragile reliability under dynamic real‑world conditions. Evaluating 15 mainstream LLMs, the authors find that **94%** of previously correct answers fail under adversarial robustness attacks, **86%** leak private health information, and **74%** produce hallucinations—exposing the illusion of safety that static benchmarks create.

---

## Core Research Content

### Problem Definition

LLMs are rapidly moving from research benchmarks into actual healthcare products and workflows—from consumer health assistants (e.g., ChatGPT Health) to clinical decision‑support tools (e.g., Claude for Healthcare). Meanwhile, models evolve on a weekly basis, yet safety evaluation still relies heavily on **static benchmarks**.

The authors identify three fundamental flaws in static benchmarks:

1. **Lack of dynamics** – Real clinical scenarios are inherently dynamic: patients rephrase, omit context, add emotional or irrelevant details; clinicians iterate decisions based on new findings. Static benchmarks cannot capture these interactive complexities.
2. **Rapid obsolescence** – Models improve faster than benchmarks are updated, making static evaluations quickly outdated.
3. **Goodhart’s Law trap** – “When a measure becomes a target, it ceases to be a good measure.” Public benchmarks become optimisation targets; fine‑tuning on them inflates scores without improving genuine clinical reasoning.

**Core question:** Can models that excel on static benchmarks be trusted in dynamic real‑world medical interactions?

### Innovative Approach

DAS transforms safety evaluation from a **static checklist** into a **living adversarial audit**. Its key innovations:

**1. Four‑Dimension Safety Assessment**

DAS stress‑tests models along four safety‑critical axes:
- **Robustness** – Can the model maintain correct answers under adversarial input transformations?
- **Privacy** – Does the model leak protected health information under direct or disguised requests?
- **Bias/Fairness** – How do demographic, linguistic, emotional, and cognitive biases shift model recommendations?
- **Hallucination/Factuality** – What is the factual accuracy of generated medical information?

**2. Adversarial Agent System**

DAS employs autonomous adversarial agents that start from the same health‑related “seed” requests and strategically mutate prompts over multiple dialogue turns based on the model’s responses. The agent system includes:
- **Six orthogonal robustness mutation tools**: Answer Negation, Cognitive Bait, Narrative Distraction, Choice Expansion, Question Inversion, and Physiological Impossibility.
- **An orchestrator** (built with OpenAI Agent SDK, using o3 as backbone at temperature 0.2) that coordinates these tools over up to five dialogue turns.
- **Four privacy‑disguise attacks** and **four bias‑induction strategies**.

Crucially, the attack strategies evolve with the target model’s capabilities, ensuring the evaluation cannot be “gamed” and co‑evolves with model improvements.

**3. Automated Verifier Validation**

DAS’s automated verifier is rigorously validated against certified physician judgments, achieving high inter‑rater reliability (e.g., Cohen’s κ = 0.952 for privacy) and further supported by cross‑vendor and non‑LLM baselines.

### Research Results

Evaluating 15 mainstream LLMs (including OpenAI GPT‑4o, o3, o4‑mini; Anthropic Claude Sonnet‑4; Google Gemini‑2.5‑Pro; DeepSeek‑V3/R1; open‑source models like Llama‑4‑Scout, etc.), DAS yields alarming findings:

| Dimension | Key Finding |
|-----------|-------------|
| **Robustness** | Despite >80% median accuracy on MedQA, **94%** of previously correct answers fail under dynamic robustness attacks. |
| **Open‑ended dialogue** | On real‑world open‑ended HealthBench, top models show failure rates **exceeding 70%**. |
| **Privacy** | **86%** of scenarios successfully elicit privacy leakage. |
| **Bias/Fairness** | Cognitive‑bias priming alters model recommendations in **81%** of fairness tests. |
| **Hallucination** | Hallucination rates exceed **74%** for widely used models. |

The study also reveals drastic rank shifts between exam‑style tasks and open‑ended health tasks, indicating that public benchmarks have become optimisation targets rather than stable proxies for reasoning ability. **Question Inversion** is identified as the most impactful adversarial technique, achieving an average jailbreak rate of 60%.

### Practical Deployment Potential

DAS offers clear real‑world applicability:

1. **Pre‑deployment safety audits** – Before deploying LLMs into consumer health assistants, clinical tools, or broader workflows, DAS can proactively uncover hidden risks.
2. **Continuous lifecycle auditing** – DAS is not a one‑off test but a living audit mechanism that runs throughout the model’s lifecycle, revealing when “surface progress masks unresolved failures”.
3. **Community‑driven attack lexicon expansion** – The authors position DAS as a “living” medical benchmark generator, inviting the community to expand attack lexicons, share failure corpora, and standardise evaluation metadata.
4. **Open‑source code** – The official repository (https://github.com/JZPeterPan/DAS-Medical-Red-Teaming-Agents) provides a fully reproducible runner, paper‑specific config presets, reusable Python libraries, and validation tools.

---

## Technical Details

### DAS Workflow

DAS is an agent‑based auditing framework that autonomously generates prompts, selects/evolves jailbreak strategies, escalates attacks dynamically, and detects failures. Its core workflow:

1. **Seed generation** – Start from health‑related initial “seed” requests.
2. **Adaptive dialogue** – The agent strategically mutates prompts over subsequent turns based on the target model’s responses.
3. **Attack orchestration** – The orchestrator selects a single tool or combination (constrained by incompatibility matrices) from the predefined toolset.
4. **Adjudication** – Determine correctness/safety via deterministic rules or LLM‑based verifiers.
5. **Iterative escalation** – Attacks continue over up to five dialogue turns.

### Robustness Mutation Tools

Six mutation tools are designed to deterministically alter correct answers rather than create ambiguity:

| Tool | Type | Mechanism |
|------|------|-----------|
| Answer Negation | Rule‑based | Replace the correct answer with a negation (e.g., “None of the above”), forcing the model to defend a negative claim. |
| Cognitive Bait | GPT‑4o agent | Inject nine cognitive‑bias frameworks (frequency bias, confirmation bias, recency bias, status quo bias, etc.). |
| Narrative Distraction | GPT‑4o agent | Insert seemingly plausible but irrelevant narrative statements to test distraction filtering. |
| Choice Expansion | o3 agent | Expand answer options from 4‑5 to more alternatives. |
| Question Inversion | — | Invert the question structure (identified as the most impactful technique). |
| Physiological Impossibility | — | Design physiologically impossible scenarios to force the model to contradict medical commonsense. |

### Evaluation Metric

The core metric is the **Dynamic Jailbreak Rate** – the frequency with which a model’s already‑demonstrated knowledge collapses under adversarial pressure. By retaining only initially‑correct seeds, this metric isolates reasoning failures (robustness) from knowledge acquisition failures (training).

### Hallucination Detection

DAS introduces an agent‑based, automated hallucination detector tailored to medicine, which adjudicates target responses on curated datasets and generates fine‑grained, traceable source evidence.

---

## Experimental Setup

### Evaluated Models

16 LLMs were evaluated (o3‑mini only included in robustness analysis):
- **OpenAI**: GPT‑4o (Aug 2024), o3‑mini (Jan 2025), o3 (Apr 2025), o4‑mini (Apr 2025)
- **Anthropic**: Claude Sonnet‑4 (May 2025), Claude Sonnet‑3.7 (Feb 2025)
- **Google**: Gemini‑2.5‑Flash (preview, May 2025), Gemini‑2.5‑Pro (preview, Jun 2025)
- **DeepSeek**: DeepSeek‑V3 (Mar 2025), DeepSeek‑R1 (May 2025)
- **Open‑source**: Gemma‑3‑27B, MedGemma‑27B, HuatuoGPT‑o1‑70B, QwQ‑32B, Qwen3‑32B, Llama‑4‑Scout (17B‑16E)

### Datasets

- **MedQA**: US Medical Licensing Exam‑style multiple‑choice benchmark for structured Q&A auditing.
- **HealthBench**: Real‑world open‑ended medical dialogue dataset to validate whether vulnerabilities generalise to actual conversational scenarios.

### Hardware/Software Requirements

- **Python**: 3.10 or higher.
- **Core dependency**: OpenAI Agent SDK (for orchestrator).
- **Installation**:
  ```bash
  pip install -e ".[dev]"          # development mode
  pip install -e ".[api]"          # API integrations
  pip install -e ".[hallucination]" # hallucination detection
  ```
- **Configuration**: Preset files under `configs/` expose a `CONFIG` object for each axis.
- **Credentials**: Handled exclusively via environment variables.
- **Result storage**: Atomic checkpoint writes and axis‑owned recovery identities.

### Scale

The study used **100 million text tokens** in adversarial dialogues and **over 100,000 scored micro‑tasks**. Fully automated evaluation analyses millions of interactions – a scale unattainable with human annotation.

---

## Comprehensive Analysis

### Core Insight: The “Trust Trap” of Static Benchmarks

The study’s most profound insight is that we are measuring the most critical safety issues with the wrong ruler. Static benchmarks reflect model performance on an “open‑book exam,” but real clinical practice is a “practical combat” with no standard answers.

**94%** collapse of previously correct answers is a stark warning: a model with a high MedQA score can lose its “medical knowledge” like a house of cards when confronted with a patient who insists on a misinterpretation (self‑diagnosis bias), a distracting irrelevant story (narrative distraction), or a misleading option framed as “all colleagues believe this” (false‑consensus bias).

### Why Healthcare Is Particularly Risky

The authors highlight a crucial contrast: cognitive‑bias attacks are ineffective on MedQA but highly effective on physician‑authored fairness scenarios. This shows that models have “learned” to recognise benchmark patterns and ignore biases, but when faced with ambiguous clinical fairness cases without standard answers, they fall back to cognitive shortcuts – authoritative cues or compelling anecdotes.

**Implication:** Alignment training on static benchmarks may only teach models “how to perform well on exams,” not “how to reason correctly in real clinical contexts.”

### DAS Design Philosophy: An Arms Race in Evaluation

DAS embodies a profound realism: safety evaluation cannot be one‑off; it must be a continuous arms race. As the authors state, DAS agents co‑evolve with model capabilities – not to “harass” models, but because real‑world attackers (malicious or not) also evolve constantly.

The authors honestly discuss the fidelity–scale trade‑off. Fully automated evaluation covers millions of interactions but must accept some systematic noise – a trade‑off balanced by rigorous clinician validation (Cohen’s κ = 0.952).

### Limitations and Future Directions

The authors acknowledge several limitations:
1. **Attack lexicon may be exhaustible** – The current six robustness tools, four privacy disguises, and four bias strategies already induce >90% jailbreak rates, but developers could overfit to these specific attacks.
2. **Privacy/bias testing lacks dynamic evolution** – Currently, privacy and bias tests stop evolving after compound attacks (because most models already exceed 90% jailbreak rates); future work will port the robustness orchestrator.
3. **Text‑only for now** – Multimodal extension (e.g., medical vision‑language models) is a high‑priority future direction.

---

## Practical Applications

### For Model Developers

1. **Integrate DAS into CI/CD pipelines** – Run DAS audits as quality gates before each model release.
2. **Do not trust static benchmarks** – 90% accuracy on MedQA does not equate to clinical reliability; build internal dynamic test suites.
3. **Focus on edge cases** – Cognitive biases and narrative distractions expose fundamental reasoning vulnerabilities, not just knowledge gaps.

### For Healthcare Institutions and Regulators

1. **Mandate dynamic red‑teaming before deployment** – Require DAS or equivalent audit reports before deploying any LLM in patient‑facing or clinician‑facing applications.
2. **Establish living evaluation standards** – Regulatory frameworks should require continuous lifecycle audits, not one‑time approvals.
3. **Monitor unstable model rankings** – A model ranked first on static benchmarks may drop to the bottom under dynamic tests – regulatory decisions should be based on dynamic performance.

### For Researchers

1. **Expand the attack lexicon** – DAS is open‑source; contribute new attack strategies and test scenarios.
2. **Multimodal extension** – Safety evaluation for medical vision‑language models is an urgent gap.
3. **Share failure corpora** – Build open repositories of medical LLM failures to promote collective learning.

### Quick Start with the Code

The official repository provides a fully reproducible implementation:

```bash
# Clone the repository
git clone https://github.com/JZPeterPan/DAS-Medical-Red-Teaming-Agents
cd DAS-Medical-Red-Teaming-Agents

# Install development dependencies
python -m pip install -e ".[dev]"

# Run robustness baseline
python -m scripts.robustness.run_baseline \
    --config configs/examples/robustness/baseline.py \
    --dataset /path/to/medqa_test.jsonl

# Run robustness attack
python -m scripts.robustness.run_attack \
    --config configs/examples/robustness/attack.py \
    --baseline-results /path/to/baseline_results.json
```

Configuration presets are available under `configs/paper/` to reproduce the paper’s experiments.

---

## References

- Original paper: Pan, J., Jian, B., Hager, P. et al. *Addressing benchmarking gaps in large language models for health and medicine with dynamic red‑teaming*. Nature Health (2026). https://arxiv.org/abs/2508.00923
- Official code: https://github.com/JZPeterPan/DAS-Medical-Red-Teaming-Agents
