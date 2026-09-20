# Mechanistic Interpretability Study

**Status: Ongoing** — this is an active undergraduate research project. See "Current Progress" below for exactly what's done and what's next.

Investigating whether small language models reuse the same internal circuits across semantically equivalent reasoning tasks using mechanistic interpretability.

---

## Research Question

When a small language model solves the same underlying reasoning problem presented in different surface forms — for example, direct arithmetic (`3 + 5 = ?`) versus an equivalent word problem ("Priya has 3 apples and gets 5 more...") — does it reuse **one shared internal circuit**, or rely on **separate, format-specific shortcuts** that happen to both produce correct answers?

This matters because the answer distinguishes between two very different things a model could be doing: genuinely generalizing a reusable mechanism, versus memorizing surface-level patterns per phrasing — with real implications for how much we should trust a model's behavior generalizing to inputs worded differently than what it was tested on.

## Method

Using **activation patching** (causal tracing): running a model on an input, caching its internal activations, then "patching" an activation from a different run into the first to see whether it causally changes the output. By systematically patching across layers and attention heads, it's possible to build a map of which components are causally responsible for a correct answer — separately for each surface form — and compare the overlap between those maps.

This approach follows established methodology from published interpretability research (see References) rather than proposing a new technique; the contribution here is the specific surface-form comparison, not the method itself.

## Tools & Stack

| Tool | Purpose |
|---|---|
| [TransformerLens](https://github.com/TransformerLensOrg/TransformerLens) | Open-source library for activation caching, hooking, and causal intervention on transformer models |
| GPT-2-small / Pythia | Small, open-weight models — chosen to keep the whole project runnable on free-tier GPUs |
| PyTorch | Underlying deep learning framework |
| Google Colab | Free GPU compute |

## Repository Structure

```
├── 01-Research log/     # Daily research log — what was learned/done each session
├── 09-Resources/        # Reading notes, learning resources, reference material
├── code/                # Notebooks — reproductions and original experiments
├── figures/             # Generated plots and visualizations
├── data/                # Custom datasets built for the surface-form comparison
├── LICENSE
└── README.md
```

## Current Progress

- [x] Built foundational understanding of transformer architecture — attention mechanism, residual stream, MLP layers, positional encoding
- [x] Set up TransformerLens and reproduced the official demo notebook on GPT-2-small
- [x] Reproduced the logit lens technique on custom prompts, visualizing model confidence across layers
- [ ] Reproduce a basic activation-patching example
- [ ] Design and validate a custom dataset of matched surface-form examples (arithmetic vs. word problem vs. additional formats)
- [ ] Run the core cross-format activation-patching comparison
- [ ] Compute and analyze circuit overlap across surface forms
- [ ] Write up findings

## Key References

- Meng et al., *"Locating and Editing Factual Associations in GPT"* (ROME) — causal tracing methodology
- Wang et al., *"Interpretability in the Wild"* (IOI circuit) — structural template for identifying and validating a circuit
- nostalgebraist, *"interpreting GPT: the logit lens"* — foundational technique for viewing intermediate-layer predictions
- Elhage et al., *"A Mathematical Framework for Transformer Circuits"* (Anthropic) — conceptual grounding for the residual stream

## Author

Sinchana Suresh — Second-year CS undergraduate, RNS Institute of Technology, Bengaluru

---

