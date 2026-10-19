# Generalized Jacobians (Lean 4.35.0-rc2)

[![Lean](https://github.com/n-yamaguchi-0729/JacobianVarieties/actions/workflows/lean.yml/badge.svg)](https://github.com/n-yamaguchi-0729/JacobianVarieties/actions/workflows/lean.yml)
[![License](https://img.shields.io/badge/license-Apache--2.0-blue.svg)](LICENSE)

This Lean component of [NYLL](https://github.com/n-yamaguchi-0729/NYLL) develops generalized Jacobians of algebraic
curves, with ordinary Jacobians as the zero-modulus case. It includes
Abel–Jacobi maps, unique factorization through generalized Jacobians,
torsion and cohomological comparisons. The [mathematical guide](https://n-yamaguchi-0729.github.io/NYLL/libraries/jacobian-varieties/) explains the main results
and their hypotheses.

The curve and Jacobian development builds substantially on
[Anthropic’s Fermat’s Last Theorem project](https://github.com/anthropics/fermats-last-theorem),
whose code is adapted and extended here. The original source credits
are retained in the files and in [NOTICE](NOTICE).

## Main results

<!-- nyll-publication:jacobian-main-results:begin -->
| Result | Mathematical content |
| --- | --- |
| [Zero modulus](Nyll/JacobianVarieties/Generalized/EmptyOrdinaryComparison.lean#L34) and [Picard-zero sheaf](Nyll/JacobianVarieties/Generalized/ExistingJacobianSheafRepresentation.lean#L86) | For a smooth proper geometrically integral curve over an arbitrary field, the zero-modulus generalized Jacobian is the ordinary Jacobian representing the fppf Picard-zero sheaf. The generalized construction commutes with [field extensions](Nyll/JacobianVarieties/Generalized/ScalarExtensionGroupScheme.lean#L67). |
| [Modulus existence and factorization](Nyll/JacobianVarieties/Generalized/GeneralGroupPicardOneFactorization.lean#L35) and its [pointed form](Nyll/JacobianVarieties/Generalized/GeneralGroupPointedFactorization.lean#L30) | A morphism from a nonempty open of such a curve to a separated commutative group scheme locally of finite type admits a modulus and a unique equivariant factor through the Picard degree-one torsor for that modulus. The pointed form assumes a rational point in the open sent to the identity, and gives a group factor. |
| [Least conductor](Nyll/JacobianVarieties/Generalized/GeneralGroupLocalSymbolExistence.lean#L70) and [product formula](Nyll/JacobianVarieties/Generalized/GeneralGroupProductConductor.lean#L103) | Over an algebraically closed field in any characteristic, a least full conductor classifies the moduli of the point-value function extended by the identity outside the open. The conductor of a product is the coefficientwise maximum. |
| [Tate-module comparison](Nyll/JacobianVarieties/Kummer/AbelJacobiTate.lean) | Over an algebraically closed field of characteristic zero, a Picard representation and Abel–Jacobi morphism compatible with point line bundles induce a topological isomorphism π₁(C)ᵃᵇ ≃ T(J). |
<!-- nyll-publication:jacobian-main-results:end -->

## Incomplete results

<!-- nyll-publication:jacobian-remaining:begin -->
The positive-genus Abel–Jacobi closed immersion and the principal polarization
are not yet formalized. Lie/differential and Witt comparisons, symmetric powers,
analytic uniformization and pinching comparisons also remain open.
<!-- nyll-publication:jacobian-remaining:end -->

## Public API

Use [leanprover/lean4:v4.35.0-rc2](lean-toolchain) and the Mathlib revision pinned in
[lake-manifest.json](lake-manifest.json). Add this dependency to your project’s
lakefile.toml:

```toml
[[require]]
name = "Nyll"
git = "https://github.com/n-yamaguchi-0729/JacobianVarieties.git"
rev = "main"
```

Run `lake update Nyll`, then commit your generated lake-manifest.json to retain
the resolved revisions. Import the library in a Lean file:

```lean
import Nyll.JacobianVarieties.All
```

The standalone distributions share the package name Nyll. Choose one distribution;
for several libraries together, use [NYLL](https://github.com/n-yamaguchi-0729/NYLL).

To build a checkout of this repository, run from its root:

```sh
lake exe cache get
lake --wfail build
```

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
