# Licensing (proposal — the authors confirm before deposit)

The authors hold the copyright of everything in this archive. The licence
choice is theirs; the two files below implement the most common pairing for
research archives and were prepared as a default. Replace them if the
authors or Al-Furat Al-Awsat Technical University prefer other terms.

| Material | Proposed licence | File |
|---|---|---|
| Source code (`implementation/**/*.py`, `*.m`, `campaign/*.py`, `article_figures/figsrc/*.py`, `tools/*`) | MIT License | `LICENSE` |
| Data (`campaign/*.csv`, `*.json`, `implementation/**/out*/**`, `discussion/*.csv`) and documents (`*.md`, `*.pdf`, `*.tex`, figures) | Creative Commons Attribution 4.0 International (CC BY 4.0) | `LICENSE-DATA-AND-DOCS` |

Why this pairing: MIT lets other groups reuse the implementation (including
inside commercial simulators) with attribution and no copyleft obligation;
CC BY 4.0 is the licence Zenodo and most funders recommend for data and
makes the campaign files citable with attribution. Both are on Zenodo's
licence list (`MIT`, `CC-BY-4.0`) and both are accepted by IEEE for
supplementary material.

Third-party material: none is redistributed. QuaDRiGa (Fraunhofer HHI) is
not included; `campaign/*.csv` are outputs produced by the authors' own
runs of QuaDRiGa 2.8.1 and are the authors' data. The 3GPP technical
reports are cited, not reproduced; `stage2/uav_channel/params.py`
transcribes coefficient tables, which are facts of the standard.

If the authors choose different terms, update `LICENSE`,
`LICENSE-DATA-AND-DOCS`, the `license` fields of `CITATION.cff` and
`.zenodo.json`, and the Zenodo deposit form consistently.
