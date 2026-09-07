# Safety and Security of Large Language Models in Healthcare (Nature 2026)

## Summary

This *Nature* review systematically maps safety and security risks of deploying large language models (LLMs) in clinical settings. The authors frame risks across the entire lifecycle of clinical AI systems—from design, data, model, inference, to operational environment—and identify multi‑layer safeguards ranging from core optimisation objectives and knowledge integrity to alignment, human‑AI interaction, and system integration. The central thesis is that safety in medical AI cannot be treated as a one‑time deployment decision but must be managed as continuous operational risk.

---

## Core Research Content

### Problem Definition

LLMs are rapidly advancing in healthcare—from clinical documentation and knowledge retrieval to decision support. However, deployment outpaces the development of formal safety measures, and unapproved “shadow AI” usage is already widespread in hospitals. The paper asks: what are the concrete safety and security risks of LLMs in medicine, and how can they be systematically categorised, assessed, and mitigated?

### Innovative Approach

The paper does not propose a single algorithm but **constructs a systematic risk taxonomy and protection framework**. It classifies risks along three dimensions:

1. **Security Risks** – external malicious attacks, including data poisoning, targeted behavioural manipulation, and **prompt injection** (where hidden instructions are embedded to force harmful outputs).
2. **Model‑Inherent Safety Risks** – intrinsic to LLM operation, most notably **hallucinations** (plausible but incorrect medical information), and excessive sycophancy that reinforces false assumptions.
3. **Human‑AI Interaction Risks** – clinician‑AI interaction patterns that affect decision quality, such as over‑reliance due to confident model outputs (automation bias) or confirmation bias.

Crucially, the risks are mapped to five lifecycle stages: design, data, model, inference, and operational environment.

### Research Findings

As a review, the core output is a **complete risk landscape and mitigation framework**:

- A structured taxonomy (security, model‑inherent, human‑AI interaction) with clinical relevance grading.
- A lifecycle perspective showing that risks persist across all phases, not only at deployment.
- Multi‑layer safeguards: from core optimisation objectives, knowledge integrity, model alignment, to interaction design and system integration.
- Concrete mitigation recommendations: secure development, data curation, systematic evaluation, continuous monitoring, clear institutional responsibilities, and centralised AI security operations centres for cross‑institutional threat detection.
- An interdisciplinary synthesis of evidence from medical AI, cybersecurity, regulatory science, ethics, and behavioural psychology.

### Practical Feasibility

The framework is **highly actionable**. The risk‑lifecycle mapping provides a clear checklist for healthcare institutions—what risks to watch for and what actions to take at each stage of AI adoption. The paper emphasises that safety is not purely technical; it requires coordinated efforts from research, clinical practice, and regulation. It also highlights that many LLM systems are not locally hosted, raising critical issues of data sovereignty and privacy. For hospital administrators, this means evaluating AI from supply‑chain and workflow perspectives.

---

## Technical Details

### Key Technical Concepts

- **Prompt Injection**: An attack where malicious instructions are embedded in user input to bypass safety alignment. In medical imaging, hidden prompts can cause vision‑language models to ignore tumours or other critical findings.
- **Data Poisoning**: Adversarial manipulation of training data to cause systematic failures for specific patient groups or conditions.
- **Hallucinations**: Arise from the statistical nature of LLMs—they generate the most probable token sequences, not verified medical facts. Statistical “optimal” can diverge from clinical correctness.
- **Automation Bias & Confirmation Bias**: Behavioural risks where clinicians over‑trust AI outputs or selectively accept outputs that match their initial diagnostic hypotheses.

### Lifecycle Risk Mapping

| Phase | Typical Risks |
|-------|---------------|
| **Design** | Misalignment between optimisation objectives and clinical safety goals |
| **Data** | Poisoning, privacy leakage, under‑representation |
| **Model** | Hallucinations, alignment failures, poor robustness |
| **Inference** | Prompt injection, adversarial attacks |
| **Environment** | IT infrastructure vulnerabilities, data exfiltration |

### Protection Layers (from inner to outer)

1. Core optimisation objectives – aligned with clinical safety
2. Knowledge integrity – ensuring factual accuracy
3. Alignment – model behaviour consistent with human values and clinical norms
4. Human‑AI interaction – interface design to mitigate cognitive biases
5. System integration – secure incorporation into clinical workflows

---

## Research Setting

- **Type**: Systematic review article (not an original experimental study). It synthesises evidence from medical AI, cybersecurity, regulatory science, ethics, and behavioural psychology.
- **Team**: Led by researchers from the Else Kröner Fresenius Center for Digital Health at TU Dresden, with multiple German and international collaborators. First author: Dr. Jan Clusmann; Corresponding author: Prof. Jakob N. Kather.
- **Scope**: Covers the full spectrum of LLM risks—from technical vulnerabilities to organisational human‑AI interaction, from malicious attacks to unintentional failures. It also addresses structural challenges: many systems are not locally hosted, and shadow AI usage is already occurring.
- **Hardware/Software**: Not experimental; the framework applies to any healthcare AI deployment, including local vs. cloud‑hosted LLMs, EHR‑integrated tools, specialised medical LLMs, and general‑purpose models used informally.

---

## Comprehensive Analysis

### Academic Contribution

The paper’s key contribution is **systematisation**. Prior work on LLM safety in healthcare was fragmented across disciplines, lacking a unified framework. This paper integrates security, model‑inherent, and interaction risks into one coherent taxonomy, mapped to the AI lifecycle, making risk identification structured and actionable rather than ad‑hoc.

### Paradigm Shift: From Model Problem to System Problem

The deepest insight is that **medical AI safety is not a model‑performance issue but a systemic one**. Risks can originate from poisoned data, injected prompts, insecure infrastructure, over‑trusting clinicians, inadequate governance, or evolving workflows. This implies:

- **Safety is continuous**: Approval is not the end; models update, workflows change, user behaviours evolve—monitoring must persist.
- **Regulatory frameworks need updating**: Traditional medical device regulations address static products, but modern AI software evolves continuously.
- **Responsibilities must be clear**: The paper stresses the need for explicit allocation of accountability within healthcare organisations.

### Implications for Medical AI

Key warnings:

1. **Shadow AI is a real threat** – clinicians already use general‑purpose AI tools for medical tasks without institutional oversight, potentially more risky than formal deployments.
2. **Human factors matter** – technical fixes alone cannot solve automation or confirmation biases; training, interface design, and workflow restructuring are essential.
3. **Safety requires interdisciplinary collaboration** – not just a technical issue; it needs joint efforts from engineers, clinicians, administrators, regulators, and ethicists.

---

## Practical Applications

### For Healthcare Institutions

- **Establish AI governance**: Create a dedicated oversight team for LLM introduction, evaluation, and continuous monitoring. Prohibit unapproved use of general‑purpose AI on patient data.
- **Perform lifecycle risk assessments**: Use the five‑phase framework to systematically evaluate risks before and after deployment.
- **Invest in security operations**: Consider joining or setting up a centralised AI security operations centre for threat detection and response.
- **Address data sovereignty**: For non‑locally hosted LLMs, clarify data flow and control.
- **Training and workflow design**: Train clinicians on AI interaction to reduce automation and confirmation biases. Design interfaces that provide value without unduly influencing clinical judgement.

### For AI Developers

- **Build safety into design**: Not as a post‑hoc patch, but as a core optimisation objective.
- **Conduct red‑teaming**: Perform adversarial testing, especially against prompt injection.
- **Implement continuous monitoring**: Post‑deployment, track output quality, safety, and fairness.
- **Provide transparent reporting**: Clearly communicate capabilities, limitations, and known risks to healthcare partners.

### For Regulators

The paper implies that traditional pre‑market approval pathways are insufficient for continuously evolving AI. Regulators should consider adaptive oversight that covers the full lifecycle, not only initial certification.

---

## References

- Clusmann J, Freyer O, Ostermann M, et al. Safety and security of large language models in healthcare. *Nature*, 2026, 656(8128): 577‑589.
- DOI: 10.1038/s41586‑026‑10687‑1
- Original paper: https://www.nature.com/articles/s41586-026-10687-1
