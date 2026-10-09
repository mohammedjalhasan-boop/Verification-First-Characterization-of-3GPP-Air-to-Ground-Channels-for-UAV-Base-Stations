# Depositing this archive and citing it in the manuscript

Prepared October 9, 2026 for the authors of "Verification-First
Characterization of 3GPP Air-to-Ground Channels for UAV Base Stations".
Procedures below were checked against the current Zenodo help pages, the
GitHub documentation on Zenodo DOIs, the IEEE Author Center page on research
reproducibility, and the IEEE Access submission guidelines (all read on
October 8-9, 2026). Portal screens change; where the wording differs, follow
the screen.

## 1. Before you upload (authors' checklist)

1. Run the reproduction on your own machine from the zip as it will be
   deposited: unzip it to an empty folder, then
   `pip install -r implementation/ch3_code_complete_v7/requirements.txt`
   and `bash tools/run_all.sh` (Windows: run the commands inside
   `tools/run_all.sh` one by one from Git Bash, or use WSL). The script must
   end with `ALL STEPS COMPLETED`. If anything fails, stop and send the
   output back before depositing.
2. Confirm the licence choice in `LICENSING.md` (default: MIT for code,
   CC BY 4.0 for data and documents). If you change it, update `LICENSE`,
   `LICENSE-DATA-AND-DOCS`, `CITATION.cff`, `.zenodo.json`, and the Zenodo
   form consistently.
3. Decide whether the six dissertation-chapter files excluded from the
   public archive (listed in `README.md`, "What was changed") should be
   added back. The default is to leave them out: the article is derived from
   the chapter, and the chapter text itself is not needed to reproduce any
   result.
4. Do not edit anything under `implementation/ch3_code_complete_v7/stage2/uav_channel/`
   (frozen). If you change any other file, re-run
   `python3 tools/make_manifest.py` so that `MANIFEST.sha256` matches, then
   `python3 tools/make_manifest.py --verify`.
5. Keep the zip name `uavbs-a2g-verification-v1.0.0.zip`; the version
   string appears in the manuscript text and in `CITATION.cff`.

## 2. Deposit on Zenodo (recommended)

Zenodo (CERN; free; DataCite DOI; no subscription needed by readers) is
the repository the IEEE Author Center names for data sharing, and the one
reviewers can open without an account.

1. Go to https://zenodo.org and log in. Logging in with the corresponding
   author's ORCID iD is the simplest choice and ties the record to the
   ORCID that the IEEE Author Portal already requires.
2. Click the "+" in the header and choose "New upload".
3. Drag `uavbs-a2g-verification-v1.0.0.zip` onto the file area (a record
   may hold up to 100 files and 50 GB; this zip is about 11 MB). Optionally
   add `README.md` as a second file so it is readable on the record page
   without downloading the zip.
4. Digital Object Identifier: answer "No" to "Do you already have a DOI for
   this upload?" and click "Get a DOI now!". This reserves the DOI; it is
   registered only when you publish, and it is lost if you delete the draft.
   Copy the reserved DOI: you will paste it into the manuscript before
   submission.
5. Basic information: Resource type = Software. Title, description,
   keywords, version (1.0.0) and language are in `.zenodo.json`; paste them.
   Creators: add the four authors in the article's order, each with ORCID
   (use the ORCID search in the form) and affiliation "Al-Furat Al-Awsat
   Technical University". Publication date: the day you publish.
6. Licence: choose "MIT License" (the data/document licence CC BY 4.0 is
   stated inside the archive and in the description; Zenodo allows only one
   licence field per record).
7. Visibility: Public. Metadata is always public; choosing "Restricted"
   would hide the files from reviewers, which defeats the purpose. If the
   authors prefer to hide the files until the article is accepted, the
   alternative is "Restricted" with an embargo date, and the files become
   public automatically at that date; this is not recommended for a
   verification-first paper.
8. Related works (optional now, required later): after the article has a
   DOI, add it here as "is supplement to". This can be done at any time,
   because Zenodo lets you edit metadata after publishing.
9. Click "Save draft", then "Preview", then "Publish" and confirm.
10. Record the DOI exactly as shown on the published page (form
    10.5281/zenodo.NNNNNNN). Files can be added, removed, or modified by the
    owner only within 45 days of publication; after that, changes require
    "New version" on the record page, which produces a new version DOI and
    keeps the old one valid. Cite the v1.0.0 DOI in the article so that
    reviewers see exactly the files that were checked.

### 2a. Alternative route through GitHub

If the authors prefer to keep the archive in a public GitHub repository:
sign in to Zenodo with the GitHub account, switch the repository on under
Zenodo's GitHub settings (the repository must be public), and create a
GitHub release tagged `v1.0.0`. Zenodo archives the release automatically,
issues a DOI, and reads `.zenodo.json` and `CITATION.cff` for the metadata.
Each later release gets its own DOI.

## 3. Alternatives and complements

IEEE DataPort (https://ieee-dataport.org) assigns a DOI and accepts code
and data; uploading a standard dataset is free at present, but its FAQ
states that downloading standard datasets requires a subscription (open
access datasets carry an up-front fee), so an anonymous reviewer may not be
able to open the files. Use it only in addition to Zenodo.

Code Ocean is IEEE's partner for executable code: authors who have
published with IEEE can create a capsule, choose "Yet To Be Published" with
the target journal (or enter the DOI once the article is in IEEE Xplore),
and submit it for publication; the article then shows a "Code & Datasets"
tab in IEEE Xplore where readers run the code. This is an optional step
after acceptance; it does not replace the DOI in the manuscript.

## 4. What to change in the manuscript once the DOI exists

Three places change: the Code and Data Availability section, note q of
Table 1, and the reference list. Replace the current "available from the
corresponding author upon reasonable request" sentence with the following,
inserting the real DOI (never submit with `NNNNNNN` in place):

```latex
\section*{Code and Data Availability}
The verified implementation (\texttt{uav\_channel}, frozen interface
v1.0.0), the three test suites that instantiate the 48 checks, the
executed QuaDRiGa campaign artifacts (the 13{,}500-link raw sample
files, the 270-cell reports, and the campaign log), the comparator and
analysis scripts, and the scripts that regenerate every figure and table
in this article from fixed seeds are archived as a single package under a
SHA-256 manifest on Zenodo~\cite{alhasan2026archive} (version 1.0.0,
DOI: 10.5281/zenodo.NNNNNNN). A one-command script reproduces the
checks, the cross-validation reports, and the figures.
```

Add to `refs.bib` (IEEE reference style for software/data; it renders
through the template's `IEEEtran.bst`):

```bibtex
@misc{alhasan2026archive,
  author       = {Alhasan, Mohammed J. and Shakir, Wafaa M. R. and
                  Daghal, Asaad S. and Salman, Fahama Hassoon},
  title        = {Verification-first {3GPP} air-to-ground channel
                  implementation for {UAV} base stations: Code,
                  verification suites, and {QuaDRiGa} campaign data},
  howpublished = {Zenodo},
  year         = {2026},
  note         = {Version 1.0.0. [Online]. Available:
                  https://doi.org/10.5281/zenodo.NNNNNNN},
}
```

Table 1, note q, currently reads "Reproducible package under a SHA-256
manifest; available from the corresponding author on request at the time of
writing (Code and Data Availability)". Replace it with:

```latex
$^{\mathrm{q}}$\,Reproducible package under a SHA-256 manifest, publicly
archived with a DOI~\cite{alhasan2026archive} (Code and Data Availability).
```

Then rebuild (`pdflatex`, `bibtex`, `pdflatex` ×3), check that the new
reference receives a number consistent with its first citation (note q of
Table 1 appears before the Code and Data Availability section, so the entry
will be numbered by that first appearance; IEEEtran numbers by citation
order automatically), and remove the
remaining "AUTHOR ACTION REQUIRED — archive" note from the working copy.
No other sentence in the article needs to change: Sections I, IV, and VIII
already say "available as stated in the Code and Data Availability
section". Do not describe the archive as "peer reviewed" or as "validated
against measurements"; it is a verification record.

## 5. What to enter in the IEEE Author Portal

1. Supplementary material: IEEE Access accepts supplementary files for
   review ("have any supplementary material for review ready for the
   submission process"). Upload `README.md` and, if the portal accepts the
   size, the archive zip itself, so that reviewers have the files even
   before they follow the DOI. The DOI in the manuscript remains the
   citable location.
2. Cover letter: add one sentence, for example "The complete verified
   implementation, the 48-check verification suites, the executed QuaDRiGa
   campaign data (13,500 links), and the scripts regenerating every figure
   and table are publicly archived with a DOI (10.5281/zenodo.NNNNNNN) and
   reproduce with a single command."
3. Nothing else changes: article type Regular Manuscript / Research
   Article, nine keywords, ORCID of the submitting author linked to the
   account.

## 6. After acceptance

Add the article's DOI to the Zenodo record as a related identifier ("is
supplement to"), update `CITATION.cff` (`identifiers` and the article
reference) in a new version if you wish, and, optionally, publish a Code
Ocean capsule linked to the article.
