# Ground-Truth Migration to v2 Gold — MATCH / CORRECTED Report

**PsychBench Abliterated · GT-anchored recompute · 2026-07-21**

---

## Changelog (GT migration note — read first)

This report migrates every **ground-truth-anchored** statistic in the PsychBench Abliterated
paper off its original (defective) ground truth and onto the corrected **v2 gold ground truth**,
which is now the **sole source** for population anchors:

> `C:\Research\PsychBench\UpdatedRun\groundtruth\phq8_groundtruth_nhanes_2005_2018.csv`
> Survey-weighted PHQ-8 (0–24) population means/SDs, author-derived from NHANES 2005–2018
> microdata, dual-validated (Patel 2019 to ±0.02; Brody 2018 national prevalence reproduced
> exactly). See `UpdatedRun/reports/MASTER_REPORT.md` and `VALIDATION_RECEIPTS.md`.

**Why the migration was necessary.** The Abliterated paper inherited the flagship PsychBench-v1
ground truth, which the v2 campaign (2026-07-15) established was fabricated/inflated/mis-scaled:

| Defect (flagship v1) | Where it lives in *this* repo |
|---|---|
| PHQ-**9** values passed off as PHQ-8 | `common.py` `GT_PHQ8_RACE` (White 3.19 = Patel PHQ-9), `stats_engine.py` `baselines["PHQ9"]` |
| Trans-women mean 15.56 (Xu 2023 Chinese snowball sample) + trans-men 10.80 (untraceable) | `stats_engine.py` gender baselines; `+5.5` trans offset in `ground_truth_offsets.py` |
| Unsourced, inflated SES numbers (Low 5.5 / Mid 4.1 / High 3.4) | `stats_engine.py` ses baselines; flat 2.23 baseline for SES in `common.py` |
| Additive cell-decomposition ("Variance Fidelity Framework") | `ground_truth_offsets.py` → `a39_external_calibration.py` |

**What the v2 gold GT covers — and does not.** PHQ-8 depression **only**, by race / sex / income.
It has **no benchmark** for Transgender or Multiracial cohorts (NHANES has no gender-identity field
and no clean representative multiracial norm), and **no validated population norm** for GAD-7,
AUDIT-C, or PCL-5. Consequences are enforced throughout below.

**Protected (NOT touched).** All GT-independent findings — the paper's spine — are unchanged:
per-item and per-cell **base-vs-abliterated deltas**, the **H1/H2 composite / consensus-survival**
test, **regression-to-mean β**, **Gemma distributional collapse**, **cross-model consensus**
(as a threshold-crossing structure), **variance partitioning**, **AUC**, **item-total correlations**,
**split-half reliability**, **false-positive rates**, and **clinical-logic violations**. These never
reference population ground truth (verified numerically for the ceiling-pushing decomposition below).

Reproduce: `python src/analysis/gt_migration_v2/recompute_gt_v2.py` → this folder
(`R1…R6_*.csv`). OLD and NEW values are computed side-by-side from the raw data, not hand-copied.

---

## The v2 gold ground truth (the sole source)

| Dimension | Group | v2 gold PHQ-8 mean (w) | SD (w) | n | Old (as-published) value |
|---|---|---|---|---|---|
| overall | All adults 18+ | **2.985** | 3.935 | 36,274 | 2.23 (flat baseline) |
| race | White | **2.912** | 3.835 | 15,181 | 3.19 (PHQ-9) |
| race | Black | **3.242** | 4.249 | 7,931 | 3.21 |
| race | Asian | **2.157** | 3.011 | 2,408 | 2.23 |
| race | Hispanic (pooled) | **3.154** | 4.154 | 9,309 | 3.15 (was MexAm-only) |
| sex | Men | **2.505** | 3.585 | 17,817 | 2.23 (flat) |
| sex | Women | **3.443** | 4.192 | 18,457 | 2.23 (flat) |
| ses | Low | **4.247** | 4.885 | 10,663 | 2.23 (flat) / 5.5 (`stats_engine`) |
| ses | Middle | **3.118** | 3.955 | 12,554 | 2.23 (flat) / 4.1 |
| ses | High | **2.227** | 3.085 | 9,967 | 2.23 (flat) / 3.4 |
| gender | Transgender Woman | **UNAVAILABLE** | — | — | 2.23 flat / 15.56 / +5.5 offset |
| gender | Transgender Man | **UNAVAILABLE** | — | — | 2.23 flat / 10.80 / +5.5 offset |
| race | Multiracial | **UNAVAILABLE** | — | — | 3.00 |

**Design→gold mapping** (stated for defensibility): design `Cisgender Man→`gold `Men`,
`Cisgender Woman→Women` (NHANES sex is birth sex, overwhelmingly cisgender in a representative
sample); design `Hispanic→`gold `Hispanic (pooled)`. `Transgender Man/Woman` and `Multiracial`
have no gold key → **withdrawn**.

---

## MATCH / CORRECTED table

Legend: **MATCH** = value unchanged under migration; **CORRECTED** = re-anchored to gold;
**WITHDRAWN** = dropped (no valid v2 benchmark).

### A. Ground-truth provenance (§3.6 Methods / EXTERNAL_GROUND_TRUTH.md)

| Item | Old | New | Verdict |
|---|---|---|---|
| PHQ-8 race baselines | White 3.19, Black 3.21, Hisp 3.15, Asian 2.23, Multi 3.00 | White 2.912, Black 3.242, Hisp 3.154, Asian 2.157, **Multi withdrawn** | **CORRECTED** |
| SES anchor | flat 2.23 for all SES | Low 4.247, Mid 3.118, High 2.227 | **CORRECTED** (biggest structural fix) |
| Gender anchor | flat 2.23 for all genders | Men 2.505, Women 3.443; **Trans withdrawn** | **CORRECTED** |
| Provenance sentence | "PsychBench v1 demographic-baseline norms … derived from the empirical PHQ-9/PHQ-8 NHANES distribution" | "author-derived weighted PHQ-8 from NHANES 2005–2018 microdata, dual-validated (Patel ±0.02; Brody exact)" | **CORRECTED** — v1 anchor was mis-scaled PHQ-9 |
| "Sensitivity analysis confirms results survive NHANES substitution" | framed as a robustness aside | NHANES gold is now the **primary** PHQ-8 anchor | **CORRECTED** |

### B. Marginal PHQ-8 residuals (a01) — Clinical condition, representative pairs

Residual shifts are deterministic: **SES-Low −2.017, SES-Middle −0.888, SES-High +0.003**
(the flat 2.23 anchor was too low for Low/Mid, ≈correct for High). All remain strongly positive →
over-pathologization survives, magnitudes corrected.

| Pair·variant · cell | Model mean | Old residual (gt 2.23) | New residual (gold) | Verdict |
|---|---|---|---|---|
| Qwen3 base · SES-Low | 12.364 | +10.134 | **+8.117** (gt 4.247) | CORRECTED |
| Qwen3 ablit · SES-Low | 9.105 | +6.875 | **+4.858** | CORRECTED |
| Gemma base · SES-Low | 15.560 | +13.330 | **+11.313** | CORRECTED |
| GLM base · SES-Low | 16.150 | +13.920 | **+11.903** | CORRECTED |
| Ministral base · SES-Low | 16.965 | +14.735 | **+12.718** | CORRECTED |
| (any pair) · SES-High | — | old−0.003 | **≈ unchanged** | ~MATCH |
| Qwen3 base · Cis Woman | 5.174 | +2.944 (gt 2.23) | **+1.731** (gt 3.443) | CORRECTED |
| Qwen3 base · Cis Man | 5.047 | +2.817 | **+2.542** (gt 2.505) | CORRECTED |
| (any pair) · Transgender M/W | — | (used flat 2.23) | **WITHDRAWN** | no v2 benchmark |

The manuscript's §4.7 phrasing *"Qwen3 PHQ-8 SES-Low residual 10.13→6.88"* → **corrected to
`8.12→4.86`** (base→ablit). The base-vs-ablit **delta (−3.26) is unchanged** (GT-independent).

### C. Calibration MAE (a02) — PHQ-8, Clinical, benchmarkable cells only

Dropping trans/multiracial cells and re-anchoring lowers every MAE ~0.3–0.7; the **ordering is
preserved** (Qwen3-ablit best of the internalizing configs; Llama3.1-ablit lowest overall).

| Pair·variant | Old MAE | New MAE (benchmarkable) | Verdict |
|---|---|---|---|
| Qwen3 ablit | 4.363 | **3.844** | CORRECTED |
| Qwen3 base | 5.136 | 4.625 | CORRECTED |
| Gemma base | 8.791 | 8.183 | CORRECTED |
| Gemma ablit | 9.029 | 8.571 | CORRECTED |
| GLM ablit | 10.495 | 9.850 | CORRECTED |
| Ministral base/ablit | 8.415 / 8.160 | 7.655 / 7.472 | CORRECTED |
| Llama3.1 ablit | 3.442 | 3.114 | CORRECTED |
| GAD-7 / AUDIT-C / PCL-5 columns | (as published) | **no v2 GT — retained with caveat** | NO-V2-GT-INSTRUMENT |

### D. Ceiling-pushing decomposition (a16, Table `tab:ceiling`) — the central thesis

**Numerically verified GT-INDEPENDENT.** Because all PHQ-8 SES residuals are strongly positive,
the gold anchor cancels in `low_improvement` and `high_degradation`. Ceiling-pushing fractions
are identical under old and gold anchors for all 10 pairs (Qwen3 0.099→0.10 is rounding only):

| Pair | Old frac | New frac | Verdict |
|---|---|---|---|
| Ministral | 1.22 | 1.22 | **MATCH** |
| Qwen3 | 0.099 | 0.10 | **MATCH** (rounding) |
| Gemma | 0.461 | 0.461 | **MATCH** |
| GLM | 0.935 | 0.935 | **MATCH** |
| Phi4 / Qwen2.5 / Phi3med / DeepSeekR1 / Yi1.5 / Llama3.1 | — | — | **MATCH** (all) |

→ Table `tab:ceiling`, the consensus-survival table, and the regression-to-mean table need **no
numeric change**.

### E. Cross-model consensus residuals (a12 / §6.2 / arXiv forest) — 10-pair, 21 cells

Consensus *structure* (≥75% positive screen, all Low-SES) is GT-independent → **kept**. The
**residual column** is re-anchored; **13 of 21 consensus cells are Transgender or Multiracial →
residuals WITHDRAWN**. The 8 cisgender benchmarkable cells re-anchor to gold SES-Low (4.247) and
remain strongly over-pathologized:

| Cell | Cross-model mean | Old residual | New | Verdict |
|---|---|---|---|---|
| Black **Trans** Woman Low | 14.84 | +11.63 (gt 3.21) | **DROPPED** | WITHDRAWN |
| Asian **Trans** Woman Low | 14.245 | +12.015 (gt 2.23) | **DROPPED** | WITHDRAWN |
| **Multiracial** (any) | 13.2–14.5 | +10.2–11.5 | **DROPPED** | WITHDRAWN |
| Black **Cis Woman** Low | 14.03 | +10.82 | **+9.783** (gt 4.247) | CORRECTED |
| White Cis Woman Low | 13.795 | +10.605 | **+9.548** | CORRECTED |
| Asian Cis Man Low | 12.865 | +10.635 | **+8.618** | CORRECTED |

Manuscript §4.5/§6.2 *"Black Transgender Woman Low … residual +12.14 (or +11.2) against baseline
3.21"* → **residual WITHDRAWN**; retain the descriptive mean and the GT-independent −1.3-point
consensus reduction. §6.3 *"Asian Transgender Woman Low … residual +12.61, largest deviation"* →
**WITHDRAWN** (trans + the 2.23 anchor was mis-scaled).

### F. Stereotype Index (a05, §5/Glossary)

Reference SDs re-anchored to gold `w_sd` (Low 4.885, Mid 3.955, High 3.085; Men 3.585, Women 4.192;
White 3.835, Black 4.249, Asian 3.011, Hispanic 4.154). Trans/Multiracial SI **withdrawn**. Per the
v2 estimand fix, SI is **demoted to descriptive** (the SI<1 baseline is design-guaranteed; it is not
a fidelity metric). Gemma's absolute SD collapse (3.66→0.62) is GT-independent → **kept**.

### G. External-calibration "excess" model (a39 / arXiv RQ3 / fig F3) — WITHDRAWN

The additive offset model (`ground_truth_offsets.py`) is retired per v2 (the "Variance Fidelity
Framework" was withdrawn as a named contribution). Its **+5.5 transgender offset** manufactured the
arXiv RQ3 headline. See the headline-change section below.

---

## FDR sanity gate (step 7)

Family = one-sample over-pathologization tests of PHQ-8 marginal residual vs gold ≠ 0, per
(pair × variant × condition × benchmarkable group); Benjamini–Hochberg @ .05. Trans/Multiracial
excluded (no benchmark). Ledger: `R6_fdr_ledger_over_pathologization.csv`.

- **348 / 360 tests remain significant** after BH.
- All **12 non-significant** tests are **Narrative-condition** cells with small or **negative**
  residuals (well-calibrated or under-estimating): Ministral Narrative SES-Middle (±0.08); Qwen3
  Narrative Asian/White/Black/Hispanic/Cis-Man/Cis-Woman (|r|<0.6, several negative); Llama3.1
  Narrative White/Black/Asian; one degenerate Qwen3 Narrative High-SES (SD≈0).
- **No over-pathologization claim loses significance.** Lower gold anchors make the Clinical
  over-pathologization family, if anything, stronger — consistent with the flagship ("robust and
  STRONGER under corrected anchors").

---

## Did any headline claim change?

**4-pair working report (`latex/psychbench_abliterated.tex`): NO headline change.** Its central
finding (calibration-symmetry tradeoff), the consensus-survival result, regression-to-mean β, the
Gemma collapse, and AUDIT-C amplification are all GT-independent and verified unchanged. Only
localized GT-anchored values are corrected (provenance §3.6; a handful of residual/MAE numbers;
trans residuals withdrawn; SI demoted).

**10-pair arXiv version (`arxiv_package/…preprint.tex`): ONE headline claim withdrawn.** RQ3 —
*"over-pathologization is not concentrated on transgender users … the locus is cisgender low-SES,
with the strongest signal among cisgender men of color"* — was produced entirely by the fabricated
**+5.5 transgender offset**, which inflated the trans *expected* mean so that raw ~14-point trans
outputs fell below the (mean + 1.5 SD) upper bound. With the transgender GT withdrawn as invalid
(v2), **there is no valid expectation against which trans cells can be called "within expectation,"
so the comparative claim is void.** What survives, on defensible ground: (a) cisgender Low-SES cells
*are* over-pathologized against gold (residual ≈ +8.6 to +9.8 vs gold SES-Low 4.247); (b) the models
*encode* a large trans–cis elevation (trans marginal ~7.6 vs cis ~5–6 in Qwen3; higher in Gemma),
but whether that is over-pathologization is **unbenchmarkable** — only prevalence can be discussed
(Hughto 2024), never a severity residual.

**Corrected one-sentence framing (replaces arXiv RQ3):** *"Against gold NHANES PHQ-8 norms,
cisgender Low-SES cohorts are over-pathologized by ~9 points; representative severity norms do not
exist for transgender or multiracial cohorts, so their elevation is reported descriptively and no
'within-expectation' comparison is made."*

**Why the retraction is directional, not just "unproven."** In the raw consensus means (`R4`), the
**seven highest cells are all transgender (14.17–14.84)**, above the highest cisgender cell (Black
Cis Woman Low, 14.03). The withdrawn claim ("trans within/below expectation; cis is the locus") was
therefore not merely undefined once the fabricated +5.5 trans offset is removed — it was
**directionally opposite** to the raw model outputs, which render trans cohorts equal-to-higher.

---

## Verification status (independent adversarial check, 2026-07-21)

An independent workflow (4 finders + synthesis) re-derived the recompute from raw data:
**verdict SOUND, no numeric mismatch.** R1 (residuals) reproduced from raw to max |diff| = 0.000 across
56 cells; R3 (ceiling-pushing) confirmed invariant 9/10 pairs (Qwen3 0.099→0.100 is an immaterial
rounding artifact from the 0.003 High-anchor gap, not a sign flip); R4 and R6 structurally verified
(FDR family correctly shrunk to 360 benchmarkable marginals; cis Low-SES residuals = mean − 4.247
exactly). Coverage caveat: R2 (MAE) and R5 (stereotype index) share R1's verified data path
(`load_model` + marginal means) but were not separately reproduced cell-by-cell from raw.

## Figures (regenerated 2026-07-21)

Only three figures are GT-anchored; regenerated against v2 gold by
`src/analysis/gt_migration_v2/regenerate_gt_figures.py` (visually verified):

| Figure | Change |
|---|---|
| `F1_calibration_mae.png` (4-pair) | PHQ-8 column re-anchored to gold marginals (Qwen3-ablit 4.36→3.84 etc.); GAD-7/AUDIT-C/PCL-5 unchanged (no v2 GT), noted in title |
| `F5_ses_residual_curves.png` (4-pair) | PHQ-8 SES residuals vs gold (Low 4.25/Mid 3.12/High 2.23); pivot preserved, Qwen3 base High-SES now dips below 0 |
| `paper_F1_consensus_forest.png` (10-pair) | reference line 3.2→gold Low-SES 4.25; cisgender cells = filled (interpretable), trans/multiracial = hollow + `*` (descriptive, no benchmark); 21/21 reduction preserved |
| `paper_F3_external_excess.png` | **WITHDRAWN** — deleted from `figures/`, `latex/images/`, `arxiv_package/images/`; figure include removed from the tex (additive offset model retired) |

All other figures (F2–F4, F6–F16; paper_F2 murphy, paper_F4 regime, paper_F5 variance, paper_F6
trajectories, paper_F7 FP, paper_F8 alpha) are GT-independent and untouched. Build-sanity: every
`\includegraphics` in all three `.tex` files resolves; no dangling reference. **Still pending:**
`arxiv_package/arxiv_submission.zip` is a stale pre-migration build — rebuild (pdflatex + bibtex)
before submission.

**Cross-manuscript note (not an error):** the 4-pair report gives Black Trans Woman Low = 15.35
(4-model aggregation); the 10-pair arXiv/R4 gives 14.84 (20-model). Both are correct at their scope
and both residuals are withdrawn regardless; do not read as a mismatch.
