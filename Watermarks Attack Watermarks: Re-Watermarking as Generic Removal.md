# Watermarks Attack Watermarks: Re‑Watermarking as a Generic Removal Strategy

## Paper Highlights

This paper uncovers a counter‑intuitive finding: **watermarks themselves can be the most effective weapon against other watermarks**. The authors show that simply re‑watermarking an already watermarked image (i.e., applying a second watermark) can reduce the original watermark’s bit accuracy by **25–48%** while maintaining high visual quality. Even more concerning, when combined with a lightweight classifier that identifies the watermarking method (accuracy **87.8–95.3%**), an attacker can automate this attack without any knowledge of the original watermarking scheme’s internal details.

---

## Core Research Content

### Problem Definition

With the proliferation of generative AI, invisible watermarks are widely used to label AI‑generated content for provenance tracking and intellectual property protection. However, the robustness of watermarking schemes has always been questioned. Existing attacks often require complex gradient computations, surrogate models, or API access to detectors. This paper asks: **is there an extremely simple, universal, and low‑cost method to remove watermarks?**

The researchers keenly observe that watermark embedding and watermark removal are essentially **isomorphic optimisation problems** – both seek a perceptually acceptable perturbation that flips the detector’s decision. If so, why not use watermarks to attack watermarks?

### Innovative Method

The innovation can be summarised as **“one observation + two contributions”**:

**Core Observation:** A watermark embedder is fundamentally a “micro‑perturbation generator” that naturally possesses the ability to alter existing signals in an image. Therefore, any available watermark encoder can be **weaponised** as an attack tool.

**Contribution 1 – Re‑Watermarking Attack Strategy.** The authors systematically tested **96 combinations** of 8 mainstream watermarking schemes (2 generative‑process and 6 post‑hoc) and derived a surprisingly simple strategy:

- **For post‑hoc watermarks:** applying the *same* method again is most devastating – detection rates drop to near zero, and bit accuracy falls to the baseline of unwatermarked images.
- **For generative‑process watermarks** (e.g., Tree‑Ring, Stable Signature): overwriting with the post‑hoc method **ZoDiac** works best, also reducing detection rates to near zero.
- **Simultaneous ownership forgery:** the attacker’s own watermark can be recovered with high accuracy after overwriting, creating an ambiguity where “the last to watermark is the author.”

**Contribution 2 – Watermark Method Classifier.** The authors fine‑tune a **ConvNeXt‑V2 Large** model as a 9‑class classifier (8 watermark methods + unwatermarked). It identifies the watermark scheme purely from visual features, without needing any detection keys or model internals. Accuracy reaches **95.3%** on DiffusionDB and **87.8%** on the cross‑domain MS‑COCO dataset.

Combined, these two components form a **fully automatic, blind, end‑to‑end attack pipeline (WAW pipeline)**: first classify the watermark method, then execute the corresponding re‑watermarking strategy.

### Research Results

Key experimental results:

| Evaluation Dimension | Result |
|----------------------|--------|
| Bit accuracy reduction | 25% to 48% drop |
| Classifier accuracy (DiffusionDB) | 95.3% |
| Classifier accuracy (MS‑COCO cross‑domain) | 87.8% |
| Post‑hoc watermarks (same‑method overwrite) | TPR → near zero, BA drops to baseline |
| Generative watermarks (ZoDiac overwrite) | TPR → near zero |
| Attacker’s own watermark recovery rate | ≥99% for most schemes |
| Perceptual quality loss | Significantly lower than diffusion‑based baseline attacks |

### Practical Deployment Feasibility

**Extremely high.** This is precisely the most concerning conclusion.

The paper’s threat model assumes an **ordinary internet user** – no specialised skills, no knowledge of internal mechanisms, detection APIs, keys, or gradients – only the ability to obtain an off‑the‑shelf watermark encoder. And such encoders are **widely distributed and actively promoted** to help content creators protect their IP. This means **anyone who can download software can, in theory, carry out this attack**.

Moreover, the authors have **open‑sourced the complete code** on GitHub, further lowering the technical barrier. From a practical standpoint, this is not a question of “possibility” – it is a **real‑world threat**.

---

## Technical Details

### Formalising Re‑Watermarking as an Optimisation Problem

Watermark embedding and removal solve nearly the same optimisation problem:

- **Watermark embedding:** find a perturbation δ (within perceptual constraint ∥δ∥<ε) such that the detector outputs “watermarked”.
- **Watermark removal:** find a perturbation δ (within perceptual constraint ∥δ∥<ε) such that the detector outputs “unwatermarked”.

Both search within the same perceptual boundary for a perturbation that flips the detector. Hence, a watermark encoder is naturally an “adversarial perturbation generator”.

### Strategy Selection

A data‑driven approach was used to determine optimal strategies:

1. For each training image, embed using each victim method v (with random messages, keys, seeds, etc.).
2. For each watermarked image, re‑watermark using each attack method a.
3. Run the victim detector and measure: detection evasion rate (TPR@1%FPR), payload destruction (bit accuracy), and perceptual quality loss.
4. Aggregate results for all (v,a) combinations to build an interference matrix and select the best strategy.

### Classifier Architecture

- **Backbone:** ConvNeXt‑V2 Large, pre‑trained on ImageNet‑22K.
- **Task:** 9‑class classification (8 watermark methods + unwatermarked).
- **Training data:** 500 images per class from DiffusionDB, split 80/10/10.
- **Input resolution:** 512×512.
- **Cross‑domain generalisation:** tested on MS‑COCO, overall accuracy dropped less than 8 percentage points.

### End‑to‑End WAW Pipeline

```
Input image → Classifier predicts watermark method → Execute attack strategy:
├─ If Tree‑Ring / Stable Signature / ZoDiac → Apply ZoDiac
└─ Otherwise (post‑hoc) → Re‑watermark with the same method
```

---

## Experimental Setup

### Datasets
- **DiffusionDB:** high‑quality AI‑generated images.
- **MS‑COCO:** natural images for cross‑domain generalisation testing.

### Watermarking Schemes (8 total)
- **Generative‑process (2):** Tree‑Ring, Stable Signature.
- **Post‑hoc (6):** StegaStamp, RoSteALS, ZoDiac, Pixel Seal, WAM, Video Seal.

### Evaluation Metrics
- **Detection evasion:** TPR@1%FPR (True Positive Rate at 1% False Positive Rate).
- **Payload destruction:** Bit Accuracy (BA) – decoding fails when BA drops below the ECC threshold (typically 90‑92%).
- **Perceptual quality:** Normalised Quality Degradation (NQD), aggregating 8 image quality metrics.

### Hardware/Software Requirements
The paper does not specify exact hardware, but based on the model size (ConvNeXt‑V2 Large) and dataset scale (500 images per class), **a single consumer GPU (e.g., RTX 3090/4090) is sufficient** to reproduce the experiments. Code is fully open‑sourced on GitHub.

### Baseline Comparisons
Compared against the three most effective attacks from the WAVES benchmark: prompt‑based diffusion reconstruction, VAE reconstruction, and iterative diffusion “washing”. Notably, the prompt‑based attack assumes knowledge of the original text prompt – an unrealistic scenario in practice.

---

## Comprehensive Analysis

### Authenticity Assessment

**The core findings are real and reproducible.** Several reasons support this:

- The author team is from **the University of Melbourne**, and the corresponding author, **Benjamin I. P. Rubinstein**, is a tenured professor in AI security and privacy (Ph.D. from Berkeley) with a strong publication record in machine learning security. This is not a fringe or unsubstantiated claim.
- The paper provides **complete open‑source code**, with transparent experimental data. The repository contains the full embedding, attack, and evaluation pipeline, allowing anyone to verify the results.
- The logical chain is clear and self‑consistent: from the observation “embedding ≈ attack”, to systematic experiments, then to classifier automation, and finally to an end‑to‑end attack – each step is supported by data.

### Feasibility in the Real World

**This attack is even easier to execute than the paper suggests.**

The paper assumes the attacker must train a classifier to identify the watermark method. In reality, however, attackers can simply **download the pre‑trained weights** that the authors have already open‑sourced – no training required.

Even more extreme: if an attacker does not care about “precision strikes”, **they do not even need the classifier**. The data show that ZoDiac already causes cross‑method damage to many watermarks. An attacker could simply overwrite every image with ZoDiac – it may not be as effective as targeted attacks for some methods, but it still causes significant disruption.

### Deeper Implications

This paper exposes a **systemic security paradox**: watermarks are promoted as a defence tool, yet precisely because they are accessible, they become the most effective attack tool. It is like “leaving the key at the door for everyone to take, and then being surprised that anyone can open the lock.”

For regulators, the EU AI Act requires labelling of synthetic content. If watermarks can be so easily overwritten and forged, then compliance solutions based solely on watermarking may need re‑evaluation.

For industry, business models that rely on a single watermarking scheme to protect IP face a fundamental challenge. The paper explicitly states: “Re‑watermarking not only suppresses the original signal, but replaces it with a new, valid watermark message” – meaning **copyright attribution can be deliberately made ambiguous**.

### Limitations

The paper also has certain limitations:

- **The classifier misclassifies unwatermarked images relatively often** – in cross‑domain tests, about 1/3 of clean images were falsely predicted as watermarked. The paper considers this acceptable in an attack scenario (a false positive only adds an unnecessary re‑watermark), but from a defensive perspective, it implies a risk of “collateral damage” to clean images.
- **RoSteALS and WAM are more robust victims** – no single attack method can reduce their TPR below 50%. This shows that not all watermarks are equally vulnerable, and some designs still offer defensive value.
- Experiments are primarily on still images; applicability to video watermarks (Video Seal is included but limited) and real‑time streams remains to be explored.

---

## Practical Recommendations

### For Watermark Scheme Developers

1. **Re‑evaluate the “public availability” strategy.** If the encoder is public, it becomes both a protection and an attack tool. Consider keeping the encoder as a secret rather than a public resource.
2. **Design anti‑overwriting mechanisms.** Explore techniques to detect whether another watermark already exists before embedding, and if so, refuse to embed or apply special protective measures.
3. **Joint embedding with multiple heterogeneous schemes.** The paper shows that a single watermark can be easily overwritten, but multiple heterogeneous watermarks may increase the attack difficulty (though coexistence is a challenge – see Petrov et al. on watermark coexistence).
4. **Strengthen ECC error correction.** Since the attack aims to push bit accuracy below the ECC threshold, improving the robustness of ECC can raise the attack cost.

### For Content Platforms / Regulators

1. **Do not treat watermarks as the sole defence.** They should be one layer among many – combine with metadata, blockchain timestamps, content fingerprinting, etc.
2. **Establish watermark “freshness” verification.** If one can detect the temporal order of multiple watermarks, it may help distinguish the original owner from subsequent overwriters.
3. **Focus on “detecting presence” rather than “decoding payload.”** The attack mainly damages bit accuracy (i.e., decoding ability). For 0‑bit schemes that only detect presence, the impact may differ. However, the paper also affects detection statistics.

### For Researchers

1. This is an **excellent case study** in security research, illustrating the classic paradox of “defence tools being weaponised.” It can serve as a teaching example in AI security courses.
2. **Future research directions:** explore design principles for “overwrite‑resistant” watermarks; investigate “watermark‑aware” embedding strategies; evaluate the attack’s applicability to other modalities (video, audio, text).

---

## References

- Original paper: [https://arxiv.org/html/2605.16796v1](https://arxiv.org/html/2605.16796v1)
- Open‑source code: [https://github.com/MariaBulychev/Watermarks-Attack-Watermarks](https://github.com/MariaBulychev/Watermarks-Attack-Watermarks)
- Author affiliation: University of Melbourne
