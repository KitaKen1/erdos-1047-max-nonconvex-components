# A Lean solution to the maximum-component variant of Erdős Problem 1047

This project gives a formally checked answer to
[`Erdos1047.erdos_1047.variants.max_non_convex_components`](https://github.com/google-deepmind/formal-conjectures/blob/main/FormalConjectures/ErdosProblems/1047.lean)
in Formal Conjectures.

The question asks how many non-convex connected components a region
`{z : ℂ | ‖f.eval z‖ < c}` can have when `f` ranges over monic polynomials
of degree `n` and `c > 0`. The maximum is `0` for degrees `0, 1`,
`1` for degree `2`, `2` for degree `3`, and `n` for every `n ≥ 4`.

**Try it in Lean4Web:** [open the standalone proof](https://live.lean-lang.org/#url=https%3A%2F%2Fraw.githubusercontent.com%2FKitaKen1%2Ferdos-1047-max-nonconvex-components%2Frefs%2Fheads%2Fmain%2Flean4web%2FErdos1047Lean4Web_v4_35_0_rc2.lean)

Select a Lean/mathlib `v4.35.0-rc2` project.

## Formal Conjectures target

The [FC proof](lean/Erdos1047MaxNonConvexFC.lean) imports
`FormalConjectures.ErdosProblems.«1047»` at commit `d5ba143c...` and
establishes the following result using FC's definitions and answer annotation:

```lean
theorem erdos_1047_max_non_convex_components_solved (n : ℕ) :
    IsGreatest {k : ℕ | ∃ (f : ℂ[X]) (c : ℝ), f.Monic ∧ f.natDegree = n ∧ 0 < c ∧
      {t ∈ componentsIn (strictSublevelSet f c) | ¬ Convex ℝ t}.ncard = k}
      answer(if n ≤ 1 then 0 else if n = 2 then 1 else if n = 3 then 2 else n) := by
  exact Erdos1047StrictAllDegrees.strict_max_nonconvex_all_degrees n
```

The declaration is in `namespace Erdos1047Proof`, with
`open Polynomial Set Erdos1047`. Its separate name avoids a conflict with
the imported conjecture. `IsGreatest` requires both an upper bound and an
example attaining it.

The corresponding FC update would fill `answer(sorry)` with this expression,
mark the variant `research solved`, and attach a stable public proof link using
`formal_proof`. FC's [contribution guide](https://github.com/google-deepmind/formal-conjectures/blob/main/CONTRIBUTING.md)
places long proofs in external repositories rather than in the conjecture file.

The original Grunsky convexity question is separate and already has a negative
answer. Here the region is defined by `<`, not `≤`, and there is no assumption
that the component count equals the number of distinct roots.

## Mathematical explanation (AI generated)

For a monic polynomial `f` of degree `n` and a positive level `c`, write

```text
U(f, c) = {z ∈ ℂ : |f(z)| < c}.
```

Count the connected components of `U(f, c)` that are not convex as subsets of
the real plane. For positive degree, every component contains a root, giving

```text
non-convex components ≤ all components ≤ distinct roots ≤ n.
```

The low degrees need sharper bounds:

- For `n = 0`, the region is empty or the whole plane; for `n = 1`, it is
  an open disk. Neither case has a non-convex component.
- For `n = 2`, the classification of quadratic sublevel regions gives an
  upper bound of one.
- For `n = 3`, a normalized cubic analysis gives an upper bound of two,
  attained by an explicit example.

For the quadratic lower bound, take `f(z) = z² − 1` and `c = 6/5`.
The region is connected. The points `1 + i/2` and `−1 + i/2` lie inside,
while their midpoint `i/2` lies outside:

```text
|f(1 + i/2)| = |f(−1 + i/2)| = √17/4 < 6/5,
|f(i/2)| = 5/4 > 6/5.
```

For `n ≥ 4`, start with degree-four and degree-five examples. Multiplication
by a carefully chosen quadratic factor preserves the old non-convex components
and creates two additional ones. Repeating this step realizes `n` such
components in every degree at least four, matching the general upper bound.

## Files

| Directory | Lean version | Purpose |
| --- | --- | --- |
| [`lean/`](lean/Erdos1047MaxNonConvexFC.lean) | `v4.33.1` | Proof using the FC definitions, pinned to `d5ba143c...` |
| [`lean4web/`](lean4web/Erdos1047Lean4Web_v4_35_0_rc2.lean) | `v4.35.0-rc2` | Self-contained Mathlib proof for Lean4Web |

Both directories have a single proof file plus `lakefile.toml`,
`lean-toolchain`, and `lake-manifest.json`. No private modules or development
files are needed to build them.

## Verification

FC package:

```sh
cd lean
lake exe cache get Mathlib
lake build
```

Web package, starting from the repository root:

```sh
cd lean4web
lake exe cache get Mathlib
lake env lean -j1 Erdos1047Lean4Web_v4_35_0_rc2.lean
```

Use the committed manifests to retain the checked dependency versions.
The projects request a single Lean worker to limit memory use.

On 2026-09-18, both complete proof files passed local Lean checking with exit
code 0, taking approximately 229 seconds each. Their final axiom guards accept
only:

```text
[propext, Classical.choice, Quot.sound]
```

Neither final theorem depends on `sorryAx` or FC's admitted conjectures.
Additional type checks verify the fully expanded target; the FC check uses
fully qualified references to FC's own definitions.

## Status boundary

The established result is:

```text
For every degree n, the largest attainable number of non-convex components
of the strict sublevel region is 0, 0, 1, 2, n for n = 0, 1, 2, 3, ≥ 4.
Both the universal bound and attainment are proved.
```

This does not assert:

- that every polynomial of degree `n` attains the maximum;
- a result about a different closed-sublevel or root-count-restricted target;
- acceptance into FC, external validator approval, or human peer review;
- a first-publication or mathematical novelty claim;
- successful execution on a public Lean4Web server.

At the pinned FC revision, this variant is still tagged `research open`.
This repository hosts the proof; submitting the answer/status/proof-link update
to Formal Conjectures is a separate step. No FC pull request has been submitted.
The Web file is about 1.4 MB, so public-server time and memory limits may matter.

## Sources

- [Erdős Problems #1047](https://www.erdosproblems.com/1047).
- [Formal Conjectures: current 1047.lean](https://github.com/google-deepmind/formal-conjectures/blob/main/FormalConjectures/ErdosProblems/1047.lean).
- [The pinned FC statement](https://github.com/google-deepmind/formal-conjectures/blob/d5ba143cc2fafd48cc6d5b6320a3aab287c38df7/FormalConjectures/ErdosProblems/1047.lean).
- P. Erdős, F. Herzog, G. Piranian, [*Metric properties of polynomials*](https://www.renyi.hu/~p_erdos/1958-05.pdf), J. Analyse Math. **6** (1958), 125–148; the original convexity question is Problem 16 on p. 145.
- A. W. Goodman, [*On the Convexity of the Level Curves of a Polynomial*](https://doi.org/10.1090/S0002-9939-1966-0188408-3), Proc. Amer. Math. Soc. **17** (1966), 358–361.

FC attributes the maximum-count question to Goodman. The article's full text
was not independently checked here, and the answer formula is not presented
as a quotation or theorem from that paper.

## AI usage disclosure

OpenAI Codex assisted with the formalization, explanatory text, and repository
preparation.
