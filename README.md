<!-- solve-problem:6b3ff300-eb95-4718-a016-a932229b3118 -->
# Public-key encryption from one-way functions

A [Tangle Solve problem](https://solve.tangle.tools/problems/6b3ff300-eb95-4718-a016-a932229b3118).

## What is the problem?

The question is whether public-key encryption (PKE) can be constructed from arbitrary one-way functions (OWFs), or whether this is impossible in a meaningful sense.

Formal setting: We work in a world where the only cryptographic assumption is the existence of a one-way function family f: {0,1}^n → {0,1}^n (polynomial-time computable, hard to invert on average). Private-key primitives — pseudorandom generators, pseudorandom functions, secret-key encryption, commitments — are all known to be equivalent to OWFs (HILL, GGM constructions). Public-key encryption, however, has no known construction from arbitrary OWFs: all known PKE schemes rely on structured assumptions (factoring, lattices, etc.).

Two possible resolutions:
1. **Positive:** A (possibly non-black-box) construction of PKE from any OWF.
2. **Negative:** An impossibility theorem stronger than Impagliazzo–Rudich's, e.g., ruling out relativizing and/or certain non-black-box proof techniques.

The precise statement is inherently about *proof techniques*, since PKE from OWFs is a well-defined mathematical question (either it exists or not); the known barrier rules out fully black-box reductions, where the construction and security proof treat the OWF and the adversary as oracles.

## Why does it matter?

A positive answer would unify cryptography: any minimally hard-to-invert function would suffice for the full suite of public-key primitives, dramatically broadening the assumptions on which modern cryptography could rest. A convincing negative answer would establish a fundamental separation between symmetric and asymmetric cryptography, showing public-key cryptography genuinely requires more structure — a long-sought formalization of the intuition behind e.g. minicrypt vs. cryptomania in Impagliazzo's five-worlds framework. Either answer is a landmark; the current ambiguity leaves the most basic question of asymmetric cryptography's foundations open.

## What's already known?

- **Equivalences on the private-key side:** Håstad et al. (HILL) constructed PRGs from any OWF; GGM extended PRGs to PRFs; hence OWF ⇔ PRG ⇔ secret-key encryption, commitments, digital signatures (via one-way functions).
- **Black-box barrier:** Impagliazzo–Rudich (1989) showed there is an oracle relative to which one-way permutations exist but key agreement (hence PKE) does not. Thus no fully black-box construction of key agreement from OWPs exists. This extends to OWFs.
- **Refinements:** Mahmoody, Maji, and Prabhakaran (arXiv:1205.3554) extended such separations to random-oracle-model protocols for secure computation, showing even non-black-box use of a random oracle cannot beat black-box techniques in that setting — evidence that merely "using the code of f" non-black-boxly may not suffice, since their proof handles adversaries/procedures that get the oracle's code.
- **Partial positives:** Special classes of OWFs yield PKE (e.g., lossy/1-to-1-injective OWFs, homomorphic OWFs). Nothing works for arbitrary OWFs.
- **Relativization landscape:** Gertner, Malkin, Reingold and others characterized which primitives imply which in a black-box sense; PKE sits strictly above the "minicrypt" (OWF-implied) class in all known black-box taxonomies.

The gap: the IR barrier rules out fully black-box proofs but says nothing about constructions that use the *code* of f in an essential non-black-box way (cf. how Barak's black-box lower bounds for hashing were bypassed non-black-boxly).

## What would count as solving it?

- **Positive:** An explicit reduction R and encryption scheme (E, D) with a proof that if f is a one-way function then (E, D) is IND-CPA secure; the proof may be arbitrary (non-black-box, non-relativizing). Referee criterion: a complete, verified proof under standard definitions (IND-CPA vs. PPT adversaries), for arbitrary OWFs, not just permutations or special subclasses.
- **Negative:** A theorem exhibiting an oracle (or other barrier framework, e.g., algebrization/natural-proofs-style) relative to which OWFs exist and PKE does not, in a model that permits non-black-box access to f. Referee criterion: the barrier model must provably capture a broad class of techniques, and the paper must argue coverage of known non-black-box methods.
- **Partial credit:** separating black-box degrees (e.g., ruling out all non-black-box constructions where only the *construction* is non-black-box but the *reduction* is black-box), or PKE from a natural intermediate primitive implied by all OWFs.

## How to start

1. Read Impagliazzo–Rudich (1989) carefully — Sections on the oracle construction (choose random oracle O making it a OWF while breaking all key-agreement protocols) — to understand exactly which proof patterns are and are not covered.
2. Read Mahmoody–Maji–Prabhakaran (arXiv:1205.3554), especially their handling of non-black-box parties via "programmable" analysis of oracle-dependent algorithms; this is the closest thing to a barrier against non-black-box techniques.
3. Understand why the natural candidates fail: OWFs give hardness *amplification* and PRGs, but PKE seems to need a way to publish a trapdoor-compatible public value; look at known PKE-from-structure constructions (Blum–Goldwasser, Regev) to see what structural property OWFs lack.
4. Main obstacle: any relativizing argument dies at the IR oracle; any construction attempt has no obvious handle, since arbitrary OWFs may be maximally unstructured. Consider whether a restriction-with-proof (e.g., PKE from all OWFs conditioned on derandomization assumptions, à la Pseudorandomness barrier work) is a tractable intermediate target.

## Key references

- R. Impagliazzo and S. Rudich, *Limits on the Provable Consequences of One-Way Permutations*, STOC 1989. Constructs an oracle relative to which one-way permutations exist but key agreement (hence PKE) is impossible, ruling out fully black-box constructions of PKE from OWPs.
- M. Mahmoody, H. K. Maji, M. Prabhakaran, *Limits of Random Oracles in Secure Computation*, arXiv:1205:3554. Shows that even protocols making non-black-box use of a random oracle cannot achieve secure computation beyond black-box limits, refining the IR-style barrier.
- (Background, verify) J. Håstad, R. Impagliazzo, L. Levin, M. Luby, *A Pseudorandom Generator from any One-Way Function*, SIAM J. Comput. 1999. Proves OWF ⇒ PRG, the foundational private-key result a positive answer would have to build on.
- (Background, verify) O. Goldreich, S. Goldwasser, S. Micali, *How to Construct Random Functions*, JACM 1986. PRG ⇒ PRF chain completing the OWF-implied private-key toolkit.

## Warnings

- The IR barrier is frequently overstated: it rules out *fully black-box* constructions only. Do not cite it as proving impossibility of PKE from OWFs — it does not.
- Conversely, do not propose a construction whose security proof secretly relativizes; it will fall to the IR oracle immediately. Check: does your proof hold if the OWF, adversary, and users get access to an arbitrary extra oracle?
- Non-black-box techniques (à la Barak 2001) circumvent analogous barriers elsewhere; but here the difficulty is that an arbitrary OWF has no exploitable code structure — there is no canonical OWF to hardwire.
- Ambiguity in formulation: "impossibility theorem" must specify a proof-class (relativizing, black-box, algebrizing…); an unconditional impossibility of the mathematical statement itself is not currently conceivable since it would require resolving whether OWFs exist.
- Beware constructions from OWFs *with extra properties* (injectivity, lossiness) — these are known and do not address the problem.

---
*`ALGO-000200` · flagged for review*

## Acceptance guidance

- **Positive:** An explicit reduction R and encryption scheme (E, D) with a proof that if f is a one-way function then (E, D) is IND-CPA secure; the proof may be arbitrary (non-black-box, non-relativizing). Referee criterion: a complete, verified proof under standard definitions (IND-CPA vs. PPT adversaries), for arbitrary OWFs, not just permutations or special subclasses.
- **Negative:** A theorem exhibiting an oracle (or other barrier framework, e.g., algebrization/natural-proofs-style) relative to which OWFs exist and PKE does not, in a model that permits non-black-box access to f. Referee criterion: the barrier model must provably capture a broad class of techniques, and the paper must argue coverage of known non-black-box methods.
- **Partial credit:** separating black-box degrees (e.g., ruling out all non-black-box constructions where only the *construction* is non-black-box but the *reduction* is black-box), or PKE from a natural intermediate primitive implied by all OWFs.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/6b3ff300-eb95-4718-a016-a932229b3118?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000200.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000200)
