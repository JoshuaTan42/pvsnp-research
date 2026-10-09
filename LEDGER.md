# LEDGER.md: the claims ledger

This file is the lab's single source of truth about what has been claimed, what has been
attacked, and what has survived. Every research claim made anywhere in this repository must
have an entry here before it is cited as evidence for anything else. Rules for the roles that
write to this file are in `README.md`.

Founding session: 2026-10-08 (session S0, model Claude Fable 5.1, `claude-fable-5-1`).
Nothing in S0 was proved or attempted; S0 only set up the lab.

## 1. Status scale

| Status | Meaning | Minimum evidence required to hold the status |
|---|---|---|
| `conjecture` | A precise statement someone wants to be true. | The statement itself, written so that a counterexample would be recognisable, plus a kill criterion fixed before any evidence is gathered. |
| `computationally supported` | A pre-registered experiment ran, its kill criterion did not fire, and the run is reproducible. | Script, seed, instance manifest with hashes, raw output, and the pre-registration text written before the run. A run that was not pre-registered counts as exploratory and confers no status. |
| `proved on paper` | A complete written proof exists in this repository and a Breaker in a different session failed to find a gap. | The proof file, the Breaker's report with the attacks tried, and the Breaker's session id. |
| `Lean-verified` | A Lean 4 theorem whose statement a Breaker has matched against the ledger statement, building with no `sorry`, `admit`, `axiom`, `native_decide`, `implemented_by`, `extern`, `unsafe`, `partial`, or environment-modifying metaprogramming, and whose `#print axioms` output lists only `propext`, `Classical.choice`, `Quot.sound`. | Commit hash, theorem name, the `#print axioms` output quoted verbatim, the build log, and the Breaker's statement audit (a restatement in plain machine terms and the degenerate readings ruled out). |
| `refuted` | A counterexample, a kill criterion that fired, or a gap in the proof that nobody could repair. | The refuting object (instance, run, or gap description), reproducible. Refuted entries are never deleted. |

Two further fields travel with every entry and are not statuses:

* `novelty`: one of `unchecked`, `checked` (with the sources read, in `LANDSCAPE.md` or in the
  entry), or `known` (with the citation that already contains the result). A claim may be
  `Lean-verified` and `known` at the same time; that is a formalization result, not a new theorem,
  and must be described as such.
* `scope`: whether the claim is about the formal objects of `LeanMillenniumPrizeProblems`
  (`Problems/PVersusNP/Millennium.lean`, Mathlib `FinTM2`), about a paper-level mathematical
  object, or about an experiment.

## 2. Promotion rules

1. **Different-session attack.** A status may be promoted only after an attack on the claim was
   made in a session different from the one that proposed the claim or produced the evidence, by
   the Breaker role, on a different model from the one that produced the claim (see `README.md`
   for the model rule), and that attack failed. "Failed" means the Breaker's report says it found
   no counterexample, no gap and no statement bug, after trying at least the attacks listed in the
   Breaker prompt for that candidate. The attack record (rule 4) is written before the status
   field changes.
2. **Order.** The only promotion paths are
   `conjecture -> computationally supported -> proved on paper -> Lean-verified`,
   `conjecture -> proved on paper -> Lean-verified` (claims that are not empirical),
   and `anything -> refuted`. Each arrow needs its own failed attack. A claim never goes directly
   from `conjecture` to `Lean-verified`: the Lean statement must first be matched to the paper
   statement by a Breaker, and that match is part of the `proved on paper` attack.
3. **Demotion.** Any role may demote a claim at any time by adding evidence (a counterexample, a
   gap, an unverifiable number). Demotion needs no second session. A demoted claim records why.
4. **Attack record.** Every attack, successful or not, is appended to the entry with: date,
   session id, role, model id, what was tried (families, sizes, proof steps checked, degenerate
   readings tried), and the outcome. An attack with no record did not happen.
5. **Kill criterion first.** The kill criterion is fixed when the entry is created, before any
   experiment or proof attempt. Changing it afterwards requires a new entry with a new id and a
   cross-reference; the old entry keeps the old criterion and is marked `superseded by`.
6. **Numbers and citations.** Every number in an entry is either reproducible from a script and
   data in this repository (path given) or carries the tag `unverified`. Every theorem name and
   citation is either checked against a source read in the session that wrote it (source given)
   or carries the tag `unverified`.
7. **No self-promotion.** The session that proposes a claim may not promote it, and a Theorist
   may not act as Breaker on their own claim in any session.
8. **Formalization does not confer novelty.** `Lean-verified` says the statement is true as
   stated in Lean. Whether the statement is the intended one is the Breaker's statement audit;
   whether it is new is the `novelty` field.
9. **External results.** Results from `D:\PvsNP` and `D:\Relativization` enter the ledger with
   the status their own documentation supports, and with their existing red-team reviews counted
   as attacks (they were made in separate sessions that had not written the code). Those
   repositories are read-only for this lab.

## 3. Entry template

```
### CLAIM-NNNN: <short title>
- id: CLAIM-NNNN
- statement: <precise statement; for Lean claims, the exact theorem statement and file>
- status: conjecture | computationally supported | proved on paper | Lean-verified | refuted
- novelty: unchecked | checked (<sources>) | known (<citation>)
- scope: formal (Millennium/Mathlib) | paper | experiment
- proposed: <date>, <session id>, <role>, <model id>
- evidence:
  - <item: path, commit, command, quoted output>
- attacks:
  - <date>, <session id>, <role>, <model id>: <what was tried> -> <outcome>
- kill criterion: <fixed in advance; what observation or object would refute the claim>
- dependencies: <other CLAIM ids this one needs>
- history:
  - <date>: created as <status>
  - <date>: promoted/demoted to <status> after attack <ref>
```

Ids are assigned in order of creation and never reused. CLAIM-0001 to CLAIM-0099 are reserved
for results inherited from the background repositories; CLAIM-0100 upward for this lab.

## 4. Entries

### CLAIM-0001: Cook-Levin against the LeanMillenniumPrizeProblems definitions
- id: CLAIM-0001
- statement: `PvsNP.cook_levin : NondeterministicPolynomialTimeComplete (fin_encoding_string Bool) SAT`
  in `D:\PvsNP\Pkg.lean`, where `SAT : Language (List Bool)` is the dense CNF encoding of
  `D:\PvsNP\CookLevin.lean` and `NondeterministicPolynomialTimeComplete` is the definition of
  `LeanMillenniumPrizeProblems` commit `603053dc267cf3efe422f438eb78098c0ececd6f`.
- status: Lean-verified
- novelty: known (Cook 1971, Levin 1973; prior formalizations listed in `D:\PvsNP\README.md`
  "Prior work": Gäher and Kunze, Coq, ITP 2021; Balbach, Isabelle AFP, 2023). What is specific
  to this formalization is the target definitions. Those citations were not re-read in S0:
  unverified by S0.
- scope: formal (Millennium/Mathlib)
- proposed: before 2026-10-05, sessions of `D:\PvsNP`, Claude Code agents directed by the author.
- evidence:
  - `D:\PvsNP\README.md` quotes `'PvsNP.cook_levin' depends on axioms: [propext, Classical.choice, Quot.sound]`.
  - Toolchain `leanprover/lean4:v4.31.0`, Mathlib `v4.31.0` (`fabf563a`), per `D:\PvsNP\README.md`.
  - S0 did not rebuild `D:\PvsNP`; the quoted output is taken from that README. Tag: build not re-run in S0.
- attacks:
  - `D:\PvsNP\REDTEAM.md` (commit `c311ceb`, "Red-team results and README caveats"), a separate
    session that had not written the code: hunted for vacuous hypotheses, signature drift, encoding
    tricks and statement bugs -> "found no critical or major issues; its four minor documentation
    findings are reflected in the caveats" (`D:\PvsNP\README.md`).
- kill criterion: a build of `D:\PvsNP` at commit `c271016` in which `#print axioms PvsNP.cook_levin`
  lists anything beyond the three standard axioms, or a Lean witness that `PvsNP.SAT` or
  `NondeterministicPolynomialTimeComplete` is degenerate (for example every language reduces to SAT
  for a trivial reason).
- dependencies: none
- history:
  - 2026-10-08: entered as Lean-verified on inherited evidence (rule 9).

### CLAIM-0002: Baker-Gill-Solovay, Theorems 1 and 3, in Lean
- id: CLAIM-0002
- statement: `Relativization.baker_gill_solovay : (∃ A : Oracle, PEqNP A) ∧ (∃ B : Oracle, ¬ PEqNP B)`
  in `D:\Relativization\Relativization\BakerGillSolovay.lean:30`, with `Oracle := Set (List Bool)`
  and `PEqNP A := ∀ L : Language (List Bool), InP A (fin_encoding_string Bool) L ↔ InNP A (fin_encoding_string Bool) L`,
  where `InP A` and `InNP A` are the Millennium `InPolynomialTime` and
  `InNondeterministicPolynomialTime` with an oracle `FinTM2` (binary query stack, one answer bit
  per step) in place of Mathlib's `TM2ComputableInPolyTime`.
- status: Lean-verified
- novelty: known (Baker, Gill, Solovay, SIAM J. Comput. 4(4):431-442, December 1975, Theorems 1
  and 3; citation data verified in S0 from Crossref, DOI 10.1137/0204037). The Lean statement
  omits the recursiveness of the oracles claimed in the paper's abstract (finding F1 of
  `D:\Relativization\REDTEAM.md`). `BARRIERS.md` section 1 states exactly what it does and does
  not show.
- scope: formal (Millennium/Mathlib)
- proposed: 2026-10-06 to 2026-10-08, sessions 1 to 9 of `D:\Relativization`.
- evidence:
  - `D:\Relativization\README.md` quotes `'Relativization.baker_gill_solovay' depends on axioms: [propext, Classical.choice, Quot.sound]`
    (`logs/session9-print-axioms.txt`) and a clean `lake build` (`logs/session9-build.txt`, 1342 jobs, 0 warnings).
  - `leanchecker --fresh` replay of the whole import closure exited 0 (`logs/redteam-leanchecker-fresh.txt`).
  - Empty-oracle faithfulness is machine-checked: `inP_empty_iff`, `inNP_empty_iff`,
    `classEquality_empty_iff : ClassEquality ∅ ↔ ClayPVersusNP` (`Relativization/Plain.lean:196-228`).
  - S0 did not rebuild `D:\Relativization`; outputs are taken from its README and REDTEAM. Tag: build not re-run in S0.
- attacks:
  - 2026-10-08, `D:\Relativization\REDTEAM.md` (session 8, a separate session that had not written
    the code): independent restatement on plain machines (`bgs_explicit`), degenerate readings ruled
    out with Lean proofs, source scan for banned tokens, term-walk of axioms, `leanchecker` per module
    and `--fresh`, literature check against a scanned copy of the 1975 paper -> no critical or major
    findings; minor findings F1 (no recursiveness), F2 (model equivalence with the paper's query
    machines is a paper argument, machine-checked only at the empty oracle), F3 (binary languages only).
- kill criterion: a build at commit `2d9d847` whose `#print axioms` lists extra axioms, or a Lean
  proof that `PEqNP` holds or fails for a degenerate reason (for example that `InP A` is empty or
  universal for the witness oracles).
- dependencies: none
- history:
  - 2026-10-08: entered as Lean-verified on inherited evidence (rule 9).

### CLAIM-0003: SDCL with PR learning is a polynomial-time SAT algorithm (candidate C3 of D:\PvsNP)
- id: CLAIM-0003
- statement: The pre-registered candidate C3 of `D:\PvsNP\PHASE3.md` (satisfaction-driven clause
  learning with propagation-redundant clause learning, configuration fixed in PHASE3 section 2.3.8)
  decides SAT in polynomial time; operationally, none of the kill criteria K1 to K5 of PHASE3 fires
  on the pre-registered families F1 to F8.
- status: refuted
- novelty: not applicable (a negative experimental result about one configuration).
- scope: experiment
- proposed: 2026-10-05, Phase 3 session of `D:\PvsNP`.
- evidence:
  - `D:\PvsNP\PHASE4.md` (report generated 2026-10-06, 536 of 1881 planned runs recorded):
    K1 (wrong answer or rejected proof) did not fire; K2 (home-turf exponential signature),
    K3 (scaling fits), K4 (no separation from resolution on F1 UNSAT at n in {200, 225, 250}) and
    K5 (more than 10 percent of instances at the time cap) all fired.
  - Example row, F1 (random 3-SAT at clause-to-variable ratio 4.26) at n = 250: SDCL 0 of 3 solved,
    3 timeouts at the 3600 s cap; plain CDCL 3 of 3 solved.
  - UNSAT answers checked by dpr-trim and an independent Python checker; SAT answers checked by an
    independent model evaluator (`PHASE4.md` section 1).
- attacks:
  - The refutation is itself the attack. No one has attacked the refutation; a Breaker could re-run
    `D:\PvsNP\phase4\src` on the manifest to confirm it.
- kill criterion (for the refutation): a re-run under the pre-registered configuration in which no
  kill criterion fires. Not expected.
- dependencies: none
- history:
  - 2026-10-08: entered as refuted on inherited evidence (rule 9).

### CLAIM-0004: SAT ∈ P (the open problem, kept visible)
- id: CLAIM-0004
- statement: `PvsNP.sat_in_p : InPolynomialTime (fin_encoding_string Bool) SAT` (`D:\PvsNP\SatInP.lean`, `sorry`).
- status: conjecture (open problem; the lab's working assumption is that it is false, and no
  session may attempt it directly; see `README.md`)
- novelty: not applicable
- scope: formal (Millennium/Mathlib)
- proposed: not by this lab.
- evidence: none. Listed so that no later entry can quietly depend on it.
- attacks: none recorded here. CLAIM-0003 is one failed algorithmic approach.
- kill criterion: a Lean proof of `¬ Millennium.ClayPVersusNP`, or of P ≠ NP in any accepted form.
- dependencies: none
- history:
  - 2026-10-08: created.

### CLAIM-0100: Exact circuit complexity of MOD3_n over B2 for n = 5, 6, 7 is certifiable (candidate C1)
- id: CLAIM-0100
- statement: For each n in {5, 6, 7} there is a value v_n such that (a) an explicit circuit of size v_n
  over the full binary basis B2 computes MOD3_n (x ↦ [Σ x_i ≡ 0 mod 3]), checked by evaluation in
  Lean, and (b) no circuit of size v_n − 1 does, as a Lean theorem obtained by replaying an LRAT
  certificate through Lean's verified LRAT checker (trusted base stated: Lean kernel plus compiler
  via `Lean.ofReduceBool`). The same statement is a template for MAJ_n, SUM_n mod 4 and the middle
  bit of multiplication; each family gets its own entry when its Theorist session starts.
- status: conjecture
- novelty: checked for the certified form only (LANDSCAPE.md 6.3: no SAT-certified circuit lower
  bound with a verified checker, and no machine-checked exact circuit complexity value, was found on
  2026-10-08). Whether v_5 is already in Knuth 7.1.2 or in Kojevnikov-Kulikov-Yaroslavtsev 2009 is
  unchecked; the Theorist must record it. The value itself is not claimed new for n = 5.
- scope: formal (a concrete Lean circuit model; not the Millennium definitions)
- proposed: 2026-10-08, S0, founding session (no role), `claude-fable-5-1`
- evidence: none yet
- attacks: none
- kill criterion: dead if (a) MOD3_6 at s = v_6 − 1 does not reach an UNSAT certificate within 48
  CPU-hours or the certificate exceeds 50 GB; or (b) the encoding soundness lemma is not proved
  within 10 Formalizer sessions; or (c) any certified value contradicts Knuth's 5-input table (then
  all results are withdrawn until the encoding bug is found).
- dependencies: none
- history:
  - 2026-10-08: created as conjecture.

### CLAIM-0101: Exact minimum refutation sizes of small PHP and Tseitin instances in resolution and tree-like Res(⊕) are certifiable (candidate C2)
- id: CLAIM-0101
- statement: For PHP_4^3, PHP_5^4, and Tseitin contradictions on 3-regular graphs with 4 to 8
  vertices, the exact minimum refutation size in dag-like resolution, regular resolution and
  tree-like Res(⊕) is a specific number, with "no refutation of size < s" certified by an UNSAT
  certificate of a meta-encoding checked by a verified checker.
- status: conjecture
- novelty: checked (LANDSCAPE.md Section 2: Peitl-Szeider 2021 and Sidorov et al. 2024/2025 compute
  shortest resolution proofs for small formulas; no exact values for these instances and nothing for
  Res(⊕) were found). Unchecked whether Sidorov et al.'s tables already contain PHP_4^3.
- scope: experiment, then formal
- proposed: 2026-10-08, S0, `claude-fable-5-1`
- evidence: none yet
- attacks: none
- kill criterion: dead if the resolution meta-encoding does not resolve PHP_5^4 within 72 CPU-hours,
  or the Res(⊕) meta-encoding does not resolve any 6-vertex Tseitin instance within 72 CPU-hours, or
  any certified value disagrees with brute force on a brute-forceable instance.
- dependencies: CLAIM-0100's pipeline (optional)
- history:
  - 2026-10-08: created as conjecture.

### CLAIM-0102: The Lean statement of arXiv 2609.23015 (Braun) faithfully states the claimed dag-like Res(⊕) lower bound (candidate C3, part i)
- id: CLAIM-0102
- statement: In `github.com/kbr-/math-research` at commit `8904bf09`, the declaration
  `MathResearch.bitPHP_exponential` (file `formalization/claims/BitPHPExponential.lean`) states, under
  a definition of Res(⊕) equivalent to the standard one (disjunctions of affine equations over F2,
  dag-like, with the usual inference rules), that every refutation of the bit pigeonhole principle
  with n+1 pigeons and 2^ℓ holes, ℓ ≥ 32, has more than exp(n/(32768 ℓ^2)) clauses; the project builds
  with the pinned toolchain (Lean 4.34.0-rc2 per the paper), `#print axioms` lists only the three
  standard axioms, and `leanchecker --fresh` passes.
- status: conjecture (an external claim entered for audit; promotion would mean "audit found no
  gap", not "the lab proved it")
- novelty: not applicable for the audit; a found gap would be a new negative finding.
- scope: formal (external repository, read-only copy)
- proposed: 2026-10-08, S0, `claude-fable-5-1`, from the abstract and PDF text read in S0
- evidence:
  - Paper Appendix B gives the declaration map and reproduction commands (read in S0 from the PDF
    text); the expected axiom report quoted there is `[propext, Classical.choice, Quot.sound]`.
  - No audit by anyone else is known to this lab (LANDSCAPE.md 6.1).
- attacks: none
- kill criterion (for the faithfulness claim): a degenerate reading of the formal Res(⊕) definition
  (for example a rule set weaker than standard Res(⊕), a size measure that is not clause count, a
  hidden hypothesis on refutations), a nonstandard axiom, a failed `leanchecker --fresh`, or a
  mismatch between `bitPHP_exponential`'s parameters and the paper's Theorem 1.1.
- dependencies: none
- history:
  - 2026-10-08: created as conjecture.

### CLAIM-0103: Exact monotone circuit size of triangle detection on 5 and 6 vertices is certifiable (candidate C4)
- id: CLAIM-0103
- statement: For graphs on v ∈ {5, 6} vertices, the minimum number of AND/OR gates in a monotone
  circuit deciding "contains a triangle" is a specific number m_v, with the lower bound certified as
  in CLAIM-0100.
- status: conjecture
- novelty: unchecked (S0 found no SAT-computed exact monotone sizes; exact-synthesis catalogues of
  Haaswijk et al. and Knuth 7.1.2 must be checked for these functions).
- scope: experiment, then formal
- proposed: 2026-10-08, S0, `claude-fable-5-1`
- evidence: none yet
- attacks: none
- kill criterion: dead if v = 6 (15 inputs) does not certify within 72 CPU-hours, or the value is
  already published.
- dependencies: CLAIM-0100's pipeline
- history:
  - 2026-10-08: created as conjecture.

### CLAIM-0104: The collapse and separation oracles of D:\Relativization admit decidable variants (candidate C5, part i)
- id: CLAIM-0104
- statement: There are oracles A', B' : Set (List Bool) with `PEqNP A'`, `¬ PEqNP B'`,
  `ComputablePred (· ∈ A')` and `ComputablePred (· ∈ B')`, proved in Lean against the
  `D:\Relativization` definitions (copied into this lab's `lean/`).
- status: conjecture
- novelty: known on paper (BGS 1975 abstract and Theorem 1 remarks: recursive oracles; read in S0);
  new as a formalization (LANDSCAPE.md 6.1: no other formalization of BGS found).
- scope: formal (Millennium/Mathlib)
- proposed: 2026-10-08, S0, `claude-fable-5-1`
- evidence: `D:\Relativization\REDTEAM.md` finding F1 identifies the gap.
- attacks: none
- kill criterion: dead if decidability of the collapse oracle is not proved within 8 Formalizer
  sessions.
- dependencies: CLAIM-0002
- history:
  - 2026-10-08: created as conjecture.

### CLAIM-0105: Ben-Sasson-Wigderson size-width and a PHP width lower bound in Lean (candidate C6)
- id: CLAIM-0105
- statement: In a Lean 4 development against Mathlib: for every k-CNF F over n variables with a
  resolution refutation of size S, F has a refutation of width at most k + O(√(n log S)) (the
  Ben-Sasson-Wigderson relation, 2001), and every resolution refutation of PHP_{n+1}^n has width at
  least n/3 (or the bound the Theorist states from a source read in that session), hence size
  exp(Ω(n)).
- status: conjecture
- novelty: known on paper (LANDSCAPE.md Section 2, Haken 1985; Ben-Sasson, Wigderson 2001); new as
  a formalization (no machine-checked Haken-type bound found, LANDSCAPE.md 6.1). The exact width
  constant must be taken from a source read by the Theorist, not from this entry.
- scope: formal (paper-level objects, not Millennium)
- proposed: 2026-10-08, S0, `claude-fable-5-1`
- evidence: none yet
- attacks: none
- kill criterion: dead if the size-width relation is not proved within 12 Formalizer sessions.
- dependencies: none (Braun's repository, CLAIM-0102, may supply reusable definitions)
- history:
  - 2026-10-08: created as conjecture.
