# S0: founding session report

- Date: 2026-10-08. Role: founding session (no research role). Model: `claude-fable-5-1`.
- Scope: set up the lab; no proof attempt, no experiment, no Lean build (per the director's brief).
- Commits: none. `git status` at the end of the session shows every file untracked;
  `git log` reports no commits.

## What was done

1. Workspace: `git init`, `core.autocrlf=false`, `core.eol=lf`, `.gitattributes` (`* text=auto eol=lf`),
   `.gitignore` (`.lake/`, compiled Lean files, Python caches, instances, proof logs, archives,
   binaries). Verified by running `git config --get core.autocrlf` (output `false`) and
   `git config --get core.eol` (output `lf`).
2. Read the background repositories (read-only): `D:\Relativization` (`README.md`, `DEFINITIONS.md`,
   `REDTEAM.md` sections 1c, 1d, 6, findings and verdict, `Relativization.lean`,
   `BakerGillSolovay.lean`, `claude.md`, `lakefile.toml`) and `D:\PvsNP` (`README.md`,
   `DEFINITIONS.md`, `claude.md`, `PHASE3.md` lines 1 to 250, `PHASE4.md` lines 1 to 150, kill
   criteria grep: K1 did not fire, K2 to K5 fired). Nothing in either repository was edited.
3. Wrote `LEDGER.md` (statuses, promotion rules, template, inherited entries CLAIM-0001 to 0004,
   candidate entries CLAIM-0100 to 0105).
4. Wrote `BARRIERS.md` from primary sources read in this session:
   - Baker, Gill, Solovay 1975: text-layer PDF of the SIAM printing downloaded from
     `cse.ucdenver.edu/~cscialtman/complexity/`, extracted with `pdftotext`, pp. 431-442 read in full.
   - Razborov, Rudich: ECCC TR94-010 PDF (bitmap fonts) rendered to page images with PyMuPDF
     (`python -P -E`, user site-packages) and pages 1 to 13 read as images.
   - Aaronson, Wigderson: the authors' PDF (`scottaaronson.com/papers/alg.pdf`) extracted with
     `pdftotext`; abstract, Sections 1, 2, 5 (theorem statements), 9, 10, 11 read.
   - Citation data for five papers verified through `api.crossref.org`.
   Failed routes, recorded for the next session: `people.cs.uchicago.edu/~razborov/files/natural.pdf`
   is a 404 (the site links a PostScript `int.ps`); `epubs.siam.org` returned 403; ScienceDirect 403;
   ACM DL PDF 403; `api.semanticscholar.org` rate-limited; no Ghostscript, `pdftoppm`, or ImageMagick
   on the machine or in WSL (Ubuntu 24.04.2).
5. Launched four literature subagents (same model) with a strict provenance rule; their hand-back
   reports are stored verbatim in `sessions/S0-evidence/` (`survey-circuits.txt` was delivered
   inline and copied by hand; the other three are the persisted files). Budgets used: 30/35, 29/40,
   33/40, 29/35 (search/fetch) calls.
6. Verified directly, by fetching abstract pages or PDFs, the 2025 to 2026 items that matter for
   the candidates: arXiv 2609.24188 (Wesonga, Lean parity lower bound; repository
   `github.com/formalcs/circuit-complexity`, Lean 4.33.1), arXiv 2609.23015 (Braun, Res(⊕)
   dag-like bound; repository `github.com/kbr-/math-research`, commit `8904bf09`, Lean 4.34.0-rc2,
   Appendix B reproduction commands read from the PDF text), arXiv 2607.00815 (lrat-catcher),
   Lean 4.12.0 release notes (`bv_decide`, `Lean.ofReduceBool`), arXiv 2606.12631 (unconditional
   AC^0-natural barrier), arXiv 2511.14038 (new algebrization barriers), ECCC TR24-113 (Vyas,
   Williams), ECCC TR26-008 (Raz), the 2024 Gödel Prize citation (sigact.org),
   `github.com/Quantyra/formal-switching-lemma` README. Locally verified: the pinned Mathlib package
   in `D:\Relativization\.lake` contains `proof_wanted TM2ComputableInPolyTime.comp` at
   `Mathlib/Computability/TuringMachine/Computable.lean:284`.
7. Wrote `LANDSCAPE.md`, `CANDIDATES.md` (six candidates, ranking, four role prompts for C1),
   `README.md`.

## What was not done, and why

- No Lean project was created (nothing to prove; a project would download about 7 GB of Mathlib
  artifacts). No Python experiment was run.
- `LANDSCAPE.md` Section 2 summarises the proof-complexity survey; the survey itself (55 KB) is in
  `sessions/S0-evidence/survey-proof-complexity.txt` and was not line-by-line cross-checked against
  the summary after the persisted copy arrived (the inline copy it was written from had the same
  section structure and claims).
- The quotes in the subagent reports are limited to about 125 characters each by the fetch tool;
  numbers inside long abstracts were sometimes not captured and are tagged accordingly.
- Commands claimed in this report were run; outputs are quoted where they matter. No build of the
  background repositories was re-run; their `#print axioms` outputs are taken from their READMEs
  and tagged so in the ledger.

## Decisions taken without the director (also listed in README.md)

- Breaker default model `claude-opus-5-5` versus Theorist `claude-fable-5-1`.
- C1 first, C3 second (CANDIDATES.md gives the reasoning).
- Repository layout: `sessions/`, `experiments/`, `lean/` conventions.

## Single most promising next step

Run C1's Theorist session (prompt in `CANDIDATES.md`): fix the circuit model, the SAT encoding and
its paper soundness proof, and record the exact values already published for the 5-input members
of each family. In parallel, a Breaker session on CLAIM-0102 (clone `kbr-/math-research` at
`8904bf09` into `lean/external/` on D:, build with its pinned toolchain, run the repository's
`verify.py` and `leanchecker --fresh`, and write the independent restatement).

## Numbers and citations in this report, with tags

- Disk free at start (PowerShell `Get-PSDrive`): D: 270.5 GB, C: 10.7 GB. Verified (run).
- Phase 4 figures quoted in CLAIM-0003 (536 of 1881 runs; F1 n = 250 rows; K1 to K5): read from
  `D:\PvsNP\PHASE4.md`. Verified (read).
- BGS 1975 citation: Crossref. Verified. RR 1997 JCSS citation: Crossref. Verified. STOC 1994 pages
  204-213: unverified (search snippet). AW STOC 2008 and TOCT 2009 citations: Crossref. Verified.
  IKK 2009, AB 2018 citations: Crossref. Verified; contents unverified.
- Lean/Mathlib versions of the background repositories: read from their lakefiles and READMEs.
  Verified (read). Wesonga's Lean 4.33.1 and Braun's Lean 4.34.0-rc2: read from their PDFs. Verified.
- All other citations: see the tags in `LANDSCAPE.md` and `BARRIERS.md`.
