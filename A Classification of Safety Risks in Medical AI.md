
# Medical AI Safety Risk Classification: An In-depth Analysis of the S1–S4 Framework

## Paper Highlights

This paper, authored by Tien Yin Wong (Tsinghua University) and colleagues from Shanghai Jiao Tong University, Stanford University, King's College London, and other institutions, addresses the urgent need for a practical safety evaluation system as medical AI moves from research pilots to routine clinical deployment. The core insight is that **assessing the safety of medical AI cannot rely solely on accuracy or short‑term performance** – AI‑induced harm may emerge months or years after diagnosis or intervention, and it is often difficult to trace back to the original AI output. The proposed S1–S4 framework is not a simple performance score; instead, it assigns different verification, human review, follow‑up, and restriction requirements to each level, shifting the focus from “static model accuracy” to “dynamic deployment‑based risk management”.

---

## Core Research Content

### Problem Definition

Current safety evaluation methods for medical AI suffer from fundamental flaws. Traditional approaches focus on static accuracy and short‑term performance, but the risks of medical AI are characterised by **delayed onset, insidiousness, and irreversibility**. Issues arising from algorithmic bias, data shifts, or contextual mismatches may not become apparent immediately after a single diagnostic or therapeutic intervention; they can surface months or even years later in the form of delayed diagnoses, overtreatment, or progression of chronic diseases. Moreover, the causal chain is often long and complex, making it extremely difficult to retrospectively attribute harm to a specific AI decision. The observation that “no harm has been observed yet is insufficient to prove the system is safe” – this recognition forms the logical foundation of the entire framework.

### Innovative Approach

The research team moves beyond traditional “outcome‑oriented” safety assessments and builds a **process‑oriented, full‑lifecycle, tiered control** framework (S1–S4). Key innovations include:

**First, tightly coupling risk levels with clinical workflows.** The framework draws on concepts from decidability‑based R1–R3 risk taxonomies but translates them into a clinically actionable S1–S4 hierarchy, each with specific verification, review, surveillance, and restriction measures.

**Second, the pivotal principle that “the safety level depends on the deployment context, not the model itself”.** The S‑level is not an inherent property of the AI model, but a safety label assigned to a specific deployment scenario. It depends on the clinical task the AI performs, the workflow into which it is embedded, the patient population it serves, whether longitudinal follow‑up is possible, and whether clinicians can effectively review and intervene before acting on AI outputs.

**Third, upgrading governance from “static outcome verification” to “dynamic end‑to‑end risk management”** – shifting from reactive accountability to proactive tiered pre‑assessment, continuous monitoring, and precise post‑event traceability.

### Research Outcomes

The paper defines a clear four‑tier classification:

| Level | Definition | Representative Use Cases | Primary Safety Strategies |
|-------|------------|--------------------------|----------------------------|
| **S1** | Well‑defined scope, testable; errors are usually detectable promptly | Input/image quality checks, structured data extraction, ECG artifact detection, radiology report drafting, medication reconciliation (decision support) | Pre‑deployment validation, external validation, routine quality control and periodic auditing |
| **S2** | Manageable in an open environment; requires clinician review before any action | AI‑assisted decisions that require clinician sign‑off, treatment recommendations reviewed by a physician | Mandatory human review, logging and drift monitoring, explicit stop rules |
| **S3** | Significant harm may be delayed; safety depends on follow‑up and outcomes | Triage prioritisation, lung nodule or cancer surveillance recommendations, screening applications | Restricted deployment, outcome linking, 30/90/365‑day (and longer if needed) surveillance |
| **S4** | “Red‑line” high‑risk autonomous actions; harm may be severe or irreversible | Autonomous refusal/delay of specialist care, emergency triage, autonomous high‑risk discharge or referral decisions | Generally prohibited for autonomous execution; only allowed under exceptional approval and rigorous independent oversight |

The framework also proposes **multi‑time‑window post‑deployment surveillance**:
- **Short‑term (days to weeks):** rapid capture of adverse events, near‑misses, and high‑risk outputs.
- **Medium‑term (months):** outcome checkpoints tailored to the specific use (e.g., 30, 90, and 365 days).
- **Long‑term (>12 months):** registries, insurance/claims data linkage, and post‑implementation studies.

### Practical Deployment Potential

The framework offers high practical value. The authors use their previous **DeepDR‑LLM** study as a real‑world example:

> DeepDR‑LLM integrates deep learning for images with a large language model to support primary care diabetes management and diabetic retinopathy screening. When the system only provides decision support that is reviewed and edited by clinicians, it can be considered S2. When safety depends on whether patients complete ophthalmology referrals and subsequent outcomes, the deployment context moves towards S3. If AI outputs bypass clinical judgment to refuse referrals, delay specialist care, or directly guide high‑risk therapy, it would approach S4.

This case clearly demonstrates that **the same AI function demands completely different safety requirements depending on the level of human oversight and follow‑up capabilities**. The framework provides clear operational guidance: vendors may propose an initial S‑level with supporting evidence, but the deploying healthcare organisation bears ultimate governance responsibility.

---

## Technical Details

### Grading Logic of the S1–S4 Framework

The four levels can be understood along two dimensions:

**Dimension 1: Time window of harm manifestation**
- S1: Errors are immediately detectable and correctable.
- S2: Harms can be intercepted during clinician review.
- S3: Harms may be delayed and only detectable through long‑term follow‑up.
- S4: Harms may be severe or irreversible, with no recovery once they occur.

**Dimension 2: Degree of human oversight**
- S1: Algorithmic outputs can be used directly (within defined limits).
- S2: Mandatory clinician review before any action.
- S3: Requires linkage to follow‑up and outcome data.
- S4: Generally prohibited for autonomous execution, or requires extraordinary safeguards.

### Dynamic Re‑assessment and Re‑classification Triggers

The framework defines clear triggers for re‑evaluation:

1. Expansion of target population or use case.
2. Loosening of clinician review requirements.
3. Automation of referral, order, or discharge decisions.
4. Significant shifts in data distribution or system performance.
5. Inadequate patient follow‑up or outcome tracking.
6. Clusters of near‑misses or adverse events.
7. Vendor updates that alter the intended use or operational mode.

Clinical leads and AI governance committees should have the authority to narrow the scope, pause, roll back updates, re‑classify, or even prohibit use when necessary.

### Responsibility Allocation

- **Vendor responsibility:** Propose an initial S‑level, provide intended use, human oversight plan, audit logs, monitoring interfaces, and evidence for model updates.
- **Healthcare organisation responsibility:** Bear ultimate governance accountability, confirm the S‑level prior to deployment through a local AI governance committee or equivalent oversight body.

---

## Research Setting

### Article Type
This is a **Perspective** article published in *NEJM AI* (August 2026 issue). It is a conceptual framework proposal rather than an empirical study.

### Background
The paper addresses the pressing need for a unified risk evaluation and management standard as medical AI products rapidly enter real‑world clinical settings. The lack of such a standard has become a critical bottleneck.

### Multi‑institutional Collaboration
The team comprises researchers from:
- Shanghai Jiao Tong University (Bin Sheng)
- Tsinghua University (Tien Yin Wong, Jiaying Song, Jiamin Wu)
- Stanford University (Nigam H. Shah)
- King’s College London (Josip Car)
- Singapore National Eye Centre

### Relationship with Existing Regulatory Frameworks
The authors explicitly state that the S1–S4 framework is intended to **complement, not replace**, existing regulatory systems, including:
- EU AI Act
- IMDRF framework for software as a medical device
- US FDA guidance
- NIST standards
- WHO guidelines

---

## Comprehensive Analysis

### Theoretical Contribution

The deepest insight of the S1–S4 framework is **redefining safety from a “property of the model” to a “property of the deployment”**. This represents a paradigm shift:

Traditional AI safety assessment follows a “model‑centric” approach – train a model, measure accuracy, sensitivity, specificity, and if they pass, deem it safe. The S1–S4 framework, however, shows that **the same model can have completely different safety levels in different deployment contexts**. The DeepDR‑LLM example illustrates this vividly: the same system is S2 when reviewed by clinicians, S3 when outcomes depend on follow‑up, and approaches S4 when autonomous decisions bypass clinical judgment.

Thus, **regulation should not only audit the model itself, but also audit the deployment plan** – who uses it, how, under what conditions, and who can stop it. This effectively shifts AI safety governance from “technical verification” to “system‑level governance”.

### Addressing the Problem of Delayed Harm

The framework provides a systematic response to the unique “delayed harm” problem in medical AI. In conventional AI, risks are usually immediately apparent – a wrong recommendation, a misidentified image – and can be noticed right away. But medical AI errors may manifest as “worsening condition months later” or “cancer progression after a year”, with the original AI output buried in clinical data.

The S1–S3–S4 grading directly addresses this temporal dimension:
- S1 handles “immediately visible” errors.
- S2 handles errors interceptable during clinical review.
- S3 specifically targets “delayed‑onset” harms.
- S4 draws a red line for “irreversible” harms.

The multi‑time‑window surveillance (30, 90, 365 days, and beyond) operationalises this temporal risk management.

### Limitations

It is important to acknowledge that, as a conceptual proposal, the framework has several limitations:

**First, lack of empirical validation.** The framework remains at a theoretical stage and has not yet been systematically tested in large‑scale real‑world clinical settings. Boundary definitions and grey‑area handling require further study.

**Second, implementation costs are not negligible.** Mechanisms such as multi‑time‑window monitoring, outcome data linkage, and local AI governance committees demand substantial IT infrastructure, data governance capabilities, and human resources. Deployment in resource‑limited primary care settings may face practical hurdles.

**Third, the detailed integration with existing regulatory systems is not fully elaborated.** While the paper states that the framework complements rather than replaces current regulations, more operational guidance is needed on how it aligns with FDA, EU, and other regional frameworks.

### Industry Significance

Despite these limitations, the framework’s value is substantial. As medical AI accelerates into clinical practice, the S1–S4 framework fills a critical gap – a unified risk assessment and management standard. It brings into a single clinical governance logic considerations of “when harm appears, when clinicians must review, when follow‑up linkage is required, and who can pause or roll back the system” – precisely the kind of systematic thinking that is most lacking in current clinical AI deployment.

---

## Practical Application Guidance

### For Healthcare Organisations

**1. Establish a local AI governance committee.**  
The paper clearly assigns ultimate governance responsibility to the deploying organisation. Hospitals should establish a multidisciplinary committee comprising clinicians, IT staff, ethicists, and patient safety officers.

**2. Confirm S‑level before deployment.**  
Request the vendor to propose an initial S‑level and supporting evidence, then review and confirm it locally based on your workflow, patient population, and follow‑up capabilities. Do not simply accept the vendor’s initial classification.

**3. Implement tiered management processes.**  
Tailor oversight according to S‑level:
- S1: routine quality control and periodic audits.
- S2: embed mandatory clinician review, log and drift monitoring.
- S3: restrict deployment, link outcomes, establish long‑term follow‑up.
- S4: generally do not deploy; only allow under exceptional approval and rigorous independent oversight.

**4. Set up re‑assessment triggers.**  
Based on the seven trigger conditions listed in the framework, establish automated or semi‑automated re‑evaluation processes, paying special attention to scope expansion, loosening of review, data drift, and other high‑risk signals.

**5. Build multi‑time‑window surveillance.**  
For S3 and above:
- Days to weeks post‑deployment: rapid capture of adverse events and near‑misses.
- 30, 90, 365 days: outcome checkpoints.
- Beyond 12 months: use registries, claims data, or post‑implementation studies for long‑term tracking.

### For AI Vendors

**1. Propose an initial S‑level** with clear justification in product documentation.

**2. Provide comprehensive monitoring interfaces** – audit logs, APIs for real‑time monitoring, and model update records – to facilitate organisational governance.

**3. Clearly define intended use, population, scenarios, and contraindications** to avoid level mismatches due to vague scope.

**4. Establish a version‑update and re‑assessment process** – notify deploying organisations when models change and assist in re‑evaluating the S‑level rather than assuming the new version inherits the old classification.

### For Regulators

The S1–S4 framework offers new perspectives for regulatory thinking. Consider:
- Incorporating deployment context into AI medical device approval, not just the model itself.
- Establishing tiered regulatory oversight, with stricter post‑market surveillance for S3 and S4 applications.
- Promoting institutionalisation of AI governance committees in healthcare organisations.
- Improving mechanisms for delayed‑harm attribution and traceability.

---

## References

- Original paper: *A Classification of Safety Risks in Medical AI*, NEJM AI, 2026. DOI: 10.1056/AIp2600358
- Paper link: https://ai.nejm.org/doi/10.1056/AIp2600358
- Tsinghua University news report (Chinese): https://www.tsinghua.edu.cn/info/1175/127744.htm
- Sohu Health coverage (Chinese): https://m.sohu.com/a/1064913395_359980

---
