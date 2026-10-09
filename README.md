# Verification-First Characterization of 3GPP Air-to-Ground Channels for UAV Base Stations — reproducibility archive (v1.0.0)

Code, verification suites, executed cross-validation data, and figure
scripts accompanying the article

> M. J. Alhasan, W. M. R. Shakir, A. S. Daghal, and F. H. Salman,
> "Verification-First Characterization of 3GPP Air-to-Ground Channels for
> UAV Base Stations: Implementation, Cross-Validation, and Deployment
> Implications," submitted to *IEEE Access*, 2026.

Everything numerical in the article derives from one frozen implementation
(`implementation/ch3_code_complete_v7/stage2/uav_channel`, interface
v1.0.0) and from two executed QuaDRiGa campaigns whose raw per-link outputs
are shipped here unchanged. This archive lets a reader (i) re-run the 48
automated checks that verify the implementation against 3GPP TR 36.777 and
TR 38.901, (ii) regenerate the 270-cell cross-validation report of Table 4
from the raw campaign files with two independent comparators, and (iii)
regenerate every data figure of the article from fixed seeds.

**Scope statement (as in the article).** The checks establish that the
code implements the standard and that two independent implementations agree
wherever the two standards share an expression. They do not validate the
standard against measurements, and nothing here is a field measurement.

## Contents

```
uavbs-a2g-verification-v1.0.0/
├── README.md                      this file
├── UPLOAD_INSTRUCTIONS.md         how the authors deposit this archive and cite it
├── ENVIRONMENT.md                 software versions of the verification run recorded here
├── LICENSING.md, LICENSE, LICENSE-DATA-AND-DOCS
├── CITATION.cff, .zenodo.json     citation metadata (Zenodo reads both)
├── MANIFEST.sha256                SHA-256 of every file (tools/make_manifest.py --verify)
├── implementation/ch3_code_complete_v7/     the authors' verified pack (verbatim; see "What was changed")
│   ├── stage2/uav_channel/        FROZEN implementation: params / large_scale / shadowing /
│   │                              small_scale / altitude / lookup  (Sections III, VI; Algorithm 2)
│   ├── stage2/tests/test_all.py   26 checks (layers 1-4: transcription, calculus, vectorization, statistics)
│   ├── stage3/tests/test_stage3.py 10 checks (validation campaign, FR2 bracket, comparator pipeline)
│   ├── stage4/tests/test_stage4.py 12 checks (interface physics closures; SHA-256 artifact manifest)
│   ├── stage3/analysis/           metrics, Monte-Carlo validation, FR2 sensitivity (Figs. 3, 8, 9)
│   ├── stage3/quadriga_kit/       QuaDRiGa comparator: ingest_quadriga.py, analyze_report.py,
│   │                              mechanism_check.py, quadriga_campaign.m (FR1 template)
│   ├── stage2/out, stage3/out3    hash-manifested artifacts (hstar_map.csv, fr2_delta_pl.csv, ...)
│   ├── discussion/lit_comparison.py  regret protocol and sigmoid/free-space comparison (Table 5, Fig. 10)
│   ├── matlab/                    MATLAB port with parity gate (not required for the article)
│   ├── extras/                    explorer, deployment tables (not used by the article)
│   └── Implementation_Guide.pdf, README_BUNDLE.md, STAGE2/3/4_REPORT.*  the pack's own documentation
├── campaign/                      EXECUTED QuaDRiGa 2.8.1 cross-validation (13,500 links)
│   ├── quadriga_samples_FR1_2026-08.csv     9,000 links, 2.6 and 3.5 GHz (Aug. 2026)
│   ├── quadriga_samples_28GHz_2026-10.csv   4,500 links, 28 GHz (Oct. 2026)
│   ├── quadriga_campaign_28GHz.m            the script that produced the 28-GHz file
│   ├── campaign_log_2026-10-04.md           versions, dates, wall times, provenance check, SHA-256
│   ├── audit_campaign.py                    INDEPENDENT comparator (does not import uav_channel)
│   ├── cell_report_FR1.csv, cell_report_28GHz.csv, summary_FR1.json, summary_28GHz.json
│   └── pack_comparator/                     outputs of the PACK comparator on the same raw files
│       ├── report_regen_FR1.csv             = stage3/quadriga_kit/report_executed_2026-08.csv (byte-identical values)
│       ├── report_executed_2026-10.csv      28-GHz block (90 cells)
│       ├── report_executed_all_2026-10.csv  270 cells (Table 4; source of Fig. 4)
│       ├── analyze_report_guarded.py        archive copy of analyze_report.py (3 guards, see below)
│       ├── combine_reports.py
│       └── analyze_report_*.txt, mechanism_check_*.txt   logs of the runs recorded here
├── article_figures/
│   ├── figsrc/make_figures_access.py   Figs. 3-10 from the frozen pack (vector PDF)
│   ├── figsrc/make_abstract_access.py  graphical abstract (reads data.json from extras/visualization/precompute.py)
│   ├── figsrc/fig_sysmodel.tex, fig_pipeline.tex, orcid_icon.tex   Figs. 1-2 and the ORCID icon (TikZ)
│   ├── figsrc/build_tikz.sh
│   └── figs/                           the eight regenerated data figures as used in the article
├── logs/                          unittest logs of the 48 checks and the environment record
└── tools/
    ├── run_all.sh                 one-command reproduction (steps 1-7 below)
    └── make_manifest.py           SHA-256 manifest writer / verifier
```

## Quick start

Python ≥ 3.10 with `numpy`, `scipy`, `matplotlib` (the versions used for the
run recorded here are in `ENVIRONMENT.md`). No other dependency is needed
for the checks, the comparators, or the figures. MATLAB + QuaDRiGa 2.8.1 are
needed only to re-run the campaign itself; its raw outputs are shipped.

```bash
pip install -r implementation/ch3_code_complete_v7/requirements.txt
bash tools/run_all.sh
```

`run_all.sh` performs, in order: (1) manifest verification; (2) the three
test suites (`26 + 10 + 12 = 48` checks, all must print `OK`); (3) the pack
comparator on both raw campaign files and a byte-for-byte comparison of the
resulting cell reports with the shipped ones; (4) the pack's forensic
analysis and mechanism check; (5) the independent comparator and a
comparison of its summaries with the shipped ones; (6) regeneration of
Figs. 3-10 into `regen/figs/`; (7) regeneration of the graphical abstract.
The whole run takes well under a minute on a laptop. Any discrepancy stops
the script with a non-zero exit status.

To run only the 48 checks:

```bash
cd implementation/ch3_code_complete_v7
(cd stage2 && python3 -m unittest discover -s tests)   # Ran 26 tests ... OK
(cd stage3 && python3 -m unittest discover -s tests)   # Ran 10 tests ... OK
(cd stage4 && python3 -m unittest discover -s tests)   # Ran 12 tests ... OK
```

## What reproduces what

| Article item | Produced by | Inputs | Expected values (as published) |
|---|---|---|---|
| Table 2 (path-loss branches, constants) | `stage2/uav_channel/params.py` (verified constants module) | TR 36.777 Table B-2 | transcription checks in `stage2/tests` |
| Table 3 / Algorithm 1 (48 checks, five layers) | `stage2/tests`, `stage3/tests`, `stage4/tests`; `STAGE2/3/4_REPORT.md` | — | 26 + 10 + 12 pass |
| Table 4 (13,500 links, 270 cells) | `stage3/quadriga_kit/ingest_quadriga.py` on `campaign/quadriga_samples_*.csv`; independent check: `campaign/audit_campaign.py` | raw campaign files | UMa LoS bias 0.000 dB in all 72 populated cells; 435 of 438 state-conditioned comparisons closed-form within 0.01 dB; NLoS gap medians −8.9 / −12.2 (−11.3 at 28 GHz) / +32.3 dB; σ ratios 1.07 (1.05) → 5.67 (6.24) |
| Table 5 (policy regret) | `discussion/lit_comparison.py` → `discussion/lit_comparison.csv` | frozen pack | regret up to 0.35 (free space), 0.30 (sigmoid, FR2 bimodal cell) |
| Fig. 3 (effective-PL CDFs) | `article_figures/figsrc/make_figures_access.py` (recipe `stage3/scripts/make_figures3.py`) | `stage3/analysis/validation.py` | KS non-rejection, `stage3/out3/validation_ks.csv` |
| Fig. 4 (σ_SF ratio) | same script | `campaign/pack_comparator/report_executed_all_2026-10.csv` | 28-GHz markers coincide with FR1 markers |
| Figs. 5, 6, 7 (P_LoS, altitude map, bimodal) | same script (recipe `stage2/scripts/make_figures.py`) | frozen pack; `stage2/out/hstar_map.csv` | wall 90.5 m at 200 m (UMa, 3.5 GHz); 43.9 m optimum in the tight-budget FR2 cell |
| Figs. 8, 9 (FR2 two-model range, h* shift) | same script (recipe `stage3/scripts/make_figures3.py`) | `stage3/out3/fr2_delta_pl.csv`, `fr2_hstar_shift.csv` | −23 to −2 dB; shift ≤ 14.4 m over 49 meaningful cells |
| Fig. 10 (LoS probability vs elevation) | same script (recipe `discussion/lit_comparison.py`) | frozen pack | RMSD 0.66, max 0.95; ≈ 42° reproduced |
| Figs. 1, 2 | `article_figures/figsrc/build_tikz.sh` | TikZ sources | — |
| Graphical abstract | `extras/visualization/precompute.py` then `article_figures/figsrc/make_abstract_access.py` | frozen pack | — |
| Algorithm 2 (h* solver) | `stage2/uav_channel/altitude.py` | — | step 0.05 m, plateau tolerance 1e-4, span 50 m |

Section numbers above refer to the article. Every expected value in the
last column was re-obtained in the run recorded in `logs/` and
`campaign/pack_comparator/` (Oct. 9, 2026).

## Campaign provenance

Both campaigns were executed by the authors with QuaDRiGa v2.8.1-0 under
MATLAB R2026a: the FR1 block in August 2026 (9,000 links, 2.6 and 3.5 GHz)
with the pack's `quadriga_campaign.m`, and the FR2 block on October 4, 2026
(4,500 links, 28 GHz; two complete runs of 48.1 and 33.2 min produced
bit-identical files) with `campaign/quadriga_campaign_28GHz.m`, which
differs from the pack template only by the five QuaDRiGa-2.8.1 API
adaptations listed in its header. Geometry (ground node at 1.5 m, UAV at
altitude h and ground range r), grid (h ∈ {30, 50, 100, 150, 200, 300} m,
r ∈ {50, 100, 200, 500, 1000} m), seeds 1-50 per cell, and the CSV schema
are identical in both blocks. The LoS-state, shadow-fading, K-factor, and
ZSD columns of the FR2 rows equal the FR1 rows seed by seed (these do not
depend on the carrier in TR 38.901), which is the provenance check recorded
in `campaign/campaign_log_2026-10-04.md` together with the SHA-256 of every
campaign file.

## Verification status of this archive

Recorded on October 9, 2026 (Ubuntu 24.04, Python 3.11.15, numpy 2.4.4,
scipy 1.17.1, matplotlib 3.10.9): all 48 checks pass (`logs/`); the pack
comparator regenerates the authors' August report from the raw FR1 file
with every value identical, and produces the 28-GHz report whose numbers are
those of Table 4; the independent comparator reproduces the same numbers;
all eight data figures regenerate from the frozen pack. The authors are
asked to repeat `bash tools/run_all.sh` on their own machine before
depositing the archive.

## What was changed relative to the authors' pack (flagged)

The pack `ch3_code_complete_v7` is included as uploaded by the authors
(zip SHA-256 `00e013370d4477662a480e330c00f63cd6f025f08f028fcdf222892a108de3be`),
with these exceptions, none of which touches the frozen implementation or
any number:

1. Byte-code caches (`__pycache__`) are omitted.
2. Six dissertation-chapter manuscript files are omitted from the public
   archive (`chapter3_rewritten.docx`, `chapter3_rewritten_4.pdf`,
   `discussion/chapter3_complete.{tex,pdf}`,
   `discussion/rewritten/chapter3_rewritten.{tex,pdf}`); the authors may add
   them back. The technical reports and the Stage-1 instantiation document
   are kept.
3. `campaign/pack_comparator/analyze_report_guarded.py` is a copy of
   `stage3/quadriga_kit/analyze_report.py` with three guards (marked
   `GUARD (archive copy)`) so that it also completes on a single-carrier
   report; sections 8-9 of the original assume the two FR1 carriers. The
   pack file itself is unchanged.
4. `article_figures/figsrc/make_figures_access.py` and
   `make_abstract_access.py` resolve the pack path relative to the archive
   (or from `CH3_PACK`) instead of an absolute path, and Fig. 4 reads the
   270-cell report (FR1 + FR2) instead of the FR1-only report. Both edits
   are marked `ARCHIVE PATH` in the scripts.
5. The pack's `README_BUNDLE.md` still carries the title "v6"; the folder
   and its contents are the authors' v7.

## Licensing

See `LICENSING.md`. Default proposal, to be confirmed by the authors before
deposit: MIT for code (`LICENSE`), CC BY 4.0 for data and documents
(`LICENSE-DATA-AND-DOCS`). QuaDRiGa itself is not included; only the raw
outputs produced by the authors' runs are.

## Citing

Cite the article and, for the archive, the DOI issued at deposit (see
`CITATION.cff`; the DOI field is filled in by the authors after the deposit
exists).
