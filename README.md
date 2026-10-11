# JacobianVarieties (Lean 4.35.0-rc2)

[![Lean](https://github.com/n-yamaguchi-0729/JacobianVarieties/actions/workflows/lean.yml/badge.svg)](https://github.com/n-yamaguchi-0729/JacobianVarieties/actions/workflows/lean.yml)
[![License](https://img.shields.io/badge/license-Apache--2.0-blue.svg)](LICENSE)

A Lean 4 library for algebraic curves, Picard groups, and Jacobian varieties.
It develops function fields, divisors, differentials, Riemann–Roch, Jacobians
over arbitrary fields, Abel–Jacobi maps, and Albanese factorization, together
with characteristic-zero results on torsion and étale cohomology.

The curve and Jacobian development builds substantially on
[Anthropic's Fermat's Last Theorem in Lean 4](https://github.com/anthropics/fermats-last-theorem),
adapted and extended here. Its only Lake dependency is
[Mathlib](https://github.com/leanprover-community/mathlib4).

## Public API

Import the curve library or the Jacobian library with:

```lean
import AlgebraicCurves.All
import JacobianVarieties.All
```

For the Jacobian construction and its principal properties:

```lean
import JacobianVarieties.JacobianChallenge
```

Individual result files below can also be imported directly.

## Main results

For a smooth proper geometrically irreducible curve `C` over any field `k`,
[`JacobianChallenge`](Lean4/JacobianVarieties/JacobianChallenge.lean) constructs
`Jacobian C` over `k` without requiring a rational point. It equips the
Jacobian with a group structure and proves smoothness, properness, geometric
irreducibility, and dimension equal to the geometric genus.

| Declaration | Mathematical content |
| --- | --- |
| [`finrank_H1_eq_geometricGenus`](Lean4/AlgebraicCurves/Geometry/Curves/GeometricGenus.lean) | For a smooth proper geometrically integral curve over any field, the dimension of structure-sheaf Čech H¹ for a two-affine-open cover equals its geometric genus. |
| [`Jacobian.ofCurve`, `Jacobian.comp_ofCurve`](Lean4/JacobianVarieties/JacobianChallenge.lean) | A rational basepoint defines an Abel–Jacobi morphism to the constructed Jacobian, sending the basepoint to the identity. |
| [`Jacobian.exists_unique_ofCurve_comp`](Lean4/JacobianVarieties/JacobianChallenge.lean) | Every basepoint-preserving morphism from the curve to a smooth proper geometrically irreducible group scheme factors uniquely through Abel–Jacobi as a morphism over `k`. |
| [`exists_abelJacobi_isIso_of_genus_one`](Lean4/JacobianVarieties/Albanese/GenusOneAbelJacobiIsomorphism.lean) | Over an algebraically closed field of characteristic zero, a pointed genus-one curve model has an Abel–Jacobi isomorphism with its Picard variety. |
| [`natCard_torsionLevel_eq_pow`](Lean4/JacobianVarieties/Picard/TorsionCardinality.lean) | For an abelian variety of dimension `d` over an algebraically closed field of characteristic zero and positive `n`, the multiplication kernel has `n^(2*d)` algebraic points. |
| [`CurveModel.natCard_finiteH1_eq_pow_genus`](Lean4/JacobianVarieties/Kummer/FiniteH1Cardinality.lean) | For a pointed geometrically integral curve model over an algebraically closed field of characteristic zero, actual étale H¹ with μₙ coefficients has cardinality `n^(2*g)`, for positive `n`. |
| [`natCard_open_finiteH1_eq_pow_genus_boundary`](Lean4/JacobianVarieties/Kummer/OpenFiniteH1Cardinality.lean) | In the same characteristic-zero setting, a quasi-compact dense integral open curve with finite nonempty boundary of size `r` has `#H¹(U, μₙ) = n^(2*g+r-1)`. |
| [`exists_abelJacobi_fullTate_equiv`](Lean4/JacobianVarieties/Kummer/AbelJacobiTate.lean) | Over an algebraically closed field of characteristic zero, a Picard representation and Abel–Jacobi morphism compatible with point line bundles induce a topological isomorphism `π₁(C)ᵃᵇ ≃ T(J)`. |

The [divisorial Weil pairing](Lean4/JacobianVarieties/WeilPairing/CharZeroNondegeneracy.lean)
is also proved nondegenerate for supplied `DivisorialWeilPairingData` over an
algebraically closed characteristic-zero field, under the principal-divisor
and one-variable function-field hypotheses.

## Scope

The Jacobian construction works over arbitrary fields; the torsion,
genus-one, and étale comparison results above have the stated
characteristic-zero hypotheses. The general identification of the fixed
unpointed Jacobian with the fppf Picard sheaf, its full base-change
compatibility, and the positive-genus Abel–Jacobi closed-immersion theorem
are not yet provided as a complete API. The torsion-pairing constructions
do not establish a principal-polarization isomorphism with the dual abelian variety.

## Build

Lean and Mathlib versions are pinned in [`lean-toolchain`](lean-toolchain)
and [`lakefile.toml`](lakefile.toml).

```console
lake exe cache get
lake --wfail build
```

[CI](.github/workflows/lean.yml) checks imports and builds the libraries
with warnings treated as errors.

## Authorship and AI assistance

[Naganori Yamaguchi](https://github.com/n-yamaguchi-0729) is the author of the
local additions and the responsible maintainer. OpenAI Codex assisted with
Lean development, statement review, and preparation of this repository.

## License and acknowledgements

Apache License 2.0. See [LICENSE](LICENSE) and [NOTICE](NOTICE) for preserved
third-party notices and attribution.

Of the 2,937 Lean modules in AlgebraicCurves and JacobianVarieties, 2,404
carry individual links to 2,379 distinct files in
[Anthropic FLT at `aa2d8b3`](https://github.com/anthropics/fermats-last-theorem/tree/aa2d8b34692b16c70f699536de0d8e75b9a3e9ef).
Additional modules have FLT provenance without a resolved file correspondence;
these are source-header counts, not a measure of unchanged code.
The adaptation reorganizes modules by mathematical subject, revises imports,
proofs, and instance handling for the pinned Mathlib, and adds unpointed descent,
Jacobian group and dimension results, and Abel–Jacobi and étale comparisons.
Original Anthropic, PBC and other upstream attributions are preserved.

The Jacobian challenge statements come from
[Christian Merten's LeanEval contribution](https://github.com/leanprover/lean-eval/blob/38361be884302c7edce1908c4aef8296eb2cb27e/LeanEval/AlgebraicGeometry/JacobianChallenge.lean);
the constructions and proofs are supplied here. Supporting code also incorporates
[chrisflav/pi1](https://github.com/chrisflav/pi1); attribution and revision
information are preserved in [NOTICE](NOTICE) and source headers.
