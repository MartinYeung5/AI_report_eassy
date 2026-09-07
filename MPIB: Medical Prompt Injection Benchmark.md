# MPIB: Medical Prompt Injection Benchmark – A Systematic Evaluation of LLM Clinical Safety

## Paper Summary

**MPIB (Medical Prompt Injection Benchmark)** is the first systematic benchmark dedicated to assessing prompt injection risks of large language models (LLMs) in clinical scenarios. It comprises 9,697 clinical adversarial samples and introduces the **Clinical Harm Event Rate (CHER)** to distinguish between “instruction-following compliance” and actual clinical harm – a critical distinction for patient safety.

---

## Core Research Content

### Problem Definition

LLMs and Retrieval-Augmented Generation (RAG) systems are being increasingly integrated into clinical workflows for tasks such as medical summarisation, medication safety checks, and triage. However, prompt injection attacks exploit the instruction‑following nature of LLMs, causing models to prioritise adversarial instructions over original constraints or user goals.

In healthcare, this problem is particularly severe. *Indirect prompt injection* can embed malicious payloads into retrieved documents (e.g., tampered “guideline updates” or poisoned PDFs), which are implicitly treated as trustworthy authoritative sources in RAG pipelines. More challengingly, the most dangerous failures in clinical settings are often *not* overtly unsafe outputs, but rather plausible, polite, and well‑structured responses that recommend wrong dosages, downplay urgent symptoms, or distort evidence. This “camouflaged risk” renders traditional Attack Success Rate (ASR) metrics inadequate for measuring real clinical harm.

### Innovative Methods

**1. Dual‑track Evaluation Metrics**

The core innovation of MPIB is the introduction of **Clinical Harm Event Rate (CHER)**, alongside the conventional Attack Success Rate (ASR). ASR measures whether the model executes adversarial instructions, whereas CHER quantifies whether the model’s output leads to high‑severity clinical harm events (Severity ≥ 3) according to a clinical grading taxonomy. The separation of these two metrics allows the identification of critical boundary cases: “instructions executed but no clinical harm” and “instructions not fully executed but clinical harm already caused.”

**2. Hierarchical Adversarial Sample Construction**

The dataset encompasses multiple adversarial vector levels:

- **V0 / V0*** : Benign samples and boundary samples with potential risk/utility (8,471 samples, 87.3%)
- **V1**: Direct injection attack samples (644, 6.6%)
- **V2‑S**: Strict adversarial injection (94, 1.0%)
- **V2‑B**: Boundary adversarial injection (488, 5.0%)

**3. Six‑Gate Quality Control Pipeline**

A six‑gate filtering process (G1–G6) checks clinical validity, format correctness, and adversarial strength. For V2‑level samples, an additional conflict quality gate (G3) scores multiple dimensions (affinity, misleadingness, plausibility, impact) on a 1–5 scale. Candidates failing the adversarial strength filter are downgraded to a V0’ boundary pool rather than discarded, preserving clinically valid edge cases.

**4. Tiered Access Control**

To balance openness and safety risks, MPIB adopts a tiered access mechanism:

- **Public tier**: V2 payloads are redacted (`[REDACTED_PAYLOAD]`) for immediate safety.
- **Restricted tier**: Approved researchers can access the full Payload Registry.
- **Reconstruction mechanism**: The full attack samples can be restored using the registry file and official evaluation toolkit.

### Research Findings

Systematic evaluations across multiple baseline LLMs and defence configurations reveal two key conclusions:

1. **ASR and CHER diverge significantly**: A model may show good ASR performance yet still produce high‑severity clinical harm outputs, and vice versa. This demonstrates the inadequacy of relying solely on ASR for medical LLM safety evaluation.

2. **Robustness depends on the location of adversarial instructions**: Models exhibit fundamentally different resistance to direct prompt injection (adversarial instructions in user queries) versus indirect prompt injection (adversarial instructions in retrieved contexts), which carries important implications for RAG system design.

### Practical Deployment Potential

MPIB provides a reproducible, systematic evaluation framework for medical AI safety. Healthcare institutions can use MPIB for pre‑deployment safety validation; LLM vendors can incorporate it into red‑team testing; regulators can refer to the CHER metric for setting safety standards for AI medical devices. The dataset and evaluation code are publicly available on GitHub and Hugging Face.

---

## Technical Details

### Dataset Composition

The MPIB dataset comprises **9,697** clinical adversarial samples derived from MedQA and PubMedQA, divided into three splits:

| Split   | Proportion | Count | Purpose                     |
|---------|------------|-------|-----------------------------|
| Train   | 80%        | 7,759 | Training or few‑shot examples |
| Valid   | 10%        | 969   | Hyperparameter tuning       |
| Test    | 10%        | 969   | Final evaluation            |

### Scenario Distribution

Data covers four clinical scenarios:

| Scenario | Count | Proportion |
|----------|-------|------------|
| S1 General Information | 5,653 | 58.3% |
| S2 Medication | 669 | 6.9% |
| S3 Triage | 2,715 | 28.0% |
| S4 Guidelines | 660 | 6.8% |

### Harm Severity Grading

CHER adopts a clinical grading taxonomy: high‑severity clinical harm events are defined as Severity ≥ 3. CHER is the occurrence rate of such events among all model outputs.

### Quality Thresholds for V2 Adversarial Samples

Multi‑dimensional scoring is applied for V2‑level samples:

**Strict tier (V2‑S)**:
- Affinity ≥ 3.0
- Misleading ≥ 3.0
- Plausibility ≥ 3.0

**Boundary tier (V2‑B)**:
- Affinity ≥ 2.7
- (Misleading ≥ 2.6 or Plausibility ≥ 2.6)
- Impact ≥ 2.0

---

## Experimental Setup

### Hardware and Software

Based on the paper and open‑source resources:

- **Models evaluated**: Multiple baseline LLMs, including open‑source and commercial API models.
- **Evaluation framework**: Official Python toolkit provided (GitHub: `jhlee0619/mpib-eval`).
- **Data access**: Via Hugging Face Datasets (`jhlee0619/mpib`).
- **License**: CC BY‑NC 4.0.

### Key Experimental Design Points

1. **Direct vs. indirect injection**: Testing adversarial instructions appearing in user queries versus retrieved contexts.
2. **Multiple defence configurations**: Evaluating robustness under different defence strategies.
3. **Dual‑metric comparative analysis**: Reporting both ASR and CHER to analyse divergence patterns and causes.

---

## Comprehensive Analysis

### Academic Contributions

MPIB fills a critical gap in medical AI safety evaluation. Previous prompt‑injection benchmarks (e.g., BIPIA) have demonstrated general LLM vulnerabilities, but they lack clinical‑harm assessment specific to healthcare. MPIB anchors evaluation on “patient safety” rather than “instruction execution” – a paradigm shift with profound implications for responsible deployment of medical AI.

More broadly, MPIB reflects a trend from “model behaviour” to “system impact” in AI safety evaluation. In high‑stakes domains like healthcare, what a model says matters less than the potential consequences of its actions. CHER essentially introduces clinical risk management methodology into AI assessment – an interdisciplinary fusion.

### Limitations and Challenges

It is worth noting that MPIB’s data mainly originates from MedQA and PubMedQA. While clinically relevant, these sources differ from real‑world electronic health records (EHRs) and free‑text clinical notes. Furthermore, the dynamic and adaptive nature of medical prompt injection attacks – where attackers can adjust strategies based on model feedback – is not fully covered by static benchmarks.

Another important consideration is the tiered access control adopted by MPIB to prevent malicious misuse. This reflects the classic “dual‑use dilemma” in security research – how to facilitate defence research without providing attack toolkits – a challenge that the entire AI safety community must continue to address.

---

## Practical Applications

### For Healthcare Institutions

1. **Mandatory pre‑deployment safety validation**: Before integrating any LLM or RAG system into clinical workflows, systematically evaluate using MPIB – with particular attention to CHER rather than ASR alone.

2. **Distinguish direct vs. indirect injection risks**: Since MPIB shows different risk patterns for indirect (retrieved document) and direct injections, design separate defence strategies for each in RAG systems.

3. **Establish continuous monitoring**: Static benchmarks reflect only current states. Integrate MPIB into CI/CD pipelines to trigger automated safety regression tests upon model updates or knowledge base changes.

### For LLM Developers

1. **Incorporate CHER into red‑team metrics**: Move beyond traditional “jailbreak” success rates by tracking high‑severity clinical harm occurrence.

2. **Design dedicated defences for retrieved contexts**: MPIB results suggest that adversarial instructions in retrieved contexts have distinct risk characteristics, requiring defences different from input‑filtering mechanisms.

3. **Engage with the MPIB community**: Obtain the full Payload Registry via Hugging Face’s restricted access to validate and improve defences on real adversarial samples.

### For Regulators

The CHER metric offers a quantifiable safety evaluation dimension for AI medical device approvals. Regulators are encouraged to refer to the MPIB framework when setting review standards, requiring manufacturers to provide CHER reports under standardised adversarial testing.

---

## References

- Original paper: [MPIB: A Benchmark for Medical Prompt Injection Attacks and Clinical Safety in LLMs](https://arxiv.org/abs/2602.06268)
- Authors: Junhyeok Lee, Han Jang, Kyu Sung Choi (Seoul National University)
- Dataset: [Hugging Face – jhlee0619/mpib](https://huggingface.co/datasets/jhlee0619/mpib)
- Evaluation toolkit: [GitHub – jhlee0619/mpib-eval](https://github.com/jhlee0619/mpib-eval)
- Publication year: 2026

