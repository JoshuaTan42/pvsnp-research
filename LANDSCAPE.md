# LANDSCAPE.md: lower-bound techniques and proof-complexity frontiers, as of 2026-10-08

Written in session S0. This is a map of what is known, what is the best known, and what is open,
with a verification tag on every claim. It is not a history and it does not try to be complete;
it is meant to stop this lab from rediscovering known results or claiming novelty where there is
none.

## 0. How this file was made, and the tags

Four research subagents of S0 (fresh sessions, same model as S0, `claude-fable-5-1`) each surveyed
one area with WebSearch and WebFetch under a strict provenance rule: a claim counts as verified
only if the agent fetched a page stating it and returned a verbatim quote. Their hand-back
reports are stored unedited in `sessions/S0-evidence/` (`survey-circuits.txt`,
`survey-algebraic-meta-structural.txt`, `survey-formalization-sat.txt`,
`survey-proof-complexity.txt`). S0 itself read the three barrier papers (see `BARRIERS.md`) and
fetched the 2025 to 2026 items that matter most for this lab's plans, which are marked "S0" below.
Caveat that applies to every fetched abstract: the fetch tool returns short excerpts (about 125
characters each), so numbers inside long abstracts were captured only when they fell inside an
excerpt; where a number was not captured it is tagged accordingly.

Tags:

* **[V]** verified from a fetched primary page (ECCC, arXiv abstract, journal or proceedings page,
  Crossref record, author repository) in S0, by S0 directly or by a subagent with a quote on file.
* **[V2]** verified only from a secondary page (Wikipedia, a blog, lecture notes, a problem tracker).
* **[S]** seen only in search-result snippets; unverified.
* **[M]** memory only; unverified. Avoided wherever possible.
* **[L]** verified locally by reading files on this machine (the background repositories or the
  pinned Mathlib package).

Venue and year information is often **[S]** even when the result is **[V]**: abstracts rarely
carry the venue. Nothing in this file was promoted to [V] on the strength of a snippet.

## 1. Boolean circuit lower bounds (non-uniform)

### 1.1 Best known, by model

| Model | Best explicit lower bound | Who | Tag | Source |
|---|---|---|---|---|
| General circuits, full binary basis B2 | 3.1n − o(n) for affine dispersers (constructible in P) | Li, Yang (ECCC TR21-023, 2021; STOC 2022 [S]) | [V] | eccc.weizmann.ac.il/report/2021/023/ |
| same, previous | (3 + 1/86)n − o(n), affine disperser | Find, Golovnev, Hirsch, Kulikov (ECCC TR15-166; FOCS 2016) | [V] | eccc.weizmann.ac.il/report/2015/166/ |
| De Morgan formulas | Ω(n^3 / (log^2 n · log log n)) for Andreev's function | Tal (ECCC TR14-048, 2014), removing log factors from Håstad 1998 | [V] | eccc.weizmann.ac.il/report/2014/048/ |
| AC^0 (depth d) | parity needs size exp(Ω_d(n^{1/(d−1)})) | Håstad 1986 (after Furst-Saxe-Sipser, Ajtai, Yao) | [V2] for attribution; the formula is stated in the abstract of the 2026 Lean formalization (Section 6.3), [V] S0 | en.wikipedia.org/wiki/Switching_lemma; arxiv.org/abs/2609.24188 |
| AC^0[p], p prime | MOD_m ∉ AC^0[p] unless m is a power of p | Razborov 1987, Smolensky 1987 | [V2] | en.wikipedia.org/wiki/ACC0 |
| ACC^0 (composite moduli) | NQP ⊄ ACC^0 ∘ THR of size n^{log^k n}, every k | Murray, Williams (ECCC TR17-188; STOC 2018) | [V] | eccc.weizmann.ac.il/report/2017/188/ |
| ACC^0, original | NEXP ⊄ ACC^0 (poly size); E^NP ⊄ ACC^0 of size 2^{n^{o(1)}} | Williams (CCC 2011; JACM 61(1), 2014) | [V] via Crossref abstract | api.crossref.org/works/10.1145/2559903 |
| ACC^0, almost everywhere | f ∈ E^NP not approximable by 2^{n^ε}-size ACC^0 for all large n | Chen, Lyu, Williams (ECCC TR20-150; FOCS 2020 [S]) | [V] | eccc.weizmann.ac.il/report/2020/150/ |
| ACC^0, average case | NQP not (1/2 + 2^{−log^a n})-approximable by 2^{log^a n}-size ACC^0 ∘ THR | Chen, Ren (ECCC TR20-010; STOC 2020 [S]) | [V] | eccc.weizmann.ac.il/report/2020/010/ |
| TC^0 | depth-2 linear threshold circuits: Andreev's function needs ω(n^{3/2}/log^3 n) gates and ω(n^{5/2}/log^{7/2} n) wires (up to ε factors); nothing superpolynomial for any depth | Kane, Williams (arXiv 1511.07860; STOC 2016 [M]); Chen, Tell (ECCC TR18-199) for the "slightly super-linear" state | [V] | arxiv.org/abs/1511.07860; eccc.weizmann.ac.il/report/2018/199/ |
| Depth 3 (AND/OR/NOT) | 2^{Θ(√n)} for parity (Paturi, Pudlák, Zane); bottom fan-in 2: (9/5)^n for inner product, tight | PPZ 1997/1999 [S]; Göös, Guan, Mosnoi 2023/2024 and Gurumukhani, Kleber, Paturi, Rosin, Talebanfard (arXiv 2601.04446, 2026) [V] | [S]/[V] | arxiv.org/abs/2601.04446 |
| Monotone circuits, explicit function | exp(n^{1/2−o(1)}) (Harnik-Raz function) | Cavalar, Kumar, Rossman (arXiv 2012.03883; Algorithmica 2022) | [V] | arxiv.org/abs/2012.03883 |
| Monotone circuits, a function in P | perfect matching needs 2^{n^{Ω(1)}} (improving Razborov's n^{Ω(log n)}) | Cavalar, Göös, Riazanov, Sofronova, Sokolov (ECCC TR25-102, 2025) | [V] | eccc.weizmann.ac.il/report/2025/102/ |
| Monotone formulas, span programs, switching networks, comparator circuits | 2^{αN} for an explicit function in NP | Pitassi, Robere (ECCC TR16-188; STOC 2017 [M]) | [V] | eccc.weizmann.ac.il/report/2016/188/ |
| Near-maximum size 2^n/n | S_2E/1 (Chen, Hirahara, Ren); S_2E almost everywhere without advice (Li); E^{prMA}/1 (Ren, Williams 2026) | ECCC TR23-144, TR23-156, TR26-118 | [V] | eccc.weizmann.ac.il/report/2023/144/, /2023/156/, /2026/118/ |

Context facts, all [V] unless marked: gate elimination, the only technique behind the 3n-type
bounds, "is inherently limited to proving lower bounds of less than 5n" (Golovnev, Kulikov,
Williams, arXiv 1811.04828). The KRW conjecture (composition of depth complexity; its validity
implies P ⊄ NC^1) is proved for monotone inner functions with lifting-based depth lower bounds
(de Rezende, Meir, Nordström, Pitassi, Robere, ECCC TR20-099) and in a "strong composition"
variant (Meir, ECCC TR23-078), and is otherwise open. Whether ACC^0 can compute majority is open
[V2]. Before Williams 2011 "it was not known whether EXP^NP had depth-3 polynomial-size circuits
made out of only MOD6 gates" (JACM abstract via Crossref, [V]). Chen and Tell (TR18-199): wire
lower bounds n^{1+c^{−d}} for depth-d TC^0 with small enough c would give TC^0 ≠ NC^1; known only
for c ≈ 2.41 [V]. 2026 items: Carmosino, Dang, Jackman give a constructive (refuter-based) gate
elimination framework re-proving Schnorr's 3(n−1) for XOR in the De Morgan basis, no new numerical
bound (arXiv 2602.17942) [V]; Riazanov, Sofronova, Sokolov give top-down lower bounds for a
subclass of AC^0[2] containing DNF-of-parities and depth-3 AC^0 (ECCC TR25-024) [V].

### 1.2 Barrier status of the techniques (which barrier each technique is subject to)

| Technique | Natural (RR94 class Γ) | Relativizes | Algebrizes | Source and tag |
|---|---|---|---|---|
| Random restrictions / switching lemma (AC^0, formulas) | yes, AC^0-natural (parity), AC^0-natural (shrinkage) | fails relative to suitable oracle gates (AW09 §9) | n/a | RR94 §3.1, §3.4; AW09 §9 (S0 read) [V] |
| Polynomial approximation (AC^0[p]) | yes, NC^2-natural | | | RR94 §3.2 [V] |
| Discrepancy (depth-2 threshold) | yes, TC^0-natural | | | RR94 §3.5 [V] |
| Approximation method (monotone) | RR94: no formal largeness analogue; the AFP formalization of Gordeev's monotone argument exists (Section 6) | not applicable | not applicable | RR94 p. 3 [V] |
| Gate elimination (3n-type bounds) | not stated on any fetched page | | | [M] |
| Diagonalization, counting | non-natural by RR94 p. 4 and AW09 p. 2 | relativizes (BGS) | | [V] |
| Arithmetization (IP = PSPACE and the circuit bounds derived from it: MA_EXP ⊄ P/poly, PP and PromiseMA ⊄ SIZE(n^k)) | non-natural (diagonalization inside) | non-relativizing | algebrizes (AW09 Thms 3.6 to 3.18) | [V] |
| Algorithmic method (ACC^0: Williams; Murray-Williams) | the 2024 Gödel Prize citation says the result "overcame several barriers against proving circuit lower bounds: the Baker-Gill-Solovay relativization barrier, the Razborov-Rudich natural proofs barrier, and the Aaronson-Wigderson algebrization barrier" (sigact.org citation page, S0) [V]; Vyas and Williams (ECCC TR24-113, S0) construct an oracle relative to which SAT is solvable in half-exponential time while EXP has polynomial-size circuits, and note the missing-string equivalence holds "in a relativizing way" [V]; Chen, Hu, Ren (arXiv 2511.14038, ITCS 2026, S0) give new algebrization barriers: oracles with multilinear extensions under which PostBPE and BPE have linear-size oracle circuits and a natural subclass of MA_E has h(n)-size oracle circuits for every super-half-exponential h [V] |
| Range avoidance / iterative win-win (S_2E bounds) | none of the fetched abstracts discuss barriers | | | [absence] |

Two 2026 barrier results found by S0 (both [V], abstracts read):

* Loff, Sherif, Talebanfard, Ugazio, "The Switching Lemma shows what the Switching Lemma cannot
  prove: an unconditional natural-proofs barrier" (arXiv 2606.12631, June 2026): AC^0-natural
  proofs (distinguishers computable in AC^0, which the authors argue covers switching-lemma-based
  and most constant-depth lower-bound techniques) cannot prove lower bounds above 2^{n^{7/(d−5)}}
  against depth-d circuits, unconditionally, by localizing the Trevisan-Xue generator; the barrier
  is in the same quantitative regime as the switching lemma frontier 2^{n^{1/(d−1)}}.
* Raz, "A Note on Natural-Proofs for Super-Linear Lower Bounds for Linear Functions" (ECCC
  TR26-008, January 2026, revised July 2026): extends the natural-proofs framework to linear
  functions over finite fields; under strong cryptographic assumptions (trapdoored matrices),
  natural proofs cannot give bounds above n·polylog(n), and under less standard assumptions not
  even super-linear ones.

## 2. Propositional proof complexity

Sources with quotes are in `sessions/S0-evidence/survey-proof-complexity.txt`. The subagent read
the text of Buss and Nordström, "Proof Complexity and SAT Solving" (Handbook of Satisfiability, 2nd
ed., 2021, chapter preprint at jakobnordstrom.se) and Alekseev, Theory of Computing 22(4), 2026,
and fetched the ECCC and arXiv pages named below. The older table in `D:\PvsNP\PHASE3.md` Section
1.3 was not re-verified.

* **Framework.** Cook and Reckhow (JSL 44(1), 1979): p-simulation; a polynomially bounded proof
  system would give NP = coNP [V]. CDCL with nondeterministic choices polynomially simulates
  resolution (Pipatsrisawat, Darwiche 2011; Atserias, Fichte, Thurley 2011) [V].
* **Resolution.** PHP needs length exp(Ω(n)) (Haken 1985); Tseitin on expanders exp(Ω(N))
  (Urquhart 1987); random k-CNF exp(Ω(N)) a.a.s. (Chvátal, Szemerédi 1988); size ≥
  exp(Ω((width − k)^2 / n)) (Ben-Sasson, Wigderson 2001) [all V via the chapter].
* **Cutting planes.** Pudlák 1997: clique-colouring exponential via interpolation to monotone real
  circuits [V]; random Θ(log n)-CNF exponential (Fleming, Pankratov, Pitassi, Robere; Hrubeš,
  Pudlák; FOCS 2017) [V]; Garg, Göös, Kamath, Sokolov lifting (ECCC TR17-175; STOC 2018) [V];
  Tseitin has quasi-polynomial CP refutations (Dadush, Tiwari, CCC 2020) [V]. **Open:** CP lower
  bounds for random k-CNF with constant k (chapter Open Problem 7.16) [V].
* **Bounded-depth Frege.** PHP exponential for every constant depth (Ajtai 1988; Pitassi, Beame,
  Impagliazzo 1993; Krajíček, Pudlák, Woods 1995) [V]; Tseitin on expanders needs depth-d size
  exp(Ω((log n)^2/d^2)) (Pitassi, Rossman, Servedio, Tan 2016) [V]; grid Tseitin
  exp(Ω(n^{1/58(d+1)})) (Håstad, ECCC TR17-142) [V]; PHP exponential in n^{Ω(1/d)} (Håstad, ECCC
  TR23-042, 2023) [V]. **Open:** superpolynomial bounded-depth Frege bounds for random 3-CNF; best is
  Ω(n^{1+ε_k}) steps (Gryaznov, Talebanfard, arXiv 2403.02275), who call the general question a
  "major open problem" [V]; Carenini (ECCC TR26-158, August 2026): random 3-CNF exponentially hard for
  Res(k) up to k = O(√log n) [V].
* **AC^0[p]-Frege, Frege, extended Frege.** No superpolynomial lower bound for AC^0[p]-Frege for any
  family: chapter Open Problem 7.22, and Impagliazzo, Mouli, Pitassi (ECCC TR19-024): "to date there
  has been no progress on AC^0[p]-Frege lower bounds" [V]. Frege and EF superpolynomial bounds open
  (Alekseev 2026 abstract; chapter Open Problems 7.19, 7.20) [V]; best known Frege bound quadratic
  [S]. Random k-CNF is the only family in the chapter whose Frege and extended-resolution status is
  unknown, "the usual conjecture is that they do not" have short proofs [V].
* **Algebraic systems.** Nullstellensatz (Beame, Impagliazzo, Krajíček, Pitassi, Pudlák 1996) with
  designs [V]; polynomial calculus (Clegg, Edmonds, Impagliazzo 1996), PHP degree lower bounds
  (Razborov 1998), size from degree (Impagliazzo, Pudlák, Sgall 1999), random k-CNF in every
  characteristic (Ben-Sasson, Impagliazzo 1999; Alekhnovich, Razborov 2003) [V]; SoS degree bounds
  (Grigoriev 2001; Schoenebeck 2008) [M, not fetched].
* **Resolution over parities Res(⊕).** Tree-like exponential (Itsykson, Sokolov, APAL 2020) [V];
  regular Res(⊕) refutations of binary PHP need 2^{Ω(n^{1/3}/log n)} (Efremenko, Garlík, Itsykson,
  ECCC TR23-187; STOC 2024), dag-like called "a highly challenging open question" [V]; lifting via
  games and depth c·n·log log n bounds (Alekseev, Itsykson, ECCC TR24-128; STOC 2025) [V]; regular
  Res(⊕) cannot simulate resolution (Bhattacharya, Chattopadhyay, Dvořák, CCC 2024) [V]; depth
  N^{2−ε} bounds for BPHP (Byramji, Impagliazzo, arXiv 2511.20023) and SETH-type bounds for depth-n
  fragments (Efremenko, Itsykson, ECCC TR25-188) [V]. The dag-like case is claimed resolved by
  Braun's September 2026 preprint (below), unaudited.
* **Automatability.** Resolution NP-hard to automate (Atserias, Müller, arXiv 1904.02991; JACM 2020
  [S]) [V]; cutting planes (Göös, Koroth, Mertz, Pitassi, ECCC TR20-049) [V]; Nullstellensatz and
  PC (de Rezende, Göös, Nordström, Pitassi, Robere, Sokolov, ECCC TR20-064; the Sherali-Adams claim
  was retracted in revision 2) [V]; depth-d Frege for every d (Papamakarios, CCC 2024) [V]; Frege
  and EF only conditionally (Bonet, Pitassi, Raz 2000; Krajíček, Pudlák 1998) [V citation, M for the
  assumptions].
* **Provability of lower bounds.** Razborov (Annals of Mathematics 181(2), 2015): a generator hard
  for Res(ε log n), so such systems cannot efficiently prove NP ⊄ P/poly [V]; Pich, Santhanam (STOC
  2021) [V]; Krajíček's proof complexity generators conjecture (BSL 30(1), 2024) [V]; Davis, Robere
  (ECCC TR26-055, April 2026): Res(log) proves the known bounded-depth Frege lower bounds, "the first
  example of a propositional proof system which is capable of proving strong lower bounds against
  itself" [V]; Khaniki, Pich, Sokolov, "Efficient Adversaries" (CCC 2026) [V].
* **Clausal systems without new variables.** PR has short PHP proofs without new variables
  (Heule, Kiesl, Biere, JAR 2020) [V]; DRAT⁻, DSPR⁻, DPR⁻ equivalent with deletion; RAT⁻ exponential
  lower bound (Buss, Thapen, LMCS 2021) [V]; SBC⁻ proofs of binary PHP need 2^{Ω(n)}, frontier now
  SPR⁻ (Yolcu, arXiv 2401.11266) [V]. **Open:** SPR⁻ and PR⁻ lower bounds.
* **2026 frontier items.** Tree-like semantic Frege with bounded line size: superpolynomial bounds
  (de Rezende, Engström, Ghannane, Risse, ECCC TR26-078) [V]; Lu, Santhanam, Tzameret: an explicit
  DNF family has no polynomial-size AC^0[p]-Frege proofs infinitely often (ITCS 2026) [S].

Items verified directly by S0 (abstract pages read, [V]):

* Braun, "An exponential lower bound for the bit pigeonhole principle in resolution over parities"
  (arXiv 2609.23015, 19 September 2026, 37 pages): claims that every DAG-like Res(⊕) refutation of
  the bit pigeonhole principle with n+1 pigeons and n = 2^ℓ holes has more than exp(n/(32768 ℓ^2))
  clauses for ℓ ≥ 32, with no regularity or depth restriction, by translation to polynomial
  calculus with extension variables (Buss, Impagliazzo, Krajíček, Pudlák, Razborov, Sgall style),
  one-shot removal of the extension variables, and a Razborov-style degree bound proved through
  chessboard-complex homology. The main theorem and a general sufficient condition are stated to be
  formalized in Lean 4 (repository `github.com/kbr-/math-research`, commit `8904bf09`, Lean
  4.34.0-rc2, `#print axioms` reported as the three standard axioms, `leanchecker --fresh`
  reported passed), developed "with substantial AI assistance". Its own account of prior work:
  superpolynomial Res(⊕) lower bounds were previously known only for tree-like, regular, or
  bounded-depth refutations, citing Alekseev-Itsykson (STOC 2025), Bhattacharya-Chattopadhyay-
  Dvořák (CCC 2024), Bhattacharya-Chattopadhyay (ECCC TR25-106), Bhattacharya-Byramji-
  Chattopadhyay-Impagliazzo (STOC 2026), Byramji-Impagliazzo (arXiv 2511.20023), Alekseev-Gaevoy
  (ECCC TR26-007). **Status for this lab: a three-week-old single-author preprint with a Lean
  artifact; neither the mathematics nor the Lean statement has been audited by anyone this lab
  knows of. It is not treated as established. See `CANDIDATES.md` C3.**
* Peitl, Szeider, "Finding the Hardest Formulas for Resolution" (JAIR 72, 2021): a SAT encoding
  for the shortest resolution refutation of a given formula, used to compute the first ten
  "resolution hardness numbers" (shortest proof of a hardest formula with m clauses) [V, subagent].
* Grosof, Zhang, Heule, "Towards the shortest DRAT proof of the Pigeonhole Principle" (arXiv
  2207.11284, Pragmatics of SAT 2022): shortest known DRAT proofs of PHP, length O(n^3) with
  leading coefficient 5/2; minimality not claimed [V, subagent].
* No source was found giving exact minimum resolution refutation sizes for specific small PHP_n or
  Tseitin instances [absence after search, subagent].

## 3. Algebraic and geometric approaches

* **Permanent vs determinant.** Best determinantal complexity lower bound for perm_n is quadratic
  (Mignon, Ressayre 2004, n^2/2) [S]; Kumar and Volk prove dc(Σ x_i^n) ≥ 1.5n − 3 and state "it is
  a long standing open problem to prove a lower bound which is super linear in max{n,d}" (arXiv
  2009.02452) [V]; Bedi gives an elementary quadratic bound in positive characteristic (ECCC
  TR24-015) [V]; nothing better for the permanent through 2026 was found [absence].
* **GCT.** Mulmuley and Sohoni proposed separating the GL_{n^2}-orbit closures of the determinant
  and padded permanent via occurrence obstructions; Bürgisser, Ikenmeyer and Panova proved "this
  approach is impossible" while not ruling out multiplicity obstructions (arXiv 1604.06431; JAMS)
  [V]; Ikenmeyer-Panova (rectangular Kronecker coefficients) and Ikenmeyer-Kandasamy (STOC 2020,
  first separation of orbit closures by symmetries) [S]; Bläser, Ikenmeyer, Lysikov, Pandey,
  Schreyer: orbit closure containment for 3-tensors is NP-hard, read by them as a positive sign
  for GCT (arXiv 1911.02534) [V].
* **Constant-depth algebraic circuits.** Limaye, Srinivasan, Tavenas: first superpolynomial lower
  bounds against general algebraic circuits of every constant depth, over characteristic 0 or
  large, for iterated matrix multiplication (ECCC TR21-081; FOCS 2021 [S]) [V]; Forbes: the same
  over any field (CCC 2024, LIPIcs 300, 31:1-31:16) [V]; Amireddy, Garg, Kayal, Saha, Thankey
  bypass set-multilinearization (ICALP 2023) [S]; superpolynomial bounds for unbounded-depth
  algebraic circuits remain open [M for "Baur-Strassen is the best"; no page fetched].
* **Algebraic natural proofs.** Introduced independently by Forbes, Shpilka, Volk and by Grochow,
  Kumar, Saks, Saraf [V attribution via arXiv 2004.14147]; Chatterjee, Kumar, Ramya, Saptharishi,
  Tengse: VP with bounded coefficients has efficient equations, and over characteristic zero VNP
  has no efficient equations if the permanent is exponentially hard (arXiv 2004.14147) [V].
* **Ideal Proof System.** Grochow and Pitassi: superpolynomial IPS lower bounds for any Boolean
  tautology imply VP ≠ VNP (arXiv 1404.3820; JACM 2018 [M]) [V]; Andrews and Forbes: lower bounds
  for low-depth IPS refutations via LST (arXiv 2112.00792; STOC 2022) [V]; Elbaz, Govindasamy, Lu,
  Tzameret: first IPS lower bounds over fixed finite fields for multilinear constant-depth and
  roABP fragments (ECCC TR25-080, 2025; STOC 2026 [S]) [V]; several other 2025 to 2026 fragment
  results [S].
* **Extension complexity (a closed route for P = NP).** Fiorini, Massar, Pokutta, Tiwary, de Wolf:
  no polynomial-size LP projects to the TSP polytope (arXiv 1111.0837; JACM 2015) [V]; Rothvoss:
  the perfect matching polytope has extension complexity 2^{Ω(n)}, so extension complexity does
  not track P vs NP (arXiv 1311.2369; JACM 2017 [M]) [V]; Lee, Raghavendra, Steurer: cut, TSP and
  stable set polytopes need SDP size 2^{n^c}, and polynomial-size SDPs are no stronger than
  low-degree sum-of-squares for max-CSPs (arXiv 1411.6317) [V].

## 4. Meta-complexity, range avoidance, magnification, unprovability

* **MCSP.** Kabanets, Cai 2000 defined MCSP and argued NP-hardness proofs would need circuit lower
  bounds for E [S]. Murray, Williams: MCSP is not NP-hard under O(n^{1/2−ε})-time projections, and
  NP-hardness under general polynomial-time reductions would imply EXP ≠ NP ∩ P/poly (Theory of
  Computing 13(4), 2017) [V]. Hirahara: non-black-box worst-case to average-case reduction for
  approximating MINKT and MCSP (ECCC TR18-138; FOCS 2018 [M]) [V]; NP-hardness of learning
  programs and of partial-function MCSP under randomized reductions, overcoming Ko's relativization
  barrier (ECCC TR22-119; FOCS 2022) [V]; one-way functions exist iff approximating distributional
  Kolmogorov complexity is NP-hard and NP is worst-case hard (ECCC TR23-037; STOC 2023 [S]) [V].
  Ilango: with probability 1 over a random oracle, approximating hypergraph vertex cover reduces in
  P/poly to MCSP^O (ECCC TR23-165; FOCS 2023) [V]. Liu, Pass: one-way functions exist iff K^t is
  mildly hard on average (arXiv 2009.11514; FOCS 2020 [M]) [V]. **Open:** whether MCSP is NP-hard,
  stated open in Ilango (TR23-165) and Hirahara (TR22-119) [V]; no 2025 to 2026 NP-hardness result
  for total MCSP was found [absence].
* **Range avoidance.** Korten: constructing a hard truth table is APEPP-complete under P^NP
  reductions (arXiv 2106.00875; FOCS 2021) [V]; Ren, Santhanam, Wang: circuit lower bounds for
  E^NP are equivalent to circuit-analysis algorithms with E^NP preprocessing (ECCC TR22-048; FOCS
  2022 [M]) [V]; Chen, Hirahara, Ren and Li as in Section 1.1 [V]; Ren, Williams 2026 (E^{prMA}/1)
  [V]; Huang, Li, Zhong (NC^0_k avoidance, ITCS 2026) and Chen, Li, Liang (TR24-182) [S].
* **Hardness magnification.** Oliveira, Santhanam (FOCS 2018) coined it [S]; Oliveira, Pich,
  Santhanam (CCC 2019), McKay, Murray, Williams (STOC 2019), Chen, Jin, Williams (FOCS 2019) [S];
  the **locality barrier**: Chen, Hirahara, Oliveira, Pich, Rajgopal, Santhanam show that every
  existing magnification theorem unconditionally gives the target problem "highly efficient
  circuits extended with small fan-in oracle gates", and lower-bound techniques against weak
  models "quite often easily extend" to such circuits, which "explains why direct adaptations of
  certain lower bounds are unlikely to yield strong complexity separations" (ECCC TR19-168; ITCS
  2020; JACM 2022 [M]) [V].
* **Unprovability in bounded arithmetic.** Razborov 1995 (conditional unprovability of
  superpolynomial circuit lower bounds in S^2_2(α) under strong PRGs) and Pich 2015 [S]; Pich,
  Santhanam: unconditionally, PV cannot prove that any NP language is inapproximable by
  co-nondeterministic circuits of sub-exponential size (STOC 2021; Oxford ORA record) [V]; Li,
  Oliveira: APC1 cannot prove strong lower bounds separating the third level of PH (ECCC TR23-022;
  STOC 2023) [V]; Chen, Li, Oliveira 2024 (IS^1_2; reverse mathematics of lower bounds), Lu,
  Santhanam, Tzameret (TR25-134: AC^0[p]-Frege cannot efficiently prove hardness of constant-depth
  algebraic lower bounds), a LICS 2026 paper on formalizing LST-type bounds in VNC^2 [S]. RR94
  itself notes the naturalization of lower-bound proofs in certain fragments of bounded arithmetic
  [V, S0].

## 5. Structural results 2024 to 2026 (time versus space)

* Cook, Mertz: Tree Evaluation is in space O(log n · log log n), via catalytic ("amortized")
  branching programs; TreeEval's status as a candidate hard problem for L "remains a mystery"
  (ECCC TR23-174; STOC 2024 [S]) [V].
* Williams: TIME[t] ⊆ SPACE[O(√(t log t))] for multitape Turing machines, improving
  Hopcroft-Paul-Valiant's O(t/log t) from 1975; bounded fan-in circuits of size s evaluable in
  √s · polylog(s) space; "a little progress on the P versus PSPACE problem" (ECCC TR25-017; arXiv
  2502.17779; STOC 2025) [V]. Shalunov: direct Circuit Value to Tree Evaluation reduction giving
  O(√(s log s)) space (CCC 2026, LIPIcs 383) [V]. Removing the √(log t) factor is open; one 2025
  preprint claiming SPACE[O(√t)] was withdrawn in January 2026 with an acknowledged error [V2,
  lacker.io tracker].
* Catalytic computation (Buhrman, Cleve, Koucký, Loff, Speelman 2014) underlies Cook-Mertz [V for
  the link; the 2014 bounds themselves [S]].

## 6. Formalization and certified computation (the lab's own ground)

### 6.1 Lean 4 and Mathlib

* Mathlib's `Turing.FinTM2`, `TM2ComputableInTime`, `TM2ComputableInPolyTime` count `step` calls
  and prove polytime computability only for the identity [V, doc mirror]. The pinned Mathlib
  package used by the background repositories (`fabf563a`) contains
  `proof_wanted TM2ComputableInPolyTime.comp` at
  `Mathlib/Computability/TuringMachine/Computable.lean:284` [L, S0]; composition was proved in
  `D:\PvsNP\Comp.lean` (CLAIM-0001's dependency) and an independent Mathlib PR was open (per
  `D:\PvsNP\README.md`, not re-checked).
* `lean-dojo/LeanMillenniumPrizeProblems`: P vs NP stated with "a concrete finite-alphabet
  Turing-machine model and Cook's verifier definition", with a 2026-09 soundness fix ("P versus NP
  certificate encoding fixed (every language was in NP)") [V].
* `SamuelSchlesinger/complexitylib` README claims (not audited): multi-tape TMs, P, NP, BPP, PSPACE,
  Cook-Levin, deterministic time hierarchy, P/poly, and "The PCP theorem: NP = PCP(O(log n), O(1))",
  pinned to Lean v4.35.0-rc3 [V as README claims only].
* `khanukov/pnp3` (Reservoir): states "There is still no unconditional in-repo theorem P != NP";
  an earlier route was formally refuted [V]. Other self-claimed P ≠ NP Lean packages exist [S];
  none is credible without audit.
* Senellart, Gnatenko, "Descriptive Complexity in Lean" (arXiv 2609.18261): logical definitions of
  classes, first-order reductions, 73 completeness results [V].
* **Formalized lower bounds in Lean (2026):**
  * Wesonga, "Formalizing PARITY Circuit Lower Bounds in Lean" (arXiv 2609.24188, 21 September
    2026; repository `github.com/formalcs/circuit-complexity`, release/0.1.0, Lean 4.33.1 and
    Mathlib 4.33.1; the README also claims a Razborov-Smolensky AC^0[p] result): parity needs
    depth-d formulas and circuits of size exp(Ω_d(n^{1/(d−1)})), and NC^1 ⊄ AC^0 in the formalized
    models [V, S0 read abstract, PDF text and README]. Not peer reviewed; the README says nothing
    about `sorry` or axioms; some PDF listings display `sorry` as an abbreviation for omitted
    bodies. Unaudited.
  * Braun, Res(⊕) lower bound for bit-PHP, Section 2 [V, S0]. Unaudited.
  * Fredriksen, `Quantyra/formal-switching-lemma` v0.10.0 (Lean toolchain v4.13.0): a SimpleDNF
    switching lemma with decision-tree encoding; the README states it does not prove or imply any
    NP or circuit lower bound and that its PHP switching lemma "remains open" [V README, S0].
  * Kolmogorov complexity: Coq (Forster, Kunze, Lauermann, ITP 2022) [V]; HOL4 (Catt, Norrish, CPP
    2021) and a Lean 4 package (July 2026) [S]; communication complexity in Lean (2026 preprint,
    log-rank lower bound) [S].
* **Formalized lower bounds elsewhere:** Isabelle AFP, Thiemann, "Clique is not solvable by
  monotone circuits of polynomial size" (2022), following Gordeev's monotone argument, bound
  (n^{1/7})^{n^{1/8}} for clique size n^{1/4} [V]. **Not found anywhere** (searched): Haken's PHP
  resolution lower bound, Razborov-Smolensky in a proof assistant other than the Wesonga README
  claim, Razborov's original monotone argument, Baker-Gill-Solovay or resource-bounded oracle
  machines (the `D:\Relativization` development is therefore the only one this lab knows of), a
  time hierarchy theorem in Isabelle [absence after search].
* **Cook-Levin elsewhere:** Coq (Gäher, Kunze, ITP 2021, "the first result in computational
  complexity theory that has been mechanised with respect to any concrete computational model")
  [V]; Isabelle AFP (Balbach, 2023, via two-tape oblivious TMs) [V]; Coq library of complexity
  with a time hierarchy theorem (`uds-psl/coq-library-complexity`, Coq 8.16) [V]; Forster, Kunze,
  Roth: the weak call-by-value λ-calculus is reasonable for time and space (POPL 2020) [V].

### 6.2 SAT certificates into Lean theorems

* Lean core: `Std.Tactic.BVDecide` (`bv_decide`, `bv_check "file.lrat"`), merged from LeanSAT in
  2024, with "A verified LRAT certificate checker, used to import UNSAT proofs generated by high
  performance SAT solvers, like CaDiCal" and `bv_check` so that "users that do not have a SAT
  solver installed" can replay a stored proof [V, Reservoir page]. Lean 4.12.0 release notes:
  "The external solver CaDiCaL is included with Lean", "the resulting LRAT proof is checked in
  Lean", "proofs generated by this tactic use `Lean.ofReduceBool`" and "this tactic includes the
  Lean compiler as part of the trusted code base" [V, S0]. The pinned toolchain of this lab is
  v4.31.0, later than 4.12.0.
* Szeider, "Streaming LRAT Certificates into Lean Theorems" (arXiv 2607.00815, v2 September 2026;
  the tool `lrat-catcher`): turns an LRAT certificate for an arbitrary CNF into a Lean theorem,
  reusing Lean core's verified LRAT checker made resumable, checking as a stream while the solver
  runs; a soundness theorem states a garbled stream can only cause failure; 174 TB of empty-hexagon
  certificates imported [V, S0]. Szeider, PBLean (arXiv 2602.08692): VeriPB pseudo-Boolean
  certificates (cutting planes with reification) imported into Lean [V].
* Worked examples: Florath, "A Lean-Certified Proof of K_8(4,2) = 23" (arXiv 2606.16688, June
  2026): two Lean-checked LRAT refutations of stored CNFs replayed "with no external SAT solver"
  [V]; Subercaseaux, Przybocki, Tarski's high school algebra problem, smallest countermodels have
  size 12, main result certified in Lean (arXiv 2608.08421, 2026) [V]; the empty hexagon number
  h(6) = 30 (Heule, Scheucher, TACAS 2024 [V]) with a Lean verification (ITP 2024) [S].
* Other verified checkers: cake_lpr (CakeML, LPR/LRAT) [V repo]; GRAT (Isabelle/HOL, gratchk)
  [V]; ACL2 checkers (Heule, Hunt, Kaufmann, Wetzler) [S]; Lammich's Isabelle-LLVM LRAT checker
  (IJCAR 2024) [S].
* Proof-by-SAT landmarks with checked certificates: Pythagorean triples (about 200 TB DRAT, 2016)
  [V]; Schur number five (2 PB, "certified using a formally verified proof checker") [V]; Keller's
  conjecture in dimension 7 [V]; Lam's problem (about 110 TiB) [S].

### 6.3 Exact circuit synthesis and small-case exact complexity

* Kojevnikov, Kulikov, Yaroslavtsev (SAT 2009): SAT solvers to find small circuits, including a
  3n + c circuit for MOD3 over the full binary basis [V]. Kulikov (DATE 2018) on what solvers can
  and cannot find [V]. Kulikov, Pechenev, Slezkin, "SAT-based Circuit Local Improvement" (arXiv
  2102.12579; MFCS 2022 [S]): exact size search for size 7 is almost instant while size 13 can take
  over a week; new upper bounds for symmetric functions by re-synthesizing subcircuits [V].
  Goncharov, Kulikov, Levtsov, "Smaller Circuits for Bit Addition" (STACS 2026) [S]. Haaswijk,
  Soeken, Mishchenko, De Micheli (IEEE TCAD 2020) on encodings and topology families [S];
  "Classifying Functions with Exact Synthesis" (ISMVL 2017) covers all 4- and 5-input functions
  with 3-input operators and hopes for 6 inputs [S].
* Knuth (TAOCP 4A, 7.1.2): minimum cost of all 616,126 NPN classes of 5-input functions; exactly
  one class needs 12 two-input gates [V2, cp4space]. For 6 inputs only the multiplicative
  complexity (AND count) of all 150,357 affine classes is known (Çalık, Sönmez Turan, Peralta), max
  6 ANDs; no complete 6-input table for total gate count was found [S for the 6-input facts;
  absence for the table].
* Exact De Morgan formula sizes known only up to 5 variables (Cox and Healy 2010; OEIS A056287)
  [S].
* **No published result was found that certifies a circuit lower bound for a specific function
  with a DRAT/LRAT certificate checked by a verified checker, and no machine-checked exact circuit
  complexity value for a specific function was found** (searches listed in
  `survey-formalization-sat.txt`, items I1 and L2) [absence after search]. The ingredients exist
  separately: exact-synthesis encodings, and Lean's LRAT import.
* Symmetry breaking tools for such searches: BreakID (SAT 2016), satsuma (SAT 2024), nauty/Traces
  [S].

## 7. What is open (consolidated, with the page that says so)

1. Any superlinear-beyond-3.1n lower bound for general circuits; any n^{3+ε} De Morgan formula
   bound; KRW; P vs NC^1 (Sections 1.1, 1.2) [V for the state; "open" partly by inference].
2. NP ⊄ AC^0[6] (or any composite modulus); whether ACC^0 computes majority [V2].
3. Any superpolynomial TC^0 lower bound [V via Chen-Tell].
4. Superpolynomial circuit lower bounds for E^NP, NEXP or NP (the near-maximum frontier is S_2E
   and E^{prMA}/1) [inference from the frontier, no page states it in those words].
5. Permanent vs determinant; VP vs VNP; superpolynomial unbounded-depth algebraic circuit lower
   bounds [V for the quadratic state].
6. NP-hardness of MCSP [V].
7. Tree Evaluation in L; TIME[t] ⊆ SPACE[√t]; P vs PSPACE [V, V2].
8. Proof complexity: superpolynomial lower bounds for AC^0[p]-Frege, Frege, extended Frege; random
   3-CNF in bounded-depth Frege; dag-like Res(⊕) beyond what Braun's unaudited preprint claims
   (Section 2, to be completed from the proof-complexity survey) [V for the Braun claim; the rest
   pending].

## 8. What this means for the lab

* No technique in Section 1 is within sight of P ≠ NP; every one has a named barrier or a known
  ceiling (5n for gate elimination; the 2026 unconditional AC^0-natural barrier; the locality
  barrier for magnification; algebrization for arithmetization-based bounds). The lab should not
  attempt an asymptotic lower bound for NP against any class above AC^0[p] without a non-natural,
  non-algebrizing ingredient that it can name in the `BARRIERS.md` table.
* The two things this lab can do that almost nobody has done: (a) machine-checked exact small-case
  lower bounds (Section 6.3: no certified exact circuit complexity value exists), and (b) formal
  audits and extensions of the 2026 AI-assisted Lean formalizations of lower bounds (Section 6.1),
  which are unreviewed and sit exactly on the lab's infrastructure (Lean 4, pinned Mathlib, `leanchecker`).
* The pipeline SAT solver → LRAT certificate → Lean theorem exists today (Section 6.2); the lab
  does not need to build a checker, only the encodings and their soundness proofs.
