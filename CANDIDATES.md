# CANDIDATES.md: research directions, ranking, and the prompts for the first run

Written in S0 (2026-10-08). None of these aims at a proof of P ≠ NP or P = NP. Each is meant to
produce a checkable result: a Lean theorem, a certified computation, or a pre-registered
refutation. Probabilities are S0's honest estimates, not measurements. "New" means not found in
`LANDSCAPE.md` after the searches recorded in `sessions/S0-evidence/`; every novelty claim below
must be re-checked by the Theorist with sources before the ledger entry is created. Barrier
answers refer to the table in `BARRIERS.md` Section 5.

## C1. Certified exact circuit complexity of small explicit functions (SAT, LRAT, Lean)

**Question.** For a fixed family of explicit Boolean functions f_n (first MOD3_n, then MAJ_n,
SUM_n mod 4, and the middle bit of n-bit multiplication) and the full binary basis B2 (then the De
Morgan basis U2), determine the exact minimum circuit size s(f_n) for every n up to the solver
frontier (expected n ≤ 6, possibly 7), where the lower bound "no circuit of size s(f_n) − 1 computes
f_n" is a Lean theorem obtained by replaying an LRAT certificate through Lean's verified checker,
and the upper bound is an explicit circuit checked by evaluation in Lean.

**Why it might be new.** `LANDSCAPE.md` 6.3: no published result certifies a circuit lower bound for a
specific function with an independently checked UNSAT certificate, and no machine-checked exact
circuit complexity value for any specific function was found. Knuth's 5-input table and the SAT
synthesis literature (Kojevnikov-Kulikov-Yaroslavtsev 2009; Kulikov-Pechenev-Slezkin 2022) give
values or bounds without certificates. The values for n = 6 and 7 of these families may be new
numbers; the certified statement is new as an artifact in any case.

**Barriers.** Not applicable in substance: the result is finite and says nothing asymptotic.
Record in the table: R1 to R5 "not applicable, finite statement"; N1 to N6 "not applicable, no
property of truth tables is claimed for large n"; A1 to A5 "not applicable". The honest limitation
is the opposite one: exact values for n ≤ 7 cannot support any asymptotic conjecture, and the
write-up must not suggest they do.

**Python experiment (falsifies fast).** Implement the Kojevnikov-Kulikov-Yaroslavtsev encoding
("there is a circuit with s gates over B2 computing f on all 2^n inputs") with symmetry breaking
(gate ordering, normal form for gate functions), run CaDiCaL through PySAT with LRAT output, and
verify every UNSAT answer with an independent checker (cake_lpr or drat-trim then lrat-trim) before
anything is trusted. Falsification: if MOD3_6 (or the smallest unsolved case of the family) does not
reach an UNSAT certificate for s = s_known − 1 within 48 CPU-hours, or if the certificate exceeds
50 GB, the frontier is below the interesting cases and the candidate is dead as stated.

**Lean route.** (1) Define straight-line programs over B2 in Lean with `eval`, `size`; (2) state
`∀ C : Circuit n, C.size ≤ s → C.eval ≠ f_n`; (3) prove it by reflection: a Lean-checked lemma that
the CNF encoding is sound ("if some circuit of size ≤ s computes f then the CNF is satisfiable"),
plus the UNSAT theorem for that CNF imported with `lrat-catcher` or `bv_check` (`LANDSCAPE.md`
6.2; the trusted base then includes the Lean compiler through `Lean.ofReduceBool`, which must be
stated). The soundness lemma is the only mathematics; it is finite but fiddly.

**Kill criterion (fixed now).** Dead if any of: (a) the Python experiment's 48-hour cap is hit for
every family at n = 6; (b) the soundness lemma cannot be completed within 10 Formalizer sessions;
(c) a certified value contradicts Knuth's 5-input table for a 5-input member of a family (then the
encoding is wrong, and every result is withdrawn until the bug is found).

**Estimates.** P(new certified artifact) 0.7; P(a new exact value for n ≥ 6 of some family) 0.4;
P(new mathematical insight) 0.05. Cost: 2 to 4 weeks of sessions, no new installs (CaDiCaL ships
with Lean; PySAT is already present per `D:\PvsNP\PHASE4.md`).

## C2. Exact proof complexity of small hard instances across proof systems, with certificates

**Question.** For PHP_{n+1}^n, Tseitin on small 3-regular graphs, and the mutilated chessboard, compute
the exact minimum refutation size in resolution, regular resolution, and tree-like Res(⊕), for the
largest instances the method reaches, with "no refutation of size < s" certified by an UNSAT
certificate of the meta-encoding checked by a verified checker, and tabulate the ratios between
systems.

**Why it might be new.** Peitl-Szeider (JAIR 2021) computed resolution hardness numbers for formulas
with up to ten clauses, and Sidorov et al. (2025) improved the search; no exact values for specific
PHP or Tseitin instances, and nothing for Res(⊕), were found (`LANDSCAPE.md` Section 2).

**Barriers.** Not applicable (finite). The known asymptotic separations (resolution versus Res(⊕) on
Tseitin) are the sanity check, not the result.

**Python experiment.** Reuse the Peitl-Szeider encoding (shortest resolution proof of size s as a
SAT instance) for PHP_4^3, PHP_5^4, Tseitin on 4 to 8 vertices; kill if PHP_5^4's exact resolution
size does not resolve within 72 CPU-hours. Res(⊕) needs a new encoding (linear clauses over F2);
budget one Experimenter session to write and fuzz it against brute force on tiny instances.

**Lean route.** State the minimum-size claims as theorems about an inductive definition of
resolution and Res(⊕) proofs over concrete clause sets and prove them by the same reflection
pattern as C1. This is the natural downstream use of C6's library.

**Kill criterion.** Dead if the meta-encoding does not reach PHP_5^4 in resolution or any Tseitin
instance with 6 vertices in tree-like Res(⊕) within the caps, or if any certified value disagrees
with a brute-force value on an instance small enough to brute-force.

**Estimates.** P(new certified data) 0.5; P(new theorem) 0.05. Cost: 3 to 5 weeks.

## C3. Formal audit, then reuse, of the 2026 AI-assisted Lean lower-bound formalizations

**Question.** (i) Does the Lean statement `MathResearch.bitPHP_exponential` in
`github.com/kbr-/math-research` at commit `8904bf09` faithfully state "every DAG-like Res(⊕)
refutation of bit-PHP with n+1 pigeons and 2^ℓ holes has more than exp(n/(32768 ℓ^2)) clauses"
under a standard definition of Res(⊕), with only the three standard axioms, and does
`leanchecker --fresh` pass on a clean machine? (ii) Same for `formalcs/circuit-complexity`
release/0.1.0 (parity against AC^0). (iii) If (i) survives, can Braun's "generic sufficient
condition" (Theorem 9.1) be instantiated in Lean on a second formula family (Tseitin or ordinary
PHP), giving a new certified Res(⊕) lower bound?

**Why it might be new.** Both artifacts are September 2026 single-author preprints with no audit
known to this lab (`LANDSCAPE.md` 6.1). A found gap is a new negative result; a confirmed audit is
the first independent check of a claimed resolution of an open problem (dag-like Res(⊕) lower
bounds were open per Braun's own survey of prior work); (iii) would be a new theorem.

**Barriers.** Proof-complexity lower bounds for Res(⊕) are not subject to the three barriers; the
table is filled "not applicable" with that reason. The relevant risk is the one the Breaker role
exists for: a formal statement that does not say what the paper says (degenerate definition of
Res(⊕), a vacuous size measure, hidden hypotheses, `sorry` behind an alias).

**Python experiment.** None needed for (i) and (ii). For (iii), a brute-force Res(⊕) size search on
tiny Tseitin instances to sanity-check the instantiated bound.

**Lean route.** Clone both repositories into `lean/external/` (read-only), build with their pinned
toolchains on D:, run `#print axioms`, `leanchecker --fresh`, the banned-token scan from
`D:\Relativization\logs\redteam-src-rt_scan.py.txt`, and write a `bgs_explicit`-style independent
restatement (as in `D:\Relativization\REDTEAM.md` Section 1.2) of the main theorem on plain
inductive definitions, proved from the repository's theorem.

**Kill criterion.** The audit itself cannot be killed; (iii) is dead if the sufficient condition
cannot be instantiated on Tseitin within 6 Formalizer sessions or if the instantiated bound is
trivial (polynomial).

**Estimates.** P(audit completes with a definite verdict) 0.8; P(a gap is found) 0.3 (base rate for
unreviewed AI-assisted formal claims is unknown; this is a guess); P(new theorem from (iii)) 0.15.
Cost: 1 to 2 weeks for (i) and (ii); disk about 7 GB per toolchain cache on D:.

## C4. Certified exact monotone complexity of small graph functions

**Question.** Exact monotone circuit size (AND/OR) of triangle detection, k-clique for k = 3, 4, and
perfect matching on graphs with up to 6 or 7 vertices, with certified lower bounds as in C1, and a
test of whether any notion of "random monotone function" makes the known lower bounds large in the
RR94 sense at these sizes.

**Why it might be new.** No SAT-computed exact monotone sizes for these functions were found
(`sessions/S0-evidence/` search notes). Monotone lower bounds are the one area where RR94 admits no
formal largeness analogue, and Thiemann's AFP entry shows a monotone bound can be formalized.

**Barriers.** Natural proofs: not applicable to monotone models per RR94 p. 3; relativization and
algebrization: not applicable (finite). Honest limit: nothing asymptotic follows.

**Python experiment.** Same pipeline as C1 with the gate basis restricted to {AND, OR}; kill if
triangle detection on 6 vertices (15 inputs) does not certify within 72 CPU-hours.

**Lean route.** As C1, monotone basis.

**Kill criterion.** As the experiment; also dead if the exact values are already in the exact
synthesis literature (the Theorist must check Haaswijk et al. and Knuth 7.1.2 before starting).

**Estimates.** P(new certified data) 0.4; P(insight) 0.05. Cost: 2 to 3 weeks after C1 exists.

## C5. Relativization toolkit in Lean: close the gaps and mechanize the relativization test

**Question.** (i) Prove `ComputablePred (· ∈ A)` and `(· ∈ B)` for decidable variants of the
`D:\Relativization` witnesses (finding F1), formalize Theorem 2 (PSPACE-complete oracle) and the
equivalence of verifier-defined and nondeterministic-machine NP^X (F2). (ii) Build the
relativization test of `BARRIERS.md` 1.5 as a reusable procedure, and run it on the Cook-Levin
development of `D:\PvsNP` to name the first lemma that does not generalize over an oracle (the
formal content of "Cook-Levin is non-relativizing" in the Arora-Impagliazzo-Vazirani sense, which
`BARRIERS.md` could only cite second-hand).

**Why it might be new.** No other formalization of resource-bounded oracle machines or of
Baker-Gill-Solovay was found (`LANDSCAPE.md` 6.1); a mechanized "which lemma fails to relativize"
analysis of a formal Cook-Levin proof was not found either.

**Barriers.** This candidate is about the barrier itself; the table is filled with "the deliverable
is the test, R1 to R3 are its output".

**Python experiment.** None. **Lean route.** Direct: the code base exists; the port is a copy into
`lean/` of this lab with the source commit recorded.

**Kill criterion.** (i) dead if decidability of the collapse oracle needs more than 8 Formalizer
sessions; (ii) dead if the parametrization produces no informative failure (every lemma generalizes,
which would mean the formal Cook-Levin proof relativizes, itself a finding worth a ledger entry).

**Estimates.** P(new artifact) 0.7; P(new theorem) 0.05. Cost: 3 to 6 weeks.

## C6. A Lean library for propositional proof systems and the first machine-checked resolution lower bound

**Question.** Define Cook-Reckhow proof systems, p-simulation, resolution (tree-like, regular, dag-like)
and Res(⊕) in Lean 4 against Mathlib; prove the Ben-Sasson-Wigderson size-width relation and a width
lower bound for PHP (or Tseitin on an expander), obtaining a machine-checked superpolynomial
resolution lower bound.

**Why it might be new.** `LANDSCAPE.md` 6.1: no formalized Haken-type resolution lower bound was
found in any proof assistant. Braun's repository (C3) contains polynomial calculus and Res(⊕)
definitions whose reuse must be evaluated first.

**Barriers.** Not applicable (known theorems; novelty is the formalization).

**Python experiment.** None. **Lean route.** Statement-first per the ledger rules; the size-width
relation is the milestone that the Breaker must match against the paper statement.

**Kill criterion.** Dead if the size-width relation is not proved within 12 Formalizer sessions.

**Estimates.** P(success) 0.6; P(new theorem) 0.0. Cost: 1 to 3 months. Value: infrastructure for
C2 and C3.

## Ranking and the first two to run

| Rank | Candidate | Reason |
|---|---|---|
| 1 | C1 | Required deliverable type; cheapest full exercise of the SAT, certificate, Lean pipeline; no barrier exposure; reusable encoding and soundness lemma for C2 and C4; highest probability of a checkable new artifact. |
| 2 | C3 | Time-sensitive (three-week-old unaudited claims on an open problem), very cheap, uses exactly the red-team methodology the lab already has, and either outcome is a result. |
| 3 | C2 | Extends C1's pipeline to proof complexity with new-data potential. |
| 4 | C6 | Long but foundational; start in parallel if a Formalizer is free. |
| 5 | C5 | Valuable for the barrier methodology; lower urgency. |
| 6 | C4 | After C1's tooling exists. |

Decision taken in S0 without the director: C1 first and C3 second. The alternative (C6 second)
loses because C3's information value decays and C6 takes months.

## Prompts for the first candidate (C1)

Each prompt is self-contained. Replace `<N>` with the session number. The Breaker prompt must be
sent without the Theorist's session report or reasoning attached.

### Theorist prompt

You are the Theorist of the lab in `D:\PvsNP-Research` (read `README.md`, `LEDGER.md`,
`BARRIERS.md` Section 5, `CANDIDATES.md` C1). Session S<N>, model `claude-fable-5-1`. Do not run
experiments and do not write Lean proofs. Deliver, in `sessions/S<N>-theorist.md` and as new
ledger entries CLAIM-01xx: (1) the exact definitions: circuit over B2 as a straight-line program,
size, the function families MOD3_n, MAJ_n, SUM_n mod 4, MUL middle bit, with n ranges; (2) the SAT
encoding E(f, n, s) in full (variables, clauses, symmetry breaking) and a paper proof of its
soundness ("if a size-s circuit computes f then E(f,n,s) is satisfiable") and completeness; (3) a
ledger entry per family with the statement "s(f_n) = v_n for n in range" left as unknown values, a
kill criterion copied from C1, and the barrier table filled "not applicable" with reasons; (4) a
novelty check: read Kojevnikov-Kulikov-Yaroslavtsev (SAT 2009), Kulikov-Pechenev-Slezkin (arXiv
2102.12579), Knuth 7.1.2 as summarized at cp4space, and Haaswijk et al. (TCAD 2020), and record
every exact value they already give for these families; (5) the list of every number and citation
in your report with its verification tag. No em-dashes. Do not claim anything is new without the
sources read in this session.

### Breaker prompt

You are the Breaker of the lab in `D:\PvsNP-Research`. Session S<N>, model `claude-opus-5-5`,
which is deliberately a different model from the Theorist's `claude-fable-5-1`; record both ids in
your attack record. You receive only: the ledger entries CLAIM-01xx (statement, kill criterion,
barrier table, evidence list), the file `experiments/c1/PLAN.md`, and the encoding specification
`experiments/c1/ENCODING.md`. You must not read `sessions/S<N'>-theorist.md` or any Theorist
report, and you must not ask for the Theorist's reasoning; if it is attached, stop and report the
breach. Attack the claims: (a) find a degenerate reading of the statement (size measure, basis,
constant inputs, output gate conventions, functions of fewer than n variables) under which the
claimed values would be trivially true or false; (b) check the encoding's soundness proof for a
missing clause or an unsound symmetry-breaking rule by constructing, by hand or by brute force in
Python, a circuit that the encoding would reject or a non-circuit it would accept, for n ≤ 3; (c)
check every exact value against Knuth's 5-input table where applicable; (d) search the literature
for the claimed values. Write `sessions/S<N>-breaker.md` with everything tried, including attacks
that failed, in enough detail to be repeated, and append the attack record to the ledger entries.
Do not change any status. No em-dashes.

### Experimenter prompt

You are the Experimenter of the lab in `D:\PvsNP-Research` (read `README.md`, `LEDGER.md`,
`CANDIDATES.md` C1 and the ledger entries CLAIM-01xx). Session S<N>, model `claude-fable-5-1`.
First write `experiments/c1/PLAN.md` before any run: families, n ranges, the size values to test
(s_known − 1 for the lower bound, s_known for the upper bound), seeds, solver (CaDiCaL via PySAT,
version quoted), LRAT output, time cap 48 CPU-hours per instance, certificate size cap 50 GB, the
checkers (cake_lpr or drat-trim plus lrat-trim; quote versions), the kill criteria copied from C1,
and your predictions. Then implement the encoding exactly as specified in
`experiments/c1/ENCODING.md`, fuzz it against brute-force enumeration of all circuits of size ≤ 4
on n ≤ 3 inputs (every discrepancy stops the session), and only then run the plan. Every UNSAT
answer must have a certificate checked by the independent checker with its exit status quoted; every
SAT answer must have its circuit re-evaluated on all 2^n inputs by a separate script. Record
instance hashes, raw outputs under `experiments/c1/raw/` (git-ignored) and a summary table in
`experiments/c1/RESULTS.md`. Report in `sessions/S<N>-experimenter.md`: what ran, what did not,
cap hits, and which kill criteria fired. Python output is evidence, never proof. Nothing on C:;
check free space before large runs. No em-dashes.

### Formalizer prompt

You are the Formalizer of the lab in `D:\PvsNP-Research` (read `README.md`, `LEDGER.md`,
`CANDIDATES.md` C1, `experiments/c1/ENCODING.md`, `experiments/c1/RESULTS.md`). Session S<N>,
model `claude-fable-5-1`. Create `lean/c1/` as a Lake project on D: pinned to
`leanprover/lean4:v4.31.0` and Mathlib `fabf563a7c95a166b8d7b6efca11c8b4dc9d911f`; ask the director
before any other toolchain (record the question in your report and stop if `lrat-catcher` needs a
newer Lean). State in the ledger, before proving: (1) `Circuit n`, `Circuit.eval`, `Circuit.size`;
(2) `encoding_sound : (∃ C : Circuit n, C.size ≤ s ∧ C.eval = f) → Satisfiable (E f n s)`;
(3) for each certified instance, `unsat_E : ¬ Satisfiable (E f n s)` obtained from the LRAT file
through Lean's verified checker (`bv_check` or `lrat-catcher`), with the trusted base stated
(`Lean.ofReduceBool`); (4) the lower-bound theorem derived from (2) and (3); (5) the upper bound by
`decide` or `native`-free evaluation of the explicit circuit (no `native_decide`). Banned tokens as
in `README.md` rule 11. Report `lake build` output, `#print axioms` for every theorem, and
`leanchecker` output verbatim in `sessions/S<N>-formalizer.md`. Never promote a status. No em-dashes.
