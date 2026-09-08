
# WoE: Watermarking of Experts for MoE LLMs

## Paper Highlights

WoE (Watermarking of Experts) introduces a black‑box text watermarking scheme tailored for Mixture‑of‑Experts (MoE) large language models. It embeds a detectable statistical signal into the expert routing process, enabling provenance tracing of generated text. The method is validated on four MoE models and remains detectable under supervised fine‑tuning, model extraction, and output‑level paraphrasing attacks.

---

## Core Research Content

- **Problem Definition**  
  MoE LLMs dynamically route each input token to a small subset of expert networks via sparse routing. Conventional watermarking schemes designed for dense models assume that the watermarked parameters are activated consistently, but MoE’s dynamic routing violates this assumption. Moreover, most existing methods focus on text fluency and are not designed for constrained or complex tasks, while their extra overhead makes them unsuitable for latency‑sensitive systems. The key challenge is how to embed watermarks in MoE architectures efficiently, with high fidelity and robustness.

- **Innovative Method**  
  WoE’s core innovation lies in **embedding the watermark into the expert routing process** rather than as a post‑processing step during token sampling. Specifically, it perturbs the expert selection of each router in a controlled manner; these perturbations accumulate layer‑by‑layer and eventually produce a detectable token‑selection bias at the output. Unlike traditional approaches, WoE integrates watermarking inside the inference loop. In addition, WoE supports **black‑box verification** – detection can be performed solely from API outputs without any internal model access.

- **Research Results**  
  Experiments are conducted on MoE models such as Mixtral‑8×7B‑Instruct and Qwen3‑30B‑A3B‑Instruct. WoE achieves **only 1% additional inference latency** while preserving output quality close to the unwatermarked model, and offers up to **4× speedup** over existing watermarking methods. In terms of robustness, WoE demonstrates superior stability under quantization, fine‑tuning, pruning, and adaptive attacks. More importantly, it remains detectable under **adversarial supervised fine‑tuning, model extraction, and output‑level paraphrasing**, forcing adversaries to trade off between removing the attribution signal and maintaining output utility.

- **Practical Deployment Potential**  
  **High.** WoE’s design considers real‑world deployment: 1% latency overhead makes it production‑ready; black‑box verification fits API‑only scenarios; and robustness to fine‑tuning, pruning, and other post‑processing ensures watermark persistence. These characteristics provide a solid technical foundation for commercial MoE model services.

---

## Technical Details

The technical core is the **routing perturbation** mechanism. In MoE, each token is routed through a router that computes a probability distribution over experts, and the top‑k experts are selected. WoE adds a controlled bias to the router’s logits to influence expert selection:

$$
\text{logit}'_i = \text{logit}_i + \alpha \cdot \mathbf{1}_{i \in \mathcal{E}_w}
$$

where $\mathcal{E}_w$ is a pre‑defined set of watermark experts, and $\alpha$ is the perturbation strength hyperparameter. This perturbation systematically biases expert selection; the biases accumulate across tokens and produce a statistically detectable distribution pattern in the generated text.

Unlike traditional token‑sampling watermarks (e.g., the KGW scheme) that require extra hash computations and vocabulary partitioning at each decoding step, WoE’s perturbation operates at the routing level without re‑partitioning the vocabulary, resulting in significantly lower computational overhead.

From a security perspective, because the watermark is embedded in the routing path rather than in the output tokens themselves, simple text rewriting or paraphrasing cannot easily remove it – changes in routing patterns cascade to affect subsequent token generation distributions.

---

## Experimental Setup

| Item | Details |
|------|---------|
| **Target Models** | Mixtral‑8×7B‑Instruct‑v0.1, Qwen3‑30B‑A3B‑Instruct (sparse MoE) |
| **Watermark Embedding Location** | Expert selection process of MoE layers |
| **Verification Modes** | White‑box routing inspection (forensic) and black‑box output detection (API access) |
| **Evaluation Metrics** | Verification accuracy (>99%), perplexity degradation (<2%), inference latency (+1%) |
| **Robustness Tests** | Quantization, fine‑tuning, pruning, adaptive attacks; adversarial supervised fine‑tuning, model extraction, output‑level paraphrasing |

---

## Comprehensive Analysis

**Author Team Background**  
The authors include Jona te Lintelo (Radboud University), Lichao Wu (PhD candidate at Delft University of Technology), and Stjepan Picek (Delft University of Technology). The team has a sustained track record in AI security, including work on MoE jailbreaking (L³) and activation manipulation (MASCing). This indicates deep practical understanding of MoE internals – the research is not purely theoretical but grounded in engineering practice.

**Technical Feasibility Assessment**  
The core idea – using MoE routing as a watermark carrier – is logically self‑consistent. MoE routing is a deterministic function (given an input, the routing distribution is fixed), so a systematic bias can embed a reproducible signal. This approach aligns with concurrent works (PathMark and WaterMoE), suggesting that routing‑based watermarking is becoming a consensus direction in MoE watermarking, which increases credibility.

**Limitations and Open Questions**  
The reported “1% latency overhead” and “4× speedup” are measured under specific experimental conditions; performance in real production environments requires further validation. Moreover, there is an inherent trade‑off between perturbation strength $\alpha$ and output quality – the paper does not clearly specify the exact boundary of this trade‑off. Finally, while the paper claims robustness against “output‑level paraphrasing”, further empirical evidence is needed for stronger semantic‑preserving rewrites (e.g., using another LLM for rephrasing).

**Theory vs. Practice**  
WoE is not a purely theoretical paper. The author team has practical experience in MoE security; the paper includes empirical validation on real MoE models and considers realistic scenarios such as API‑only deployment. All these factors indicate strong practical potential.

---

## Practical Applications

1. **Commercial MoE Model APIs**  
   WoE’s black‑box verification makes it especially suitable for SaaS model services. Providers can embed watermarks at inference time without user awareness, and later verify whether an output originates from their model via API.

2. **Model Copyright Protection**  
   When an MoE model is illegally copied or stolen, copyright holders can verify ownership through WoE’s detection mechanism. Robustness to fine‑tuning and pruning ensures that even if the attacker modifies the model, the watermark remains detectable.

3. **Content Provenance and Anti‑Forgery**  
   In domains such as journalism or academic publishing where source attribution is critical, WoE can be used to verify whether a given text was generated by a specific MoE model, helping prevent misuse of AI‑generated content.

4. **Deployment Recommendations**  
   In practice, adjust the perturbation strength $\alpha$ according to the use case: use a smaller $\alpha$ for quality‑sensitive tasks, and increase $\alpha$ when stronger detection robustness is required. Also, keep the watermark expert set $\mathcal{E}_w$ secret as a key to prevent reverse engineering.

---

## References

- Original Paper (Semantic Scholar): [WoE Wrote It? Watermarking Mixture‑of‑Experts LLMs for Black‑Box Text Provenance](https://www.semanticscholar.org/paper/WoE-Wrote-It-Watermarking-Mixture-of-Experts-LLMs-Lintelo-Wu/dbfae6a9f6cc8380ca06c4c8bf9f1b077a3bd3ba)
- Related Work: PathMark – [arXiv:2607.03688](https://arxiv.org/abs/2607.03688)
- Related Work: WaterMoE – [arXiv:2607.13099](https://arxiv.org/abs/2607.13099)
