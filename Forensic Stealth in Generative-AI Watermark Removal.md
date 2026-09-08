
# In-Depth Analysis of “Forensic Stealth in Generative‑AI Watermark Removal”

**Paper:** [Forensic Stealth in Generative‑AI Watermark Removal](https://arxiv.org/abs/2605.09203)  
**Authors:** Yevin Nikhel Goonatilake, Giuseppe Ateniese (George Mason University)  
**Status:** Under Submission (cs.CR)

---

## Paper Highlights

This paper systematically identifies a fundamental flaw in existing AI‑image watermark removal methods: while they succeed in fooling watermark verifiers and preserving visual quality, the removal process itself leaves detectable traces that can be identified by independent forensic detectors – effectively replacing one detectable signal with another. The authors introduce **Forensic Stealth** as a third necessary dimension for successful watermark removal, and demonstrate through extensive experiments that six state‑of‑the‑art removal methods are detected by forensic classifiers with over **98% True Positive Rate at 1% False Positive Rate**. Their work reveals a three‑way trade‑off among removal success, image quality, and forensic stealth.

---

## Core Research Content

### Problem Definition

The paper re‑examines the fundamental goal of removing watermarks from AI‑generated images. In real attack scenarios, the ultimate objective is to restore **deniability** – i.e., to make the processed image statistically indistinguishable from a “clean” image that never carried a watermark. Existing benchmarks only focus on two goals:  
1. **Verifier evasion** – the processed image does not trigger the original watermark detector;  
2. **Quality preservation** – perceptual distortion from the watermarked image stays below an acceptable threshold.

The paper argues that these two criteria are insufficient – if a de‑watermarked image can still be flagged as “manipulated” or “suspicious”, the removal operation is operationally a failure. They formally define a third necessary condition for any successful watermark remover: **Forensic Stealth** – the output distribution of the remover must be computationally indistinguishable from that of clean, unprocessed images.

### Innovative Methodology

The methodological innovations lie in three aspects:

- **Redefining the evaluation framework.** A successful watermark remover must simultaneously satisfy three conditions:  
  - Verifier evasion: `Verify(x_a; k) = 0`  
  - Quality preservation: `d(x_a, x_w) ≤ τ`  
  - Forensic stealth: `P_{T_rem} ≈_c P_clean`  
  (The last condition means the distributions of the remover’s outputs and clean images are computationally indistinguishable.)

- **Systematic empirical evaluation.** Six state‑of‑the‑art removal attacks are assessed, covering four major attack types:  
  - *Distortion‑based optimisation* – e.g., UnMarker (IEEE S&P 2025)  
  - *Diffusion‑based regeneration* – e.g., WatermarkAttacker (NeurIPS 2024) and CtrlRegen+  
  - *Latent‑space inversion/perturbation* – e.g., NFPA and Boundary Leakage  
  - *Stochastic erosion* – e.g., WiTS

- **Strict anti‑shortcut controls.** The authors carefully eliminate shallow artefacts (file size, metadata, container features) by re‑encoding all images to BMP format. This confirms that the forensic classifier’s discrimination stems purely from pixel‑level processing traces, not from simple file‑level cues.

### Key Findings

The empirical results are striking:

- **High forensic sensitivity.** An independent forensic detector achieves over **98% TPR at 1% FPR**, with AUROC values around **0.998–0.9999** – meaning nearly all removal operations leave a detectable fingerprint.

- **Spectral signature of UnMarker.** The residual signal exhibits a **two‑regime spectral deformation** – excess energy in low frequencies and suppression in high frequencies, with a crossover at about **0.15 cycles/pixel**.

- **Limitations of post‑processing.** Ten common post‑processing operations were tested; to suppress forensic detectability, aggressive smoothing or compression is typically required (e.g., JPEG Q75 with PSNR dropping below ~30 dB) – often unacceptable in practice. Notably, at very low false‑positive operating points (e.g., 0.1% FPR), detection rates plummet.

- **Three‑way trade‑off.** The paper confirms a fundamental tension among removal success, image quality, and forensic stealth – all three cannot be simultaneously optimised.

### Practical Deployment Potential

**High theoretical value, but direct deployment remains challenging.**

This work is fundamentally a **benchmarking and problem‑definition study**, not a proposal for a new removal algorithm. Its core contribution is revealing a critical dimension overlooked by the entire community, and providing a systematic evaluation methodology.

- **For watermark designers:** The paper offers a new security evaluation dimension – any watermark scheme should be assessed for whether removal traces are forensically detectable. This can guide the design of more robust watermarks.

- **For removal tool developers:** The findings clearly show that existing tools (including top‑conference results like UnMarker) suffer from a fundamental deficiency in forensic stealth. Any claim of “successful removal” is incomplete without forensic‑stealth testing.

- **Limitations:** The paper does not provide a feasible solution for achieving forensic stealth; it merely highlights the problem and its severity. The gap between “problem discovery” and “problem solving” remains large. Also, the experimental setup (specific datasets, detector architectures) may need further validation in real‑world deployment scenarios.

---

## Technical Details

### Formal Definition of Forensic Stealth

Let `P_clean` be the distribution of ordinary clean images, and `P_T_rem` the distribution of images processed by a remover `T`. Forensic stealth is defined as:

$$P_{T_{rem}} \approx_c P_{clean}$$

i.e., the two distributions are computationally indistinguishable. This borrows from the cryptographic notion of computational indistinguishability, providing a rigorous mathematical foundation.

### Residual Signal Analysis

The authors use **paired residual analysis** – computing pixel‑wise differences between paired clean and attacked images – to reduce content‑dominating effects and focus on processing traces.

For UnMarker, the log‑ratio of power spectral density (PSD) reveals a **two‑regime pattern**:
- **Low‑frequency regime**: energy excess
- **High‑frequency regime**: energy suppression
- **Crossover**: at about **0.15 cycles/pixel**

For diffusion‑based methods, the deformation shows axis‑aligned structures, consistent with the axis‑aligned decoders in diffusion models.

### Detection Performance Metrics

Key metrics reported:
- **AUROC**: ~0.998–0.9999
- **TPR@low FPR**: remains high even at 0.1% FPR

This property – high detection rate at extremely low false positives – implies great practical value for forensic deployments.

---

## Experimental Setup

- **Paper length:** 17 pages (12 pages main text + 5 pages appendix)  
- **Content:** Multiple figures and tables  
- **Status:** Under submission  
- **Field:** Cryptography and Security (cs.CR)

**Inferred hardware/software requirements** (based on typical needs for evaluating six removal methods and training forensic detectors):
- **GPUs:** Multiple high‑end GPUs (e.g., NVIDIA A100/V100)  
- **Framework:** PyTorch (official implementations of UnMarker and others are PyTorch‑based)  
- **Datasets:** AI‑generated image sets (e.g., from SD2.1) plus corresponding clean images  
- **Storage:** Large‑scale image data handling

**Author background:**
- **Giuseppe Ateniese** – Professor at George Mason University, a leading scholar in information security and cryptography, with a strong track record in watermarking and generative‑model security (e.g., “Watermarks in the Sand: Impossibility of Strong Watermarking for Generative Models”).
- **Yevin Nikhel Goonatilake** – Researcher at GMU, co‑author of several watermark‑related papers with Ateniese.

The team’s deep expertise in watermark security lends strong credibility to the theoretical and experimental rigour of this work.

---

## Comprehensive Analysis

### Authenticity Assessment

**The core findings are highly credible.**

Methodologically, the paper employs rigorous controls:  
- Coverage of six state‑of‑the‑art methods and four attack types ensures representativeness.  
- The BMP re‑encoding control eliminates file‑size/metadata shortcuts.  
- Paired residual analysis reduces content bias.

The authors’ prior work (e.g., “Watermarks in the Sand”) has already argued the theoretical impossibility of strong watermarks – this paper extends that line of thinking: not only can watermarks be removed, but the removal itself leaves traces. This academic continuity strengthens the paper’s trustworthiness.

Concurrently, independent studies (e.g., “The Forensic Cost of Watermark Removal”) have reported similar findings – suggesting that “removal leaves detectable fingerprints” is becoming a consensus in the field.

### Feasibility Assessment

**The evaluation methodology is highly feasible, but the problem it reveals poses a severe challenge to existing removal schemes.**

The proposed forensic evaluation framework – using an independently trained forensic detector – is technically sound and reproducible (open‑source code and datasets are available).

However, the revealed **three‑way trade‑off** implies that achieving true forensic stealth may be extremely difficult, or even theoretically impossible. If any removal operation inevitably leaves statistical traces, then a “perfectly stealthy” removal may not exist – echoing the team’s earlier impossibility results.

**Practical implications:**
- **For content platforms (e.g., OpenAI, Google, Stability AI):** Even if an attacker removes the watermark, forensic analysis may still detect “processing traces”, providing an additional line of defence for content provenance.
- **For removal tool developers:** Research directions must be rethought – pursuing only verifier evasion and visual quality is insufficient; statistical undetectability must also be addressed.

---

## Practical Applications

### For Content Platforms / Watermark Designers

1. **Integrate forensic detection into watermark security evaluation** – before deploying any watermark scheme, assess whether removal leaves detectable traces using the framework provided in this paper.

2. **Design “forensic‑friendly” watermarks** – ideally, watermarks should leave strong and hard‑to‑remove statistical traces, so that even if removed, forensic analysis can still identify prior processing.

3. **Build multi‑layer defences** – do not rely solely on watermarks; combine metadata, content fingerprints, and statistical detectors.

### For Removal Researchers / Developers

1. **Incorporate forensic stealth as an optimisation objective** – treat evasion of forensic detectors as a third dimension in the loss function, beyond verifier evasion and PSNR/SSIM.

2. **Explore adversarial training** – use a game between the removal network and a forensic detector to learn statistically undetectable outputs.

3. **Acknowledge theoretical limits** – if perfect stealth is impossible, adjust expectations from “fully undetectable” to “detection is sufficiently costly” or “detection uncertainty is high enough”.

### For Regulators / Policy Makers

The findings suggest that **watermarking of AI‑generated content is not foolproof, but removal attempts may themselves leave traceable evidence**. In legal forensics, even if a watermark is removed, professional image forensics may still identify “manipulation traces”, offering a new technical pathway for tracing AI‑generated content.

---

## References & Resources

- Original paper (arXiv): [https://arxiv.org/abs/2605.09203](https://arxiv.org/abs/2605.09203)  
- PDF version: [https://arxiv.org/pdf/2605.09203.pdf](https://arxiv.org/pdf/2605.09203.pdf)  
- HuggingFace Papers: [https://huggingface.co/papers/2605.09203](https://huggingface.co/papers/2605.09203)  
- Moonlight review: [https://www.themoonlight.io/zh/review/removing-the-watermark-is-not-enough-forensic-stealth-in-generative-ai-watermark-removal](https://www.themoonlight.io/zh/review/removing-the-watermark-is-not-enough-forensic-stealth-in-generative-ai-watermark-removal)  
- BGPT summary: [https://bgpt.pro/?sc=3745080420532224](https://bgpt.pro/?sc=3745080420532224)  
- Scirate page: [https://scirate.com/arxiv/2605.09203](https://scirate.com/arxiv/2605.09203)
