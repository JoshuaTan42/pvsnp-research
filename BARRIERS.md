# BARRIERS.md: relativization, natural proofs, algebrization

Written in session S0 (2026-10-08). Purpose: state each barrier precisely enough that a
candidate approach can be checked against it mechanically, and record exactly what the lab's
own Lean theorem `baker_gill_solovay` does and does not establish.

## 0. Provenance and verification key

Every citation below carries one of these tags.

* **[read S0]**: the text was read in session S0 from the stated copy. Quotations are verbatim
  from that copy. Where the text extraction lost mathematical symbols (the 1975 scan renders
  the calligraphic P and NP as garbage), the symbols are restored in the quotation and the
  restoration is marked with square brackets.
* **[metadata S0]**: only the bibliographic record (title, authors, venue, volume, pages, date,
  DOI) was verified in S0, through the Crossref API. The content of the paper was not read and
  any statement attributed to it is marked unverified.
* **[unverified]**: neither read nor checked in S0. Do not cite from this file as if verified.

Copies read in S0:

| Paper | Copy read | What was read |
|---|---|---|
| Baker, Gill, Solovay 1975 | text-layer PDF of the SIAM printing, from `cse.ucdenver.edu/~cscialtman/complexity/` (header "SIAM J. COMPUT. Vol. 4, No. 4, December 1975") | all of pp. 431-442 |
| Razborov, Rudich 1994 | ECCC TR94-010 PDF (`eccc.weizmann.ac.il/report/1994/010/`), bitmap fonts, read as page images | pp. 1-13 (abstract, Sections 1 to 3, Theorem 4.1 and its proof) |
| Aaronson, Wigderson 2008/2009 | the authors' PDF (`scottaaronson.com/papers/alg.pdf`) | abstract, Sections 1, 2, 5 (theorem statements), 9, 10, 11; ECCC TR08-005 abstract page |
| Chow, Almost-natural proofs | arXiv 0805.1385v3 | Sections 1 and 2 (secondary source for the Razborov-Rudich definitions) |

Crossref records fetched in S0 (DOI, as returned by `api.crossref.org`):

* Baker, Gill, Solovay, "Relativizations of the P =? NP Question", SIAM Journal on Computing 4(4),
  pp. 431-442, December 1975, DOI 10.1137/0204037.
* Razborov, Rudich, "Natural Proofs", Journal of Computer and System Sciences 55(1), pp. 24-35,
  August 1997, DOI 10.1006/jcss.1997.1494. (The STOC 1994 record was rate-limited; the
  conference pages 204-213 are **[unverified]**.)
* Aaronson, Wigderson, "Algebrization: A New Barrier in Complexity Theory", Proceedings of the
  40th ACM STOC, pp. 731-740, May 2008, DOI 10.1145/1374376.1374481; and ACM Transactions on
  Computation Theory 1(1), pp. 1-54, February 2009, DOI 10.1145/1490270.1490272.
* Impagliazzo, Kabanets, Kolokolova, "An axiomatic approach to algebrization", Proceedings of the
  41st ACM STOC, pp. 695-704, 2009, DOI 10.1145/1536414.1536509. **[metadata S0]**
* Aydinlioğlu, Bach, "Affine Relativization: Unifying the Algebrization and Relativization
  Barriers", ACM Transactions on Computation Theory 10(1), pp. 1-67, 2018, DOI 10.1145/3170704.
  **[metadata S0]**

A general remark that applies to all three barriers. None of them is a theorem of the form
"no proof of P ≠ NP exists with property X" for a formally defined X. Each is a theorem about
oracles, properties or extension oracles, together with an informal observation that a class of
proof techniques would, if it worked, contradict that theorem. The informal part is where
judgement enters, and the checklists below exist to make that judgement explicit and auditable.

---

## 1. Relativization (Baker, Gill, Solovay 1975)

### 1.1 Source **[read S0]**

Theodore Baker, John Gill and Robert Solovay, "Relativizations of the P =? NP Question",
SIAM Journal on Computing, Vol. 4, No. 4, December 1975, pp. 431-442. "Received by the editors
July 16, 1973, and in final revised form October 21, 1974." The paper credits independent
discoveries of parts of its results to M. J. Fischer, R. Ladner, A. R. Meyer and H. B. Hunt III
(p. 433, p. 434, p. 436).

### 1.2 The machine model and the theorems, as printed

Model (p. 432): "A query machine, described by Cook [1], is a multitape Turing machine with a
distinguished worktape, called the query tape, and three distinguished states, called the query
state, the yes state, and the no state." Time bound: "A query machine is polynomial-bounded if
there is a polynomial p(n) such that every computation of the machine on every input of length n
halts within p(n) steps, whatever oracle X is used." P^X and NP^X are the classes of languages
recognized (accepted) by polynomial-bounded deterministic (nondeterministic) query machines with
oracle X. Oracles are sets of binary strings.

Abstract (p. 431): "We construct a recursive set A such that [P^A = NP^A]. On the other hand, we
construct a recursive set B such that [P^B ≠ NP^B]. Oracles X are constructed to realize all
consistent set inclusion relations between the relativized classes [P^X, NP^X], and co-[NP^X], the
family of complements of languages in [NP^X]."

* **Theorem 1** (p. 434): "There is an oracle A such that [P^A = NP^A]." Proof: A = K(A) where
  K(X) = {⟨i, x, 0^n⟩ : some computation of NP_i^X accepts x in fewer than n steps}, well defined
  because "In a computation of length < n, no string of length ≥ n can be queried." Remarks (p. 434):
  "Kleene's recursion theorem can also be used to produce a recursive oracle A such that A = K(A).
  The oracle A constructed in Theorem 1 can be recognized deterministically in exponential time."
* **Theorem 2** (p. 434): "If A is polynomial-space complete, then [P^A = NP^A]."
* **Theorem 3** (p. 436): "There is an oracle B such that [P^B ≠ NP^B]." Proof by stages against
  L(X) = {x : there is y ∈ X such that |y| = |x|}: at stage i choose n with p_i(n) < 2^n, run P_i
  with the current finite oracle on 0^n, and if it rejects add the least unqueried string of
  length n. "The set B constructed in the proof of Theorem 3 is sparse". Also (p. 436): "Richard
  Ladner has shown that there are oracles B recognizable deterministically in exponential time such
  that [P^B ≠ NP^B]."
* Theorems 4 to 7 (pp. 437-440) realize the remaining consistent relations among P^X, NP^X and
  co-NP^X (for example an oracle C with NP^C not closed under complement; an oracle E with
  P^E ≠ NP^E and P^E = NP^E ∩ co-NP^E).
* Section 4 (pp. 440-441) poses open problems about the relativized polynomial hierarchy, including
  (iv) "Does there exist an oracle X such that [Σ_2^{P,X} ≠ Π_2^{P,X}]?" with "We were unable to
  settle (iv) by the methods of this paper."

### 1.3 The barrier, as the authors state it

The paper does not define "relativizing proof". The barrier is the following inference, stated on
pp. 431-432 **[read S0]**:

"It seems unlikely that ordinary diagonalization methods are adequate for producing an example of
a language in [NP] but not in [P]; such diagonalizations, we would expect, would apply equally
well to the relativized classes, implying a negative answer to all relativized [P =? NP]
questions, a contradiction. On the other hand, we do not feel that one can give a general method
for simulating nondeterministic machines by deterministic machines in polynomial time, since such
a method should apply as well to relativized machines and therefore imply affirmative answers to
all relativized [P =? NP] questions, also a contradiction. Our results suggest that the study of
natural, specific decision problems offers a greater chance of success in showing [P ≠ NP] than
constructions of a more general nature."

Precise form used by this lab: **a proof of P ≠ NP (or of P = NP) that remains valid when every
machine in the argument is given the same oracle, for every oracle, cannot exist**, because
Theorem 1 gives an oracle under which the conclusion P^A ≠ NP^A is false and Theorem 3 gives an
oracle under which P^B = NP^B is false. The two directions are symmetric.

### 1.4 What it rules out, and what it does not

Ruled out (per the sources read):

* Diagonalization and simulation arguments that treat machines as black boxes. Razborov and
  Rudich describe the historical effect: "Since relativizing proof techniques involving
  diagonalization and simulation were the only available tools at the time of their work progress
  along known lines was ruled out." (RR94 p. 1, **[read S0]**).
* Any argument that uses a Boolean formula or circuit only through evaluation queries. Aaronson
  and Wigderson (p. 1, **[read S0]**): the techniques "would work equally well in a 'relativized
  world,' where both P and NP machines could compute some function f in a single time step."

Not ruled out, with the caveats the sources themselves give:

* Arithmetization. IP = PSPACE and MIP = NEXP are non-relativizing (AW09 p. 2: "researchers
  managed to prove IP = PSPACE [27, 37] and other celebrated theorems about interactive protocols,
  even in the teeth of relativized worlds where these theorems were false"). Section 3 covers the
  barrier that applies to them instead.
* Small-depth circuit lower bounds, which "can be shown to fail relative to suitable oracle gates"
  (AW09 Section 9, **[read S0]**), and are therefore non-relativizing in that sense but are subject
  to the natural proofs barrier (Section 2).
* Local checkability. AW09 Section 9 reports that "Arora, Impagliazzo, and Vazirani [3] argue that
  even the Cook-Levin Theorem (and by extension, the PCP Theorem) should be considered
  non-relativizing", and that Hartmanis et al. cite Hopcroft-Paul-Valiant 1977 and Paul et al.
  1983 as non-relativizing results predating interactive proofs. AW09 add that "there is legitimate
  debate about whether the results listed in (2) and (3) should 'truly' be considered
  non-relativizing". The Arora-Impagliazzo-Vazirani manuscript itself is **[unverified]** in S0.
* Model dependence. BGS pp. 436-437: "The proof of Theorem 3 makes use of the fact that we can
  query an oracle about any number of length n in approximately n steps." Under "oracle tape"
  models (the oracle's elements, or its characteristic function, written on a tape) "our proofs of
  Theorems 1-3 are no longer valid. Paul Morris [7] has pointed out that with the oracle-tape
  models for query machines, if [P = NP], then [P^X = NP^X] for every oracle X." So the barrier
  depends on the convention that building a long query costs time. This is the convention of the
  Lean formalization (Section 1.5).
* Measure. BGS p. 437: "Kurt Mehlhorn [5] has shown that the family of oracles X for which
  [P^X = NP^X] is a 'meagre' set in a space of recursive oracles." Nothing follows about the
  unrelativized question.

Later formal treatments of "relativizing proof" exist (Impagliazzo, Kabanets, Kolokolova 2009;
Aydinlioğlu, Bach 2018) **[metadata S0]**; their definitions were not read in S0 and are not
relied on here.

### 1.5 The Lean theorem `baker_gill_solovay` (D:\Relativization): exactly what it shows

Statement (`Relativization/BakerGillSolovay.lean:30`, commit `2d9d847`), read in S0:

```lean
theorem baker_gill_solovay : (∃ A : Oracle, PEqNP A) ∧ (∃ B : Oracle, ¬ PEqNP B) :=
  ⟨collapse, separation⟩
```

with `Oracle := Set (List Bool)` and
`PEqNP A := ∀ L : Language (List Bool), InP A (fin_encoding_string Bool) L ↔ InNP A (fin_encoding_string Bool) L`.

**It shows** (machine-checked; `#print axioms` lists `propext`, `Classical.choice`, `Quot.sound`
only; `leanchecker --fresh` replay exit 0; per `D:\Relativization\README.md` and `REDTEAM.md`,
read in S0, not rebuilt in S0):

1. There is a set A of binary strings such that, for every language L of binary strings,
   L ∈ P^A iff L ∈ NP^A, where P^A and NP^A are defined on Mathlib `FinTM2` multi-stack machines
   extended with a binary query stack whose membership in A is visible to the program as one bit at
   every step (`OracleFinTM2`, `Oracle.lean`), with `InP A` and `InNP A` being the
   LeanMillenniumPrizeProblems definitions `InPolynomialTime` and
   `InNondeterministicPolynomialTime` with the oracle machine substituted clause for clause
   (`Classes.lean`). NP^A is verifier-defined with certificates `|y| ≤ |w|^k`.
2. There is a set B of binary strings and a language `sepLang B` with `sepLang B ∈ NP^B` and
   `sepLang B ∉ P^B` (`Stage.lean:269`, `sepLang_not_inP`). The witnesses are variants of the
   paper's Theorem 1 and Theorem 3 constructions, not copies (`REDTEAM.md` F7).
3. At the empty oracle the relativized classes are exactly the Clay-statement classes of the
   LeanMillenniumPrizeProblems repository: `inP_empty_iff`, `inNP_empty_iff`,
   `classEquality_empty_iff : ClassEquality ∅ ↔ ClayPVersusNP` (`Plain.lean:196-228`).
4. `P^A ⊆ NP^A` for every oracle A (`inP_subset_inNP`, `Comp.lean:535`), and `A ∈ P^A`
   (`Self.lean`).
5. The query cost regime is the paper's query-tape regime: a t-step run asks at most t queries,
   each of length at most `|input| + t·depth` (`card_queries_le`, `query_length_iter`).

**It does not show:**

1. That A or B is recursive or decidable. The abstract and Sections 2 and 3 of the paper claim
   recursive oracles; the Lean witnesses are `noncomputable` (built from `Classical.choose`
   enumerations) and no decidability statement is proved (`REDTEAM.md` F1). The Lean statement
   matches Theorems 1 and 3 as printed, which do not mention recursiveness.
2. That the Lean oracle-machine model defines the same classes as the paper's query machines
   (query state and yes/no states, nondeterministic machines for NP^X, time bound required under
   every oracle). That equivalence is a standard paper argument and is machine-checked only at the
   empty oracle (`REDTEAM.md` F2).
3. The collapse over every finite alphabet (`ClassEquality univOracle`): only binary languages
   (`PEqNP`) are covered for the collapse. The separation does give `¬ ClassEquality sepOracle`
   (`REDTEAM.md` F3, proved in a scratch file, not in the repository).
4. Theorem 2 (PSPACE-complete oracle), Theorems 4 to 7, or Lemma 2 of the paper.
5. **Anything about proofs.** The theorem has no notion of "relativizing proof". It cannot certify
   that a given argument relativizes or does not. It is a theorem about two oracles.
6. Anything about P vs NP itself.

**How the lab uses it.** The theorem gives a mechanical sanity check on the *shape* of a Lean
development, not a verdict on an informal argument. If a proposed Lean proof of `¬ PEqNP ∅`
(P ≠ NP for binary languages) can be stated and proved with a free parameter `A : Oracle` in place
of `∅`, so that it yields `∀ A, ¬ PEqNP A`, then it contradicts `collapse` and is wrong. Likewise a
proof of `PEqNP ∅` that generalizes to `∀ A, PEqNP A` contradicts `separation`. The practical test:
abstract every lemma of the development over `A`, and find the first lemma that fails to
generalize. That lemma is the non-relativizing step, and it must be named in the ledger entry.
The test is sound in one direction only: a development that fails to generalize might still be
wrong for other reasons, and an informal argument can relativize without anyone having tried to
generalize it.

### 1.6 Relativization checklist (every candidate must answer all five in writing)

* R1. Which step of the argument uses the explicit description (program text, clause structure,
  gate structure) of the machine or formula, rather than input-output behaviour?
* R2. Rewrite the argument for machines with an oracle gate for an arbitrary set X. Which step
  fails, and for which X does it fail? If no step fails, the argument relativizes and is wrong.
* R3. If a Lean development exists, run the parametrization test of Section 1.5 and record the
  first lemma that does not generalize over `A : Oracle`.
* R4. State the machine convention used (query-tape cost regime versus oracle-tape model) and
  confirm the argument does not depend on a convention under which the barrier would not apply
  (BGS pp. 436-437).
* R5. If the non-relativizing ingredient is local checkability (Cook-Levin, PCP) or
  arithmetization, say so, and proceed to the checklists of Sections 2 and 3, which apply to
  exactly those ingredients.

---

## 2. Natural proofs (Razborov, Rudich 1994/1997)

### 2.1 Source **[read S0]**

Alexander A. Razborov and Steven Rudich, "Natural Proofs", ECCC TR94-010, received December 13,
1994 (read as the ECCC PDF). Journal version: Journal of Computer and System Sciences 55(1),
pp. 24-35, 1997 **[metadata S0]**; the journal text was not read, so page references below are to
the ECCC version.

### 2.2 The definitions, as printed (ECCC pp. 2-4)

F_n is the set of all Boolean functions of n variables; f_n ∈ F_n is identified with its truth
table, a binary string of length 2^n. "Formally, by a combinatorial property of Boolean functions
we will mean a set of Boolean functions {C_n ⊆ F_n | n ∈ ω}."

"The combinatorial property C_n is natural if it contains a subset C*_n with the following two
conditions:

* Constructivity: The predicate f_n ∈? C*_n is in P. Thus, C*_n is computable in time which is
  polynomial in the truth table of f_n,
* Largeness: |C*_n| ≥ 2^{-O(n)} · |F_n|.

A combinatorial property C_n is useful against P/poly if it satisfies:

* Usefulness: The circuit size of any sequence of functions f_1, f_2, ..., f_n, ..., where
  f_n ∈ C_n, is superpolynomial, i.e., for any constant k, for sufficiently large n, the circuit
  size of f_n is greater than n^k.

A proof that some function does not have polynomial-sized circuits is natural against P/poly if
the proof contains, more or less explicitly, the definition of a natural combinatorial property
C_n which is useful against P/poly."

Generalization (p. 4): for complexity classes Γ and Λ, "Call a combinatorial property C_n
Γ-natural if it contains C*_n ⊆ C_n with the following two conditions: Constructivity: The
predicate f_n ∈? C*_n is computable in Γ (recall, C*_n is a set of truth-tables with 2^n bits),
Largeness: |C*_n| ≥ 2^{-O(n)} · |F_n|." and "A combinatorial property C_n is useful against Λ if
it satisfies: Usefulness: For any sequence of functions f_n, where the event f_n ∈ C_n happens
infinitely often, {f_n} ∉ Λ." A lower bound proof is Γ-natural against Λ if it states a Γ-natural
property useful against Λ; "P-natural proofs will simply be called natural" (p. 5).

Two remarks from the paper that matter for applying it:

* p. 3: "Note that the notion of a natural proof, unlike that of a natural combinatorial property,
  is not quite precise." General statements "should be understood as equivalent to 'there exists
  a natural combinatorial property C_n ...'".
* p. 3: "In monotone models, the lower bounds use constructive combinatorial properties, but there
  is apparently no formal analogue of the largeness condition." (Footnote 3: "In particular, a
  useful definition of random monotone function is not evident.")

### 2.3 The main theorem, as printed (ECCC p. 12)

Hardness of a generator G_n : {0,1}^n → {0,1}^{2n} is "the minimal S for which there exists a
circuit C of size ≤ S such that |P[C(G_n(x)) = 1] − P[C(y) = 1]| ≥ 1/S".

"**Theorem 4.1.** Assume that there exists a lower bound proof which is P/poly-natural against
P/poly. Then for every polynomial time computable G_k : {0,1}^k → {0,1}^{2k}, H(G_k) ≤ 2^{k^{o(1)}}.
Equivalently, if 2^{n^ε}-hard functions exist then there is no P/poly-natural proof (against
P/poly)."

Footnote 6: "Razborov [23] has observed that a straightforward extension gives the same result
for proofs using properties possessing largeness and computable by non-uniform circuits of size
2^{n^{O(1)}} (that is, quasipolynomial in 2^n)". On the assumption (p. 13): "The assumption that
2^{n^ε}-hard functions exist is quite plausible. For example, despite many advances in
computational number theory, multiplication seems to provide a basis for a family of such
functions (known factoring algorithms are sufficiently exponential)."

From the abstract (p. 1): "Without the hardness assumption, we are able to show that they can't
prove exponential lower bounds (for general circuits) applicable to the discrete logarithm
problem. We show that the weaker class of AC^0-natural proofs which is sufficient to prove the
parity lower bounds of Furst, Saxe, and Sipser; Yao; and Hastad is inherently incapable of proving
the bounds of Razborov and Smolensky. We give some formal evidence that natural proofs are indeed
natural by showing that every formal complexity measure which can prove super-polynomial lower
bounds for a single function, can do so for almost all functions". The proofs of these three
statements (Sections 4 and 5 of the paper) were not read in S0.

Precise form used by this lab: **if pseudorandom generators of hardness 2^{n^ε} computable in
polynomial time exist (a consequence of, for example, factoring being 2^{n^ε}-hard), then no
proof that an explicit function is outside P/poly can proceed by exhibiting a property of truth
tables that is both decidable in time polynomial in 2^n and satisfied by a 2^{-O(n)} fraction of
all functions.** The barrier is conditional, and it is about non-uniform lower bounds (P/poly and
its subclasses), not about algorithms for SAT.

### 2.4 Which techniques are natural, per the paper (Section 3, **[read S0]**)

The paper's claim (p. 1): "all lower bound proofs for non-monotone models known to us in
non-uniform Boolean complexity either are natural or can be represented as natural." The examples
it works through, with the class Γ it assigns:

| Section | Technique | Γ-natural for Γ = |
|---|---|---|
| 3.1 | AC^0 lower bounds for parity via random restrictions (Furst-Saxe-Sipser, Yao, Håstad) | AC^0 |
| 3.2 | AC^0[q] lower bounds via low-degree polynomial approximation (Razborov, Smolensky, Barrington-Beigel-Rudich); Smolensky's proof naturalized in 3.2.1 by a rank property | NC^2 |
| 3.3 | Perceptron (constant depth with one majority gate) lower bounds for parity via weak degree | P (uses linear programming) |
| 3.4 | Formula size lower bounds via shrinkage under random restrictions (Andreev, Håstad's near-n^3 bound) | AC^0 |
| 3.5 | Depth-2 threshold circuit lower bounds via discrepancy (Hajnal et al.) | TC^0 |
| 3.6 | Switching-and-rectifier network lower bounds via minimum cover | AC^0 |

Consequences the paper draws: there is no AC^0-natural proof against AC^0[2] (p. 6: "We will show
in Section 4 that there is no AC^0-natural proof against AC^0[2]"), which "gives the insight that
[33, 24, 5] had to require arguments from a stronger class than those of [7, 27, 10]".

Not covered by the paper's analysis, by its own account:

* Monotone lower bounds (p. 3, quoted above): no formal largeness analogue, so the theorem does
  not apply as stated. Whether known monotone proofs are "natural" in a modified sense is not
  settled by the paper.
* Counting and diagonalization arguments (p. 4): "The best example of a (supposedly) unnatural
  argument is a traditional counting argument", and "counting arguments (closely associated with
  diagonalization arguments) have yet not proved any lower bounds for explicit functions". AW09
  (p. 2) add that "Any complexity class separation proved via diagonalization ... is inherently
  non-naturalizing."
* Uniform lower bounds (time hierarchy, space hierarchy) and anything that does not go through a
  property of truth tables.
* Proofs for restricted classes Λ against which no pseudorandom generator is believed to exist in
  Λ: the theorem needs the generator to be computable within the class being lower-bounded (the
  construction uses "a pseudo-random function generator ... computable by polynomial size
  circuits", p. 13). For Λ = AC^0 there is no such barrier (the parity lower bound exists). For
  Λ = TC^0 the usual claim is that Naor-Reingold generators in TC^0 make the barrier apply;
  **[unverified]** in S0, see `LANDSCAPE.md`.
* Williams' algorithmic method for ACC^0 lower bounds (2011) post-dates the paper; the claim that
  it avoids naturalization is **[unverified]** in this file; see `LANDSCAPE.md` for what was
  verified about it.

Secondary source (Chow, arXiv 0805.1385v3, read in S0) summarises the scope as "all
non-relativizing, non-monotone, superlinear lower bounds" known in 1994 and proves that weakening
largeness to density 2^{-q(n)} with q quasi-polynomial allows "almost-natural" useful properties
to exist under the same assumption.

### 2.5 Naturalization checklist (for any approach that proves a circuit or formula lower bound)

* N1. Write down the property C_n that the argument establishes for the hard function. If the
  argument proves "f is hard because f has property C", C_n is explicit; if it proves "f is hard
  because f simulates all machines of some class", say so (that is a diagonalization, not a
  property of truth tables).
* N2. Largeness test: does the argument, as written or with the paper's "naturalizing" adjustments
  (RR94 Section 3.2.1), prove the same lower bound for a 2^{-O(n)} fraction of all functions, for
  example for a random function? Record the fraction.
* N3. Constructivity test: can membership in C_n be decided in time polynomial in 2^n (or by
  circuits of size 2^{n^{O(1)}}, footnote 6)? Record the algorithm or the obstacle.
* N4. If N2 and N3 both hold and the target class Λ is believed to contain 2^{n^ε}-hard
  pseudorandom function generators, the approach is natural against Λ and cannot give
  superpolynomial lower bounds against Λ unless those generators do not exist. Stop, or redirect
  the approach at a class without such generators (AC^0, monotone) where it is not a barrier.
* N5. If the approach escapes by non-largeness, name the specific structure of the hard function
  used, and check the argument does not secretly transfer to random functions. If it escapes by
  non-constructivity, name the step that is not computable in time 2^{n^{O(1)}} and confirm the
  property is still useful.
* N6. State whether the result is unconditional or inherits the barrier's hardness assumption in
  some other form.

---

## 3. Algebrization (Aaronson, Wigderson 2008/2009)

### 3.1 Source **[read S0]**

Scott Aaronson and Avi Wigderson, "Algebrization: A New Barrier in Complexity Theory". Read from
the authors' PDF (54 pages; the TOCT version has 54 pages per Crossref, and the section and
theorem numbering below is that of the copy read). ECCC TR08-005 (submitted 15 January 2008,
published 8 February 2008); STOC 2008 pp. 731-740; ACM TOCT 1(1):1-54, 2009.

### 3.2 The definitions, as printed (Section 2)

"**Definition 2.1 (Oracle)** An oracle A is a collection of Boolean functions A_m : {0,1}^m →
{0,1}, one for each m ∈ N. Then given a complexity class C, by C^A we mean the class of languages
decidable by a C machine that can query A_m for any m of its choice. By C^{A[poly]} we mean the
class of languages decidable by a C machine that, on inputs of length n, can query A_m for any
m = O(poly(n)). For classes C such that all computation paths are polynomially bounded (for
example, P, NP, BPP, #P...), it is obvious that C^{A[poly]} = C^A."

"**Definition 2.2 (Extension Oracle Over A Finite Field)** Let A_m : {0,1}^m → {0,1} be a Boolean
function, and let F be a finite field. Then an extension of A_m over F is a polynomial
Ã_{m,F} : F^m → F such that Ã_{m,F}(x) = A_m(x) whenever x ∈ {0,1}^m. Also, given an oracle
A = (A_m), an extension Ã of A is a collection of polynomials Ã_{m,F} : F^m → F, one for each
positive integer m and finite field F, such that (i) Ã_{m,F} is an extension of A_m for all m, F,
and (ii) there exists a constant c such that mdeg(Ã_{m,F}) ≤ c for all m, F." (Footnote 6:
"nowhere in this paper will mdeg(Ã_{m,F}) need to be greater than 2.") C^Ã is the class decidable
by a C machine that can query Ã_{m,F} for any m and finite field F.

"**Definition 2.3 (Algebrization)** We say the complexity class inclusion C ⊆ D algebrizes if
C^A ⊆ D^Ã for all oracles A and all finite field extensions Ã of A. Likewise, we say that C ⊆ D
does not algebrize, or that proving C ⊆ D would require non-algebrizing techniques, if there exist
A, Ã such that C^A ⊄ D^Ã. We say the separation C ⊄ D algebrizes if C^Ã ⊄ D^A for all A, Ã.
Likewise, we say that C ⊄ D does not algebrize, or that proving C ⊄ D would require
non-algebrizing techniques, if there exist A, Ã such that C^Ã ⊆ D^A."

The authors explain the asymmetry (only one side gets the extension, and which side depends on
whether an inclusion or a separation is being proved): "under a more stringent notion of
algebrization, we would not know how to prove that existing interactive proof results algebrize."
Section 6 treats extensions over the integers; the finite-field version is the one used for the
statements below.

### 3.3 The theorems, as printed

Results that algebrize (Section 3; the tildes are restored from the proofs since the text
extraction dropped them):

* Theorem 3.6: for all A, Ã, P^{#P^A} ⊆ IP^Ã. Theorem 3.7: PSPACE^{A[poly]} ⊆ IP^Ã.
  Theorem 3.8: NEXP^{A[poly]} ⊆ MIP^Ã.
* Theorem 3.14 and Corollary 3.15: EXP^{A[poly]} ⊆ MIP_EXP^Ã, and if EXP^{A[poly]} ⊆ P^A/poly then
  EXP^{A[poly]} ⊆ MA^Ã.
* Theorem 3.16: for all A, Ã and constants k, PP^Ã ⊄ SIZE^A(n^k). Theorem 3.18: PromiseMA^Ã ⊄
  SIZE^A(n^k). Section 1.2 also lists MA_EXP^Ã ⊄ P^A/poly.

Results showing that non-algebrizing techniques are needed (Section 5):

* "Theorem 5.1 There exist A, Ã such that NP^Ã ⊆ P^A." Proof: "Let A be any PSPACE-complete
  language, and let Ã be the unique multilinear extension of A. As observed by Babai, Fortnow, and
  Lund [4], the multilinear extension of any PSPACE language is also in PSPACE. So as in the usual
  argument of Baker, Gill, and Solovay [5], we have NP^Ã = NP^PSPACE = PSPACE = P^A." Hence
  **any proof of P ≠ NP requires non-algebrizing techniques.**
* "Theorem 5.2 There exist A, Ã such that PSPACE^{Ã[poly]} = P^A."
* "Theorem 5.3 There exist A, Ã such that NP^A ⊄ P^Ã. Furthermore, the language L that achieves
  the separation simply corresponds to deciding, on inputs of length n, whether there exists a
  w ∈ {0,1}^n with A_n(w) = 1." Hence **any proof of P = NP requires non-algebrizing techniques.**
  The proof "closely follows the usual diagonalization argument of Baker, Gill, and Solovay [5],
  except that we have to use Lemma 4.5 to handle the fact that P can query a low-degree
  extension."
* Theorem 5.4: NP^A ⊄ BPP^Ã; Theorem 5.5: NP^A ⊄ P^Ã/poly; Theorem 5.6: NTIME^A(2^n) ⊄ SIZE^Ã(n),
  so "any proof of NEXP ⊄ P/poly will require non-algebrizing techniques"; Theorems 5.7, 5.8:
  EXP^{NP^A} ⊄ P^Ã/poly and BPEXP^A ⊄ P^Ã/poly.
* Section 1.2 summary list: "NP^Ã ⊆ P^A, and indeed PSPACE^Ã ⊆ P^A; NP^A ⊄ P^Ã, and indeed
  RP^A ⊄ P^Ã; NP^A ⊄ BPP^Ã, and indeed NP^A ⊄ BQP^Ã and NP^A ⊄ coMA^Ã; P^{NP^A} ⊄ PP^Ã;
  NEXP^A ⊄ P^Ã/poly; NP^A ⊄ SIZE^Ã(n)". "These results imply, in particular, that any resolution
  of the P versus NP problem will need to use non-algebrizing techniques. But the take-home message
  for complexity theorists is stronger: non-algebrizing techniques will be needed even to
  derandomize RP, to separate NEXP from P/poly, or to prove superlinear circuit lower bounds for
  NP."
* "Theorem 10.2 Any proof of P ≠ NP will require techniques that are not merely non-algebrizing,
  but non-k-algebrizing for every constant k." (Iterated extensions do not help for this
  direction; for RP vs P and NEXP vs P/poly "we do not know whether double-algebrizing techniques
  already suffice.")

### 3.4 What it rules out

The arithmetization method: proofs that "start with a polynomial-size Boolean formula φ, and use φ
to produce a low-degree polynomial p", and then treat p "as an arbitrary black-box function,
subject only to the constraint that deg(p) is small" (Section 11). This covers the interactive
proof theorems and the circuit lower bounds derived from them (Buhrman-Fortnow-Thierauf's
MA_EXP ⊄ P/poly, Vinodchandran's PP ⊄ SIZE(n^k), Santhanam's PromiseMA ⊄ SIZE(n^k), all named in
Section 1.1). In the authors' words (Section 1.2): "Algebrization provides nearly the precise limit
on the non-relativizing techniques of the last two decades."

For an approach to SAT ∈ P the relevant statement is Theorem 5.3: a polynomial-time algorithm
whose correctness proof would go through if the algorithm were also given evaluation access to a
low-degree extension of the clause-satisfaction function cannot exist.

### 3.5 What it does not rule out, in the authors' words

* Section 9: "the small-depth circuit lower bounds are already 'well covered' by the natural
  proofs barrier"; on local checkability and time-space tradeoffs "there is legitimate debate about
  whether the results listed in (2) and (3) should 'truly' be considered non-relativizing"; and
  "Our results tell us a great deal about the future prospects for arithmetization, but about other
  non-relativizing techniques they are comparatively silent."
* Section 11, direction (2): "given that a polynomial Ã : F^n → F was produced by arithmetizing a
  small Boolean formula, does Ã have any properties besides low degree that a polynomial-time
  algorithm querying it could exploit?" Footnote 16 shows the formula can be recovered from Ã by
  queries, so an algorithm that uses the formula's structure is not an algebrizing algorithm.
* Section 10.2 extends the limitation to non-commutative algebras (matrix algebras and other
  associative algebras with identity over a field); escaping by "lifting to other algebras" is
  therefore constrained too.
* The asymmetric definition leaves open a stricter symmetric notion; the axiomatic (IKK 2009) and
  affine-relativization (AB 2018) frameworks address this **[metadata S0]**, content unverified.
* Williams' ACC^0 lower bound (2011) post-dates this paper; whether it algebrizes is addressed in
  `LANDSCAPE.md` with its own verification tag, not here.

### 3.6 Algebrization checklist

* A1. Does the argument use the formula or machine only through (a) Boolean evaluation and (b)
  evaluation of a low-degree arithmetization at non-Boolean points? If yes, it algebrizes and
  cannot prove P ≠ NP (Theorem 5.1), P = NP (Theorem 5.3), NP ⊄ P/poly (5.5) or NEXP ⊄ P/poly
  (5.6).
* A2. Name the step that uses structure beyond low degree: clause locality, gate fan-in,
  the specific coefficients of the polynomial, the fact that the polynomial came from a small
  formula (recoverable by footnote 16), self-reducibility of SAT, or a combinatorial property of
  the truth table (which then needs Section 2).
* A3. Rewrite the argument for machines with access to both A and an extension Ã of constant
  multidegree, placing Ã on the side Definition 2.3 prescribes for the direction being proved.
  Which step fails?
* A4. If the argument iterates arithmetization (arithmetize, Booleanize, arithmetize again), note
  that Theorem 10.2 closes that route for P ≠ NP.
* A5. If the argument changes the algebra (matrices, non-commutative rings), note Section 10.2.

---

## 4. Other barriers (pointers only; verified in `LANDSCAPE.md` where stated)

* **2024 to 2026 refinements of the three barriers, abstracts read in S0 [read S0]:**
  * Loff, Sherif, Talebanfard, Ugazio (arXiv 2606.12631, June 2026): an *unconditional* barrier
    for AC^0-natural proofs (distinguishers in AC^0, which they argue covers switching-lemma-based
    techniques): no such proof gives bounds above 2^{n^{7/(d−5)}} against depth-d circuits. This
    removes the cryptographic assumption from Section 2.3 for that class of techniques, in the
    quantitative regime of the switching lemma itself.
  * Raz (ECCC TR26-008, 2026): natural proofs extended to linear functions over finite fields;
    under trapdoored-matrix assumptions natural proofs cannot give super-linear lower bounds for
    explicit linear functions. Relevant to any candidate about rigidity or linear circuits.
  * Chen, Hu, Ren (arXiv 2511.14038, ITCS 2026): new algebrization barriers via the communication
    complexity of XOR-Missing-String: oracles with multilinear extensions under which PostBPE and
    BPE have linear-size oracle circuits and a natural subclass of MA_E has h(n)-size oracle
    circuits for every super-half-exponential h. Extends Section 3.3 to the classes where the
    algorithmic method and range avoidance currently operate.
  * Vyas, Williams (ECCC TR24-113, 2024): an oracle relative to which SAT is solvable in
    half-exponential time while EXP has polynomial-size circuits; the missing-string equivalence
    with circuit lower bounds holds "in a relativizing way". The 2024 Gödel Prize citation for
    Williams' ACC result states that it "overcame several barriers against proving circuit lower
    bounds: the Baker-Gill-Solovay relativization barrier, the Razborov-Rudich natural proofs
    barrier, and the Aaronson-Wigderson algebrization barrier" (sigact.org, read in S0). The
    checklist consequence: a candidate that uses the algorithmic method must say which of the
    Vyas-Williams and Chen-Hu-Ren oracle worlds its argument fails in.
* The locality barrier for hardness magnification (Chen, Hirahara, Oliveira, Pich, Rajgopal,
  Santhanam 2020): verified in `LANDSCAPE.md` Section 4 (ECCC TR19-168 abstract read by a
  subagent): every existing magnification theorem gives the target problem efficient circuits
  with small fan-in oracle gates, and weak-model lower-bound techniques extend to such circuits.
* Unprovability of circuit lower bounds in fragments of bounded arithmetic (Razborov 1995; Pich;
  Pich and Santhanam 2021): see `LANDSCAPE.md`. RR94 p. 1 already notes that "in certain fragments
  of Bounded arithmetic any proof of superpolynomial lower bounds for general circuits would
  naturalize".
* For algorithmic approaches to SAT the operative obstacles are not these three barriers but the
  proof-complexity lower bounds and automatizability results collected in `D:\PvsNP\PHASE3.md`
  Section 1 (its own citations, not re-verified in S0) and in `LANDSCAPE.md`.

---

## 5. The combined checklist a candidate must pass before any status above `conjecture`

Every CANDIDATES.md entry and every ledger entry that proposes a technique must contain a filled
copy of this table. "Not applicable" is an acceptable answer only with a one-line reason.

| Item | Question | Answer required |
|---|---|---|
| R1 | Step that uses explicit structure rather than black-box behaviour | name the step |
| R2 | Oracle rewrite: first failing step and the oracle it fails for | name both |
| R3 | Lean parametrization over `A : Oracle` (if a Lean development exists) | first non-generalizing lemma |
| R4 | Query-cost convention | state it |
| R5 | Non-relativizing ingredient (local checkability, arithmetization, other) | name it |
| N1 | The truth-table property C_n implicit in a lower-bound argument | write it |
| N2 | Largeness: fraction of functions for which the argument works | number or "not large" with reason |
| N3 | Constructivity: cost of deciding C_n from a truth table | time bound or "not constructive" with reason |
| N4 | Does the target class plausibly contain 2^{n^ε}-hard pseudorandom functions? | yes / no with source |
| N5 | Escape route (non-large / non-constructive / class without PRGs / not a truth-table property) | name it |
| N6 | Conditional on what? | assumption or "unconditional" |
| A1 | Uses only Boolean evaluation plus low-degree extension access? | yes / no |
| A2 | Step using structure beyond low degree | name it |
| A3 | Extension-oracle rewrite: first failing step | name it |
| A4 | Iterated arithmetization? | yes / no |
| A5 | Change of algebra? | yes / no |
| L | Locality or magnification route? | if yes, address the locality barrier in `LANDSCAPE.md` |

A candidate that cannot fill the table is not ready to be attacked. A candidate whose table says
"relativizes", "natural against a class with PRGs", or "algebrizes" for the statement it targets
is dead on arrival and goes into the ledger as `refuted` with the table as evidence.
