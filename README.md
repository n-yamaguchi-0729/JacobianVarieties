# JacobianVarieties (Lean 4.35.0-rc2)

[![Lean](https://github.com/n-yamaguchi-0729/JacobianVarieties/actions/workflows/lean.yml/badge.svg)](https://github.com/n-yamaguchi-0729/JacobianVarieties/actions/workflows/lean.yml)
[![License](https://img.shields.io/badge/license-Apache--2.0-blue.svg)](LICENSE)

This Lean component of [NYLL](https://github.com/n-yamaguchi-0729/NYLL) develops Jacobians and generalized Jacobians
of algebraic curves, including Abel–Jacobi maps, factorization properties,
torsion and cohomological comparisons. The [mathematical guide](https://n-yamaguchi-0729.github.io/NYLL/libraries/jacobian-varieties/) explains the principal
constructions and results, including their base-field and characteristic hypotheses.

The curve and Jacobian development builds substantially on
[Anthropic’s Fermat’s Last Theorem project](https://github.com/anthropics/fermats-last-theorem),
whose code is adapted and extended here. The original source credits
are retained in the files and in [NOTICE](NOTICE).

## Scope

The Jacobian construction works over arbitrary fields; the torsion,
genus-one, and étale comparison results below have the stated
characteristic-zero hypotheses.

## Main results

### Jacobians and curves

For a smooth proper geometrically irreducible curve `C` over any field `k`,
[`JacobianChallenge`](Nyll/JacobianVarieties/JacobianChallenge.lean) constructs
`Jacobian C` over `k` without requiring a rational point. It equips the
Jacobian with a group structure and proves smoothness, properness, geometric
irreducibility, and dimension equal to the geometric genus.

| Result | Mathematical content |
| --- | --- |
| [Genus and coherent cohomology](Nyll/AlgebraicCurves/Geometry/Curves/GeometricGenus.lean) | For a smooth proper geometrically integral curve over any field, the dimension of structure-sheaf Čech H¹ for a two-affine-open cover equals its geometric genus. |
| [Abel–Jacobi morphism](Nyll/JacobianVarieties/JacobianChallenge.lean) | A rational basepoint defines an Abel–Jacobi morphism to the constructed Jacobian, sending the basepoint to the identity. |
| [Albanese factorization](Nyll/JacobianVarieties/JacobianChallenge.lean) | Every basepoint-preserving morphism from the curve to a smooth proper geometrically irreducible group scheme factors uniquely through Abel–Jacobi as a morphism over `k`. |
| [Genus-one Abel–Jacobi isomorphism](Nyll/JacobianVarieties/Albanese/GenusOneAbelJacobiIsomorphism.lean) | Over an algebraically closed field of characteristic zero, a pointed genus-one curve model has an Abel–Jacobi isomorphism with its Picard variety. |
| [Torsion cardinality](Nyll/JacobianVarieties/Picard/Torsion/TorsionCardinality.lean) | For an abelian variety of dimension `d` over an algebraically closed field of characteristic zero and positive `n`, the multiplication kernel has `n^(2*d)` algebraic points. |
| [Étale cohomology of proper curves](Nyll/JacobianVarieties/Kummer/FiniteH1Cardinality.lean) | For a pointed geometrically integral curve model over an algebraically closed field of characteristic zero, actual étale H¹ with μₙ coefficients has cardinality `n^(2*g)`, for positive `n`. |
| [Étale cohomology of open curves](Nyll/JacobianVarieties/Kummer/OpenFiniteH1Cardinality.lean) | In the same characteristic-zero setting, a quasi-compact dense integral open curve with finite nonempty boundary of size `r` has `#H¹(U, μₙ) = n^(2*g+r-1)`. |
| [Tate-module comparison](Nyll/JacobianVarieties/Kummer/AbelJacobiTate.lean) | Over an algebraically closed field of characteristic zero, a Picard representation and Abel–Jacobi morphism compatible with point line bundles induce a topological isomorphism `π₁(C)ᵃᵇ ≃ T(J)`. |

The [divisorial Weil pairing](Nyll/JacobianVarieties/WeilPairing/CharZeroNondegeneracy.lean)
is also proved nondegenerate for supplied `DivisorialWeilPairingData` over an
algebraically closed characteristic-zero field, under the principal-divisor
and one-variable function-field hypotheses.

### Generalized Jacobians

<!-- nyll-publication:generalized-jacobians:begin -->
For a smooth proper geometrically integral curve over an arbitrary field,
a morphism from a nonempty open subset to a separated commutative group
scheme locally of finite type constructs a modulus with its multiplicities.
The unpointed factorization does not require a rational point, a perfect
base field, or a smooth connected target.

| Result | Mathematical content |
| --- | --- |
| [Existence of a full modulus](Nyll/JacobianVarieties/Generalized/GeneralGeometricModulusExistence.lean#L59) | A full ground-field modulus for the original open complement. |
| [Unpointed factorization](Nyll/JacobianVarieties/Generalized/GeneralGroupPicardOneFactorization.lean#L35) | A unique equivariant factor through the intrinsic Picard degree-one torsor. |
| [Pointed factorization](Nyll/JacobianVarieties/Generalized/GeneralGroupPointedFactorization.lean#L30) | A unique pointed group factor. |
| [Least conductor](Nyll/JacobianVarieties/Generalized/GeneralGroupLocalSymbolExistence.lean#L70) | Least full conductor classifying all moduli over an algebraically closed field. |
| [Product conductor](Nyll/JacobianVarieties/Generalized/GeneralGroupProductConductor.lean#L103) | The product conductor is the coefficientwise maximum of the two conductors. |

The pointed form assumes a rational point in the original open sent to the identity. Uniqueness is for the factor attached to the constructed modulus.
Over an algebraically closed field in any characteristic, the conductor classifies full moduli of the actual point-value function, extended by the identity outside the open. Its support can be smaller than the original boundary. The map also constructs its reciprocal local symbol: [`exists_group_regularMap_localSymbol`](Nyll/JacobianVarieties/Generalized/GeneralGroupLocalSymbolExistence.lean#L37).
Lie and differential comparisons, the actual nonreduced kernel and its
Witt decomposition, symmetric powers, analytic uniformization, and
pinching comparisons remain unfinished.
<!-- nyll-publication:generalized-jacobians:end -->

## Incomplete results

The general identification of the fixed unpointed Jacobian with the fppf
Picard sheaf, its full base-change
compatibility, and the positive-genus Abel–Jacobi closed-immersion theorem
are not yet provided as a complete API. The torsion-pairing constructions
do not establish a principal-polarization isomorphism with the dual abelian variety.

## Included sources

<!-- nyll-publication:distribution:begin -->
Only the module-level import closure of `Nyll.JacobianVarieties.All` is bundled. Dependencies retain their canonical `Nyll` imports.

Supporting modules are included from [AbelianVarieties](Nyll/AbelianVarieties/), [AlgebraicCurves](Nyll/AlgebraicCurves/), [AnabelianGeometry](Nyll/AnabelianGeometry/), [ClassFieldTheory](Nyll/ClassFieldTheory/), [EtaleCohomology](Nyll/EtaleCohomology/), [EtaleFundamentalGroups](Nyll/EtaleFundamentalGroups/), [GaloisCohomology](Nyll/GaloisCohomology/), [ProCGroups](Nyll/ProCGroups/), [SchemeTheory](Nyll/SchemeTheory/), [ValuedFieldTheory](Nyll/ValuedFieldTheory/). These dependency libraries are included only to the extent required by the imports.
<!-- nyll-publication:distribution:end -->

## Public API

Import the Jacobian library and its required curve modules with:

```lean
import Nyll.JacobianVarieties.All
```

For the Jacobian construction and its principal properties:

```lean
import Nyll.JacobianVarieties.JacobianChallenge
```

## Build

The toolchain and dependencies are pinned in [`lean-toolchain`](lean-toolchain)
and [`lakefile.toml`](lakefile.toml).

```console
lake exe cache get
lake --wfail build
```

[CI](.github/workflows/lean.yml) checks imports and builds the libraries
with warnings treated as errors.

## Authorship

All original code in this library is written by OpenAI Codex.

[Naganori Yamaguchi](https://github.com/n-yamaguchi-0729) sets the mathematical
goals and scope, directs development, and maintains the library.

## License and acknowledgements

<!-- nyll-publication:credits:begin -->
Apache License 2.0. See [LICENSE](LICENSE).

Included adaptations from [pi1](https://github.com/chrisflav/pi1) and [Anthropic’s Fermat’s Last Theorem project](https://github.com/anthropics/fermats-last-theorem) retain their source-specific copyrights and authors.
The included-file inventories and preserved upstream notices are in [NOTICE](NOTICE).
<!-- nyll-publication:credits:end -->
