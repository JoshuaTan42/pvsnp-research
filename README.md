# PvsNP-Research: a research lab on P vs NP, run under adversarial rules

Founded 2026-10-08 (session S0). Workspace `D:\PvsNP-Research`. Background repositories, read-only
and never edited by this lab: `D:\PvsNP` (Cook-Levin formalized against LeanMillenniumPrizeProblems;
Phases 3 and 4: one algorithmic candidate for SAT ∈ P screened and falsified) and
`D:\Relativization` (Baker-Gill-Solovay formalized in Lean 4).

The lab does not try to prove P ≠ NP or P = NP. It tries to produce checkable results near the
problem: certified small-case lower bounds, machine-checked proof-complexity and relativization
results, and pre-registered falsifications of candidate approaches. The working assumption is
P ≠ NP and that no current technique settles it.

## Files

| File | What it is |
|---|---|
| `README.md` | this file: rules, roles, session protocol |
| `LEDGER.md` | every claim, its status, evidence, attacks and kill criterion; the promotion rules |
| `BARRIERS.md` | relativization, natural proofs, algebrization: exact statements, sources read, what the Lean theorem `baker_gill_solovay` shows, and the checklist every approach must fill |
| `LANDSCAPE.md` | literature survey of lower-bound techniques and proof-complexity frontiers, every claim tagged verified or unverified |
| `CANDIDATES.md` | the research directions, ranked, with kill criteria, and the role prompts for the first candidate |
| `sessions/` | one report per session, `S<N>-<role>.md` (S0 is this founding session) |
| `experiments/` | pre-registered experiments; each has `PLAN.md` written before any run, `src/`, and small result tables; instances and raw outputs are git-ignored |
| `lean/` | Lean 4 projects of this lab (none yet); toolchain pinned to the background repositories' versions |

## Honesty rules (binding for every session and every role)

1. **No claim of having proved P vs NP or any open result** without a Lean-checked proof and an
   independent attack, in a different session and on a different model, that failed. Even then the
   claim is stated with its exact Lean statement and its `#print axioms` output, never in prose alone.
2. **No claim of novelty** without a literature check done from sources actually read in that
   session, recorded with URL or file path. "I believe this is new" is not a novelty check.
3. **Every number, theorem name and citation is verified or marked `unverified`.** Verified means:
   the number was reproduced from a script in this repository, or the source was read in the session
   that wrote the sentence. Secondary sources are named as such.
4. **Never say a command ran unless it ran; quote its output.** Python output is evidence, never proof.
5. **State before proving.** Every theorem is written in the ledger (or the session's notes) before
   its proof is attempted; every experiment's kill criterion is written before the first run.
6. **Kill criteria are fixed in advance** and are not changed after a run has started. A changed
   criterion is a new ledger entry.
7. **No em-dashes** in any file of this repository.
8. **Nothing is installed on C:.** Toolchains, caches, clones and data go on D:. Check free space
   before any download or build over 1 GB and stop if it would leave less than 10 GB free on D:.
9. **The background repositories are read-only.** Copy what is needed into this workspace and
   record the source commit.
10. **Sessions do not commit.** The director (the user) commits after reviewing the session report.
    A session may stage nothing and must leave the working tree in a state that `git status`
    explains.
11. **Banned Lean escape hatches** in anything marked proved: `sorry`, `admit`, `axiom`,
    `native_decide`, `decide +native`, `implemented_by`, `extern`, `unsafe`, `partial`,
    environment-modifying metaprogramming (`run_cmd`, `addDecl`), `open private`, `set_option debug.*`.
    `#print axioms` must list only `propext`, `Classical.choice`, `Quot.sound`. Checks go through
    `lake build`, `#print axioms` and `leanchecker`.
12. **Separation of reasoning from evidence.** The Theorist's reasoning is never shown to the
    Breaker. The Breaker receives the claim statement, the evidence files and the kill criterion only.

## Roles

Each session runs one role, declared at the top of its report together with the model id it ran on.
Model ids known to this lab: `claude-fable-5-1` (S0 ran on it), `claude-opus-5-5`, `claude-sonnet-5-5`.

**Theorist.** Proposes claims. Writes the precise statement, the argument, the filled barrier table
from `BARRIERS.md` Section 5, the novelty check with sources, the kill criterion, and the evidence
needed for each status. Never promotes a claim. Never attacks their own claim.

**Breaker.** Attacks one claim per session. Runs on a model different from the one that produced
the claim (the ledger records both ids). Receives only the claim statement, the evidence files and
the kill criterion, never the Theorist's reasoning or session report. Tries, at minimum: degenerate
readings of the statement, counterexamples at small sizes (by computation where possible), the
barrier checklists, and a search for the result in the literature. Writes an attack record for the
ledger with everything tried, whether it worked or not. A Breaker report that says "no gap found"
must list what was tried in enough detail that a later session could repeat it.

**Experimenter.** Runs pre-registered experiments. Writes `experiments/<name>/PLAN.md` (families,
sizes, seeds, cost measure, time caps, kill criteria, predictions) before any run, then runs exactly
that plan. Every SAT answer is checked by an independent evaluator; every UNSAT answer carries a
certificate (DRAT, LRAT or PR) checked by an independent checker, and the checker's exit status is
quoted. Reports include instance hashes, seeds, and the environment. Exploratory runs are allowed
but are labelled exploratory and give no status.

**Formalizer.** Writes Lean 4. States each theorem in the ledger before proving it. Uses the
pinned toolchain (`leanprover/lean4:v4.31.0`, Mathlib `fabf563a7c95a166b8d7b6efca11c8b4dc9d911f`,
LeanMillenniumPrizeProblems `603053dc267cf3efe422f438eb78098c0ececd6f`, per the background
repositories; a change needs the director's approval). Reports `lake build` output, `#print axioms`
output and the scan for banned tokens. A Lean theorem enters the ledger as `proved on paper` at most
until a Breaker has matched its statement to the intended one.

**Director** (the user). Chooses which candidate runs, assigns roles and models, reviews session
reports, commits.

## Session protocol

1. Read `README.md`, `LEDGER.md`, and the candidate's section of `CANDIDATES.md`. A Breaker reads
   only what Rule 12 allows.
2. Declare role, model id, session id (`S<N>`, N increasing across all roles), and the ledger
   entries in scope.
3. Work. Write statements before proofs, plans before runs.
4. Update `LEDGER.md` (attack records, evidence, new entries). Never change a status in the same
   session that produced the evidence for it.
5. Write `sessions/S<N>-<role>.md`: what was attempted, what ran (with quoted outputs), what failed
   and why, the single most promising next step, and a list of every number and citation in the
   report with its verification tag.
6. Do not commit.

## Decisions made in S0 (the director was not available; the most reasonable choice was taken)

* The repository ignores `.lake/`, compiled Lean files, benchmark instances, proof logs
  (`.drat`, `.lrat`, `.dpr`, `.pr`, `.frat`), archives and binaries (`.gitignore`), and normalises
  line endings to LF (`.gitattributes`, `core.autocrlf=false`, `core.eol=lf`).
* The Breaker's default model is `claude-opus-5-5` when the Theorist ran on `claude-fable-5-1`,
  and the reverse. A non-Anthropic model would be a stronger separation and is preferred when
  available; the ledger records whatever was used.
* Ledger ids `CLAIM-0001` to `CLAIM-0099` are reserved for inherited results, `CLAIM-0100` upward
  for this lab's own claims.
* `experiments/` and `lean/` are conventions, not yet populated. No Lean project was created in S0
  (nothing to prove yet; creating one would download about 7 GB of Mathlib build artifacts).

## Environment facts recorded in S0 (verified by running the commands)

* Windows 11 Pro 10.0.26200; Git Bash and PowerShell available; `git` 2.52.0.
* Python 3.11.9 (Microsoft Store build) with `pypdf`, `PyPDF2` and PyMuPDF (`fitz` 1.23.16) in the
  user site-packages. `D:\PvsNP\PHASE4.md` reports PySAT 1.9.dev15 with CaDiCaL 1.9.5, CNFgen 0.9.6,
  networkx 3.2.1 and sympy 1.14.0 in this Python; not re-checked in S0.
* `pdftotext` on PATH (`/mingw64/bin/pdftotext`); no `pdftoppm`, no Ghostscript, no ImageMagick;
  `ffmpeg` 8.1.2 on PATH. WSL Ubuntu 24.04.2 exists with none of these.
* Lean: `elan`, `lean`, `lake` on PATH (`~/.elan/bin`), toolchain `leanprover/lean4:v4.31.0` used
  by both background repositories.
* Free space at the start of S0: D: 270.5 GB free, C: 10.7 GB free.
