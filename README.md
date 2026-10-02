# Deeper Than the Guardrails
### Demographic Bias Survives Refusal-Direction Removal

Does "uncensoring" an open-weight model change how it pathologizes patients? A ten-pair
base/abliterated audit (Ministral, Qwen-3, Gemma, GLM-4, Phi-4, Qwen-2.5, Phi-3-medium,
Llama-3.1, DeepSeek-R1-Distill, Yi-1.5) across four clinical screening instruments,
120 demographic cells, 95,920 observations, graded against author-derived, survey-weighted
NHANES 2005–2018 population norms.

📄 **Paper:** [`paper/psychbench_abliterated_preprint.pdf`](paper/psychbench_abliterated_preprint.pdf) · Patrick S. Keough · preprint, July 2026; corrected September 2026

## Headline findings

- **Abliteration reduces but does not remove over-pathologization:** 21 cross-model
  consensus cells drop ~2.13 PHQ-8 points (t = −21.5 on 20 df, p = 2.8 × 10⁻¹⁵, 21/21 in the reducing direction), yet after abliteration they remain about +8 points above the
  NHANES population norm. The bias sits deeper than the refusal machinery.
- **Responses differ by architecture.** Some pairs keep the base model's score distribution
  and others compress it, and the paper's regime classification is being rechecked.
- **Calibration–discrimination dissociation:** across 40 pair×instrument combinations,
  abliteration improves both Brier components in only 20% of cases and worsens both in 52%.
- **Honest scope:** transgender and multiracial cohorts have no representative severity
  benchmark (NHANES carries no gender-identity field); their elevated scores are reported
  descriptively. An earlier "bias localization" claim resting on a non-representative
  offset was withdrawn; the correction is documented in the paper.

## Corrections (September 2026)

The September 2026 correction checked the paper against the analysis outputs. The consensus-survival p-value had been misreported as 1.2 × 10⁻⁵ because the script used a capped normal approximation, and the exact t-test gives 2.8 × 10⁻¹⁵. The correction also fixes several counts in Results and Discussion, the post-abliteration residual (+8, corrected from +9) and eleven bibliography entries. The consensus-survival test now carries an exploratory label, since the project preregistration covered a different four-pair design and none of the analyses reported here. One citation that could not be traced to a published source has been removed. The headline findings do not change.

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
experimental design, analysis decisions, and claims are the author's own, and empirical
claims are checked against the receipts in this repository.

## License

Code: MIT ([LICENSE](LICENSE)) · Paper text, figures and derived data: CC BY-NC-ND 4.0 ([LICENSE-DATA](LICENSE-DATA))
