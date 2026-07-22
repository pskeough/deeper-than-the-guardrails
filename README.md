# Deeper Than the Guardrails
### Demographic Bias Survives Refusal-Direction Removal

Does "uncensoring" an open-weight model change how it pathologizes patients? A ten-pair
base/abliterated audit (Ministral, Qwen-3, Gemma, GLM-4, Phi-4, Qwen-2.5, Phi-3-medium,
Llama-3.1, DeepSeek-R1-Distill, Yi-1.5) across four clinical screening instruments,
120 demographic cells, 95,920 observations — graded against author-derived, survey-weighted
NHANES 2005–2018 population norms.

📄 **Paper:** [`paper/psychbench_abliterated_preprint.pdf`](paper/psychbench_abliterated_preprint.pdf) · Patrick S. Keough · preprint in preparation for arXiv

## Headline findings

- **Abliteration reduces but does not remove over-pathologization:** 21 cross-model
  consensus cells drop ~2.13 PHQ-8 points (t = −21.5, 21/21 in the reducing direction) —
  yet remain roughly +9 points above the NHANES population norm. The bias sits deeper
  than the refusal machinery.
- **Three reproducible regimes**, architecture-dependent: preservation, compression,
  correction — convergently classified by six independent psychometric metrics.
- **Calibration–discrimination dissociation:** across 40 pair×instrument combinations,
  abliteration improves both Brier components in only 20% of cases and worsens both in 52%.
- **Honest scope:** transgender and multiracial cohorts have no representative severity
  benchmark (NHANES carries no gender-identity field); their elevated scores are reported
  descriptively, and an earlier "bias localization" claim resting on a non-representative
  offset was withdrawn — the correction is documented in the paper.

## Repository layout

```
paper/       manuscript source, compiled PDF, figures, arXiv submission zip
receipts/    ground-truth migration receipts: MATCH/CORRECTED report, R1–R6
             recompute tables (reproduce residuals from raw to max|diff| = 0.000)
```

Part of a four-paper program auditing LLM behavior in clinical simulation
(sibling papers: [Plausible Patients, Impossible Populations](https://arxiv.org/abs/2604.17359),
[Telling More Than They Can Know](https://github.com/pskeough/telling-more-than-they-can-know)).
Program index: [Research_Collection_Patrick_Keough](https://github.com/pskeough/Research_Collection_Patrick_Keough).

## Authorship note

Drafts prepared with AI assistance under the author's direction; all research questions,
experimental design, analysis decisions, and claims are the author's own, and every
empirical claim is verified against the receipts in this repository.

## License

Code: Apache-2.0 · Paper text and figures: CC BY 4.0 · Derived data: CC BY 4.0 (please cite)
