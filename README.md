# JacobianVarieties

[![Lean](https://github.com/n-yamaguchi-0729/JacobianVarieties/actions/workflows/lean.yml/badge.svg)](https://github.com/n-yamaguchi-0729/JacobianVarieties/actions/workflows/lean.yml)
[![License](https://img.shields.io/badge/license-Apache--2.0-blue.svg)](LICENSE)

Lean 4 と mathlib を用いた代数曲線・Picard 群・Jacobian の形式化ライブラリです。`AlgebraicCurves`（AC）と `JacobianVarieties`（JV）の全ソース、およびそれらが依存するローカルライブラリを収録しています。関数体・因子・微分・コホモロジーから、任意の体上の滑らかな固有曲線の Jacobian、Abel–Jacobi 射、Albanese 普遍性までを扱います。

このライブラリは **Anthropic の [Fermat’s Last Theorem in Lean 4](https://github.com/anthropics/fermats-last-theorem) から取り込んだ大量のソースを基礎とし、配置・依存関係・証明・API を修正して発展させたもの**です。元の成果を本プロジェクト独自の成果として扱いません。取り込み量、具体的な変更、権利表示は後述します。

以下は 2026-10-11 のソースに基づく案内です。定理の一般性はリンク先の Lean 宣言を基準にしてください。ソース上の実装、静的検査、通常ビルドの成功は区別して記録します。

## 収録範囲と入口

```lean
import AlgebraicCurves.All
import JacobianVarieties.All
```

| ライブラリ | 主な内容 |
|---|---|
| [AlgebraicCurves](Lean4/AlgebraicCurves/All.lean) | 関数体、付値と place、因子と Picard 群、微分と留数、Riemann–Roch、曲線モデル、相対因子、Hilbert 構成、曲線に必要なスキーム論 |
| [JacobianVarieties](Lean4/JacobianVarieties/All.lean) | Picard 表現、基点を要しない Jacobian、Abel–Jacobi 射と Albanese 普遍性、Pic⁰ の捩れ、Weil pairing、Kummer 被覆と有限係数 H¹ |
| ProCGroups / ValuedFieldTheory / GaloisCohomology / SchemeTheory | profinite 群、付値体、Galois コホモロジー、スキーム・層・降下の基礎 |
| ClassFieldTheory / EtaleFundamentalGroups / EtaleCohomology | 推移的な依存先を含む類体論、有限エタール被覆と基本群、エタールコホモロジー |

ローカル依存先もこのリポジトリ内に収録します。mathlib とその Lake 依存先は、固定された設定に従って取得します。取り込み範囲は AC/JV の一部抜粋ではありません。

## 主な構成と定理

### 曲線・因子・種数

[Divisors/ClassGroup.lean](Lean4/AlgebraicCurves/Divisors/ClassGroup.lean) は、関数体の place、因子、主因子、Picard 群、次数零部分 `Pic0` の基礎を与えます。曲線の関数体側とスキーム側の対応を [Models](Lean4/AlgebraicCurves/Models/All.lean) で扱い、[Differentials](Lean4/AlgebraicCurves/Differentials/All.lean) と [RiemannRoch](Lean4/AlgebraicCurves/Cohomology/RiemannRoch/All.lean) に微分・留数・Riemann–Roch の証明群を置いています。これらの低水準定理には、局所留数や関数体拡大など、宣言に明記された入力があります。

[GeometricGenus.lean](Lean4/AlgebraicCurves/Geometry/Curves/GeometricGenus.lean) では、任意の基礎体について代数閉包上の関数体から `geometricGenus` を定義し、滑らかで固有かつ幾何的整な曲線について

\[
\dim_K H^1(C,\mathcal O_C)=g(C)
\]

を `finrank_H1_eq_geometricGenus` として示します。ここで実装に現れる H¹ は、元の曲線の二つのアフィン開集合による Čech コホモロジーです。種数の定義に有理基点は不要です。

### 任意の体上の Jacobian と Abel–Jacobi 射

一般性を確認しやすい入口は [JacobianChallenge.lean](Lean4/JacobianVarieties/JacobianChallenge.lean) です。任意の体 `k` 上の固有・滑らか・幾何的既約な相対次元 1 の曲線 `C` に対し、次の宣言を実装しています。

| 宣言 | 数学的内容 |
|---|---|
| `genus`, `Jacobian` | 種数と元の体上の Jacobian。Jacobian の定義には有理点を要求しない |
| `Jacobian.instGrpObj` | 構成した同じ Jacobian 上の群対象構造 |
| `Jacobian.smoothOfRelativeDimension_genus` | Jacobian は種数と等しい相対次元をもつ滑らかなスキーム |
| `Jacobian.instIsProper`, `Jacobian.instGeometricallyIrreducible` | 固有性と幾何的既約性 |
| `Jacobian.ofCurve` | 有理基点 `P` を指定した Abel–Jacobi 射 |
| `Jacobian.comp_ofCurve` | `P` の像は群の単位元 |
| `Jacobian.exists_unique_ofCurve_comp` | 滑らか・固有・幾何的既約な群対象への基点を保つ射は、この Abel–Jacobi 射を通じて一意に分解する |

これらの入力に「Jacobian が存在する」という仮定を追加してはいません。[Existence.lean](Lean4/JacobianVarieties/Picard/Existence.lean) が基点を持つ Picard 表現を構成し、[UnpointedExistence.lean](Lean4/JacobianVarieties/Picard/UnpointedExistence.lean) は閉点から得た有限体拡大上での構成を忠実平坦降下により元の体へ戻します。[UnpointedJacobian.lean](Lean4/JacobianVarieties/Picard/UnpointedJacobian.lean) はその一つの降下データを固定し、同じスキーム上に可換群法則・次元・幾何的整性を与えます。[UnpointedAlbanese.lean](Lean4/JacobianVarieties/Picard/UnpointedAlbanese.lean) が一意分解を供給します。

`JacobianChallenge` の定理文は Christian Merten による [LeanEval の Jacobian challenge](https://github.com/leanprover/lean-eval/blob/38361be884302c7edce1908c4aef8296eb2cb27e/LeanEval/AlgebraicGeometry/JacobianChallenge.lean) に由来します。その著作権・著者表示をソースに保持し、本ライブラリで構成と証明を実装しています。ここでの一意分解の結論は `Over (Spec k)` の射についてのものです。

### 種数 1、Weil pairing、エタール H¹

| 結果 | 範囲と参照 |
|---|---|
| 種数 1 の Abel–Jacobi 射は同型 | 代数閉体・標数 0 上の基点付き曲線モデルについて、`exists_abelJacobi_isIso_of_genus_one` が Picard 表現と実際の同型を構成。[GenusOneAbelJacobiIsomorphism.lean](Lean4/JacobianVarieties/Albanese/GenusOneAbelJacobiIsomorphism.lean) |
| コホモロジーで種数 1 と判定した曲線の Albanese | 同じ標数・基礎体の範囲で、実際の H¹ の次元が 1 である仮定から同型と、群準同型としての一意分解を構成。[GenusOneAlbanese.lean](Lean4/JacobianVarieties/Albanese/GenusOneAlbanese.lean) |
| 因子による Weil pairing の非退化性 | 代数閉体・標数 0、有限生成の 1 変数関数体の条件下で、与えられた `DivisorialWeilPairingData` の `toHom` が単射。可除性・非退化性そのものを追加仮定にせず供給する。[CharZeroNondegeneracy.lean](Lean4/JacobianVarieties/WeilPairing/CharZeroNondegeneracy.lean) |
| 捩れ点の個数 | 代数閉体・標数 0 上の次元 `d` の滑らかな固有可換群について、正整数 `n` に対し `#A[n] = n^(2d)`。[TorsionCardinality.lean](Lean4/JacobianVarieties/Picard/TorsionCardinality.lean) |
| 固有曲線の有限係数 H¹ | 代数閉体・標数 0 上の基点付き曲線モデルについて、実際の小エタールサイトの層コホモロジーに `#H¹(C, μₙ) = n^(2g)`。`AlgebraicCurve.CurveModel.natCard_finiteH1_eq_pow_genus` は必要な Picard 表現と Abel–Jacobi 射を内部で構成。[FiniteH1Cardinality.lean](Lean4/JacobianVarieties/Kummer/FiniteH1Cardinality.lean) |
| 開曲線の有限係数 H¹ | 同じ標数・基礎体の範囲で、有限かつ空でない境界 `r` を持つ準コンパクトな稠密開曲線について `#H¹(U, μₙ) = n^(2g+r-1)`。[OpenFiniteH1Cardinality.lean](Lean4/JacobianVarieties/Kummer/OpenFiniteH1Cardinality.lean) |
| Abel–Jacobi と Tate module | 代数閉体・標数 0 で、Picard 表現・Abel–Jacobi 射・点の線束分類との整合性を明示した入力から、実際の基本群準同型が誘導する `π₁(C)ᵃᵇ ≃ T(J)` を構成。[AbelJacobiTate.lean](Lean4/JacobianVarieties/Kummer/AbelJacobiTate.lean) |

## 現在の範囲と開発中の部分

上記の任意の体上の Jacobian 構成と、標数 0 に限定された後半の比較定理を区別してください。標数 0 の Kummer・捩れ・種数 1 の結果を、そのまま任意標数での結果とは述べていません。

`RepresentsRelSubPic`、相対 Hilbert データ、Weil pairing のデータを受け取る補助定理もあります。これらを利用する際は、その入力を供給する存在定理と仮定を確認する必要があります。基点付き Picard 表現やその基底変換の API は存在しますが、固定した基点なし Jacobian と fppf Picard 層の同定、任意の体拡大に関する標準的同型と全整合性、正の種数での Abel–Jacobi の閉埋め込みなどをまとめた完成済みの一般 API としては案内していません。

また、[Autoduality/Cohomology.lean](Lean4/JacobianVarieties/Autoduality/Cohomology.lean) の `H1Gm` は因子類群 `Pic` をモデルとして定義され、`autodualityEquiv` は供給された完全 pairing から得られる捩れ群の同型です。このファイル名だけを根拠に、Jacobian と双対アーベル多様体の主偏極によるスキーム同型が無条件に完成したとは主張しません。実際の導来層コホモロジーとの比較は上記の `Kummer` 群の別の構成です。

移設・整理の背景と追加の完成目標は [LibraryScope.md](Lean4/AlgebraicCurves/LibraryScope.md) に残しています。この文書には過去の整理計画も含まれるため、実装の現在地は本 README とリンク先の宣言を優先してください。

## Anthropic FLT からの取り込み量と変更

取り込み・比較の基準は、[anthropics/fermats-last-theorem の `aa2d8b34692b16c70f699536de0d8e75b9a3e9ef`](https://github.com/anthropics/fermats-last-theorem/tree/aa2d8b34692b16c70f699536de0d8e75b9a3e9ef) です。2026-10-11 の AC/JV の全 `.lean` ファイルの先頭ヘッダを数えた結果は次のとおりです。依存先ライブラリのファイルはこの表に含めません。

| ヘッダによる出典分類 | AlgebraicCurves | JacobianVarieties | 合計 |
|---|---:|---:|---:|
| 全 Lean ファイル | 2,452 | 485 | 2,937 |
| 改変元の Anthropic FLT ファイルへの個別リンクあり | 2,056 | 348 | 2,404 |
| FLT 由来の本文が認められるが個別の元ファイル対応は未解決 | 214 | 29 | 243 |
| FLT を基礎とするが当該モジュールの直接一致は未確定 | 142 | 10 | 152 |
| その他のヘッダ（独自追加・別出典等） | 40 | 98 | 138 |

個別リンクが指す上流の異なるファイルは AC で 2,046、JV で 340、両者の重複を除いて **2,379 ファイル**です。したがって「数個の定義を参考にした」という規模ではありません。一方、この集計は既存の出典ヘッダの件数であり、全文一致の件数、コピーされた行の割合、上流 FLT 全体に対する取り込み率ではありません。元ファイルの対応が未解決の部分を独自作成と再分類してはいません。

主な適応・変更は次のとおりです。

- FLT の `Definitions/`、`P2M/Sol/` 等に分かれていた曲線・Picard 関連のソースを、AC/JV の数学的主題別モジュールへ移設。集約ファイルと直接 import を整え、上流・下流の責任を分けています。
- 移植した本文に、現行 mathlib、型・instance、局所的な API に合わせた修正があります。ファイルヘッダの `Adapted` 表示は、そのソースが上流の無変更コピーであることを意味しません。
- 例えば、[上流の有限写像データからの相対 Jacobian 存在証明](https://github.com/anthropics/fermats-last-theorem/blob/aa2d8b34692b16c70f699536de0d8e75b9a3e9ef/P2M/Sol/S_AlgebraicGeometry_exists_relJacobian_of_smoothOfRelativeDimension_one_of_finiteMapData.lean) を基礎とする [Existence.lean](Lean4/JacobianVarieties/Picard/Existence.lean) は、局所環での有限エタール降下と、任意の体上で曲線と基点から必要な有限写像データを構成する入口をまとめています。上流の `import Mathlib` や `Definitions`/`Theorems` の参照は、具体的なローカルモジュールへ改めています。
- 基点なしの降下、同じ Jacobian 上の群法則と種数次元、Abel–Jacobi と Albanese の比較、種数 1 と有限エタール H¹・Tate module の比較などを、その供給元を明示した追加構成として発展させています。

上流 FLT がさらに利用している [Imperial College London の FLT](https://github.com/ImperialCollegeLondon/FLT)、[flt-regular](https://github.com/leanprover-community/flt-regular)、[mathlib](https://github.com/leanprover-community/mathlib4) の帰属情報も維持します。これらのプロジェクト全体や FLT の最終定理を、この曲線・Jacobian リポジトリへすべて取り込んだという意味ではありません。

## 著作権と出典

ライセンスは [Apache License 2.0](LICENSE) です。外部由来の部分には元の権利者・著者・出典が適用されます。

- Anthropic FLT の権利表示は `Copyright 2026 Anthropic, PBC`。保存した [NOTICE](Lean4/AlgebraicCurves/ANTHROPIC_FLT_NOTICE)、[ATTRIBUTION](Lean4/AlgebraicCurves/ANTHROPIC_FLT_ATTRIBUTION.md)、[出典説明](Lean4/AlgebraicCurves/ANTHROPIC_FLT_SOURCE.md) を参照してください。JV 内にも [NOTICE](Lean4/JacobianVarieties/ANTHROPIC_FLT_NOTICE)・[ATTRIBUTION](Lean4/JacobianVarieties/ANTHROPIC_FLT_ATTRIBUTION.md) を保持しています。
- これらの保存した NOTICE / ATTRIBUTION に現れる “this repository” や 106 ファイル等の件数は **上流 Anthropic リポジトリについての記述**です。今回の AC/JV の取り込み量は上の表です。
- `JacobianChallenge.lean` の定理文の出典は Christian Merten の LeanEval です。元の Apache 2.0 表示と著者を保持しています。
- 独自作成部分の権利者・保守担当は [Naganori Yamaguchi](https://github.com/n-yamaguchi-0729)。独自部分の著者表示は `Naganori Yamaguchi (assisted by OpenAI Codex)` です。外部由来コードの著者をこの表示で置き換えてはいません。

同梱依存先にも個別の由来があります。特に `EtaleFundamentalGroups` と `SchemeTheory` は [chrisflav/pi1](https://github.com/chrisflav/pi1) 由来のソースを含みます。比較に用いた revision は [`5c5c0171efc863ce9990171bdf46d7cb5fd582fb`](https://github.com/chrisflav/pi1/tree/5c5c0171efc863ce9990171bdf46d7cb5fd582fb) で、厳密なコピー元 commit が未確認であることは各ヘッダに明記されています。権利者が未確定のファイルを推測で帰属させず、既存の出典表示を保持しています。

## 依存関係を含む配布範囲

各行のライブラリの **全 maintained source** を収録しています。主ライブラリだけでなく、ローカル import の依存先をライブラリ単位で再帰的に含めた独立パッケージです。別の非公開リポジトリを clone する必要はありません。外部依存の mathlib とその依存は Lake が固定 revision から取得します。

| 深さ | ライブラリ | Lean ファイル数 |
|---:|---|---:|
| 1 | [ProCGroups](Lean4/ProCGroups/All.lean) | 724 |
| 1 | [ValuedFieldTheory](Lean4/ValuedFieldTheory/All.lean) | 591 |
| 1 | [GaloisCohomology](Lean4/GaloisCohomology/All.lean) | 309 |
| 1 | [SchemeTheory](Lean4/SchemeTheory/All.lean) | 975 |
| 2 | [ClassFieldTheory](Lean4/ClassFieldTheory/All.lean) | 1,246 |
| 2 | [AlgebraicCurves](Lean4/AlgebraicCurves/All.lean) | 2,452 |
| 2 | [EtaleFundamentalGroups](Lean4/EtaleFundamentalGroups/All.lean) | 1,608 |
| 2 | [EtaleCohomology](Lean4/EtaleCohomology/All.lean) | 125 |
| 3 | [JacobianVarieties](Lean4/JacobianVarieties/All.lean) | 485 |

パッケージ全体の入口は [`All`](Lean4/All.lean) です。各ライブラリの元のモジュール名を保持しているため、重複するソースを含む他の分割パッケージと同じ Lake 環境へ重ねて導入することは想定していません。

## ビルドと CI

Lean は [`lean-toolchain`](lean-toolchain) の **4.35.0-rc2**、mathlib は [`lakefile.toml`](lakefile.toml) の **065356127b1dc0016f66b7283ce0ce2c4055aa55** に固定しています。再現時は同梱の [`lake-manifest.json`](lake-manifest.json) を使います。

Git と [elan](https://github.com/leanprover/elan) を用意して実行します。

```sh
git clone https://github.com/n-yamaguchi-0729/JacobianVarieties.git
cd JacobianVarieties
lake exe cache get Mathlib
python3 .github/scripts/audit.py
lake --wfail build +ProCGroups.All +ValuedFieldTheory.All +GaloisCohomology.All +SchemeTheory.All
lake --wfail build +ClassFieldTheory.All +AlgebraicCurves.All +EtaleFundamentalGroups.All +EtaleCohomology.All
lake --wfail build +JacobianVarieties.All
lake --wfail build +All
```

[`CI`](.github/workflows/lean.yml) は公開済みの [ProCGroups](https://github.com/n-yamaguchi-0729/ProCGroups)・[ClassFieldTheory](https://github.com/n-yamaguchi-0729/ClassFieldTheory)・[SawinTotallyRealTowers](https://github.com/n-yamaguchi-0729/SawinTotallyRealTowers) の版固定・依存深さ順の検証・成果物キャッシュの構成を参考にしています。今回の CI は次を行います。

- 全物理ソースと Lake glob、入口からの到達、import の欠落・循環・逆依存を静的検査。
- 深さ1、2、3のライブラリを順に通常ビルドし、最後に `+All` をビルド。
- `autoImplicit = false`、未使用引数、文書化、mathlib 標準 linter を Lake 設定で有効化。
- 段階間でプロジェクトのコンパイル成果物を再利用し、失敗時も診断ログを保存。

`lake --wfail build` を使い、警告・エラーを失敗にします。この配布範囲には AnabelianGeometry の未証明定理を含みません。新たな `sorry` / `admit` / 独自 `axiom` は静的検査でも拒否します。静的検査は証明項の独立検証ではありません。この CI では NanoDa や別途の全宣言 kernel replay を実行したとは主張しません。

**private の push / pull request ではビルドジョブを開始せず、Actions の手動実行は可能**です。public に切り替えた後は push / pull request で動作します。初回配布では private のまま作成し、公開設定の変更も CI の手動起動も行っていません。

初回の検証結果と検証範囲は [VALIDATION.md](VALIDATION.md) に記録しています。

## ソースの来歴

分割元、固定コミット、同梱ファイルの SHA-256 は [SOURCE_SNAPSHOT.json](SOURCE_SNAPSHOT.json) にあります。Lean 本文・既存の著作権表示は分割元から変更せず、配布用 `Lean4/All.lean`、Lake の対象範囲、README と CI を作成しました。ライブラリ内の既存計画には古い配置・当時の停止点が残る場合があります。

[第三者由来のコードと帰属](THIRD_PARTY.md) および [pi1 の25ファイル対応表](PI1_ATTRIBUTION.md) も参照してください。
