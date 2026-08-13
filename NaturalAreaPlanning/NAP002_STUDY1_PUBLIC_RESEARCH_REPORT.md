# Natural Area Planning / NAP-002 Study 1 — Public Research Report

**公開情報だけで阿蘇半自然草原の管理判断をどこまで支援できるか**  
**― 「何をすべきか」を断定せず、「何が判断を止めているか」を明示する ―**

- レポート版: **v1.0.1**
- 作成日: **2026-08-13 JST**
- 研究プロジェクト: **Natural Area Planning / NAP-002 Study 1**
- 研究状態: **Study 1 complete / frozen — public-only evidence-constrained decision support**
- 研究状態スナップショット: `nkkmd/natural-area-planning` / commit `93a6ab91c0dbcbac2dfa32c4ff11700f745770fc`
- 本文の性格: **単体公開用研究レポート**
- 編集改訂: **v1.0.1 — citation metadata only; scientific findings unchanged**

> この文書は、元のGitHubリポジトリ、内部の研究ログ、checkpoint、CSV、コードを参照しなくても、NAP-002 Study 1の背景、問い、方法、主要結果、限界、再現性、実務上の意味、今後の研究課題を理解できるように構成している。GitHub外へこのファイル単体をコピーして公開しても、研究レポートとして成立することを意図している。

---

## 要旨

半自然草原の管理では、「どこを保全すべきか」だけでなく、「どの管理を、どこで、どの条件で行うべきか」が実務上の問題になる。しかし、その問いに答えるには、対象地の状態だけでなく、保全対象の存在、管理による応答、他地点からのresponse transfer、悪影響・競合のsafeguard、実行可能性、資源、当日の条件など、性質の異なる情報が必要である。

Natural Area Planning / NAP-001は、熊本県阿蘇地域の半自然草原を対象として、公開情報だけからmanagement-specific systematic conservation planningをどこまで構築できるかを検討した。その結果、193のplanning units、管理regime・保全featureの構造、公開植生base state、source-localな管理応答、planning softwareへの構造的写像までは構築できた一方、planning-unit単位の管理応答へsource evidenceを転移するための証拠が不足し、直接利用可能なplanning response（Q3）= 0、local management × context model（Q4）= 0、formal optimizer = **NOT AUTHORIZED**としてpublic-only Stage Cを閉じた。

NAP-002 Study 1は、このnegative/non-ready resultを救済するために係数を補う研究ではない。研究質問を一段上流へ変更した。

> **管理効果をplanning unitへ定量的に転移できない場合でも、証拠の限界を保持したまま、「どこを管理レビュー対象とするか」「何が候補行動の判断を止めているか」「次に何を確認する必要があるか」を再現可能な意思決定支援として表現できるか。**

Study 1では、意思決定をG1–G8の非代替的なgateへ分解した。G1はmanagement-review scope、G2–G5は保全対象別のtarget relevance・management-response evidence・response transferability・ecological safeguard/conflict、G6–G8はlocal operational feasibility・resource/capacity・current operational conditionsを表す。生態学的証拠を対象別に保持するため、machine-readable representationは `planning unit × candidate action × feature` のTier Eと、`planning unit × candidate action` のTier Aの二層とした。

結果として、G1では193 planning unitsのうち188（97.41%）が `PASS_PUBLIC`、1が `UNRESOLVED`、4が `NOT_APPLICABLE`となり、公開空間情報だけでも広いmanagement-review footprintを定義できた。一方、完全なdecision engineはTier E 22,113 records、Tier A 2,509 records、計24,622 recordsとなり、validation errorは0であったが、G4・G6・G7・G8の `PASS_PUBLIC` はすべて0だった。すべてのsource-supported management responseはG4でplanning-unit transferを禁止され、local feasibilityやcurrent conditionsも公開情報だけでは閉じなかった。

したがって、**public-onlyでmanagement-review scopeはかなり具体化できるが、どの管理をどのplanning unitへ割り当てるべきかは正当化できない**、というのがStudy 1の主要結果である。

本研究はこの状態を「情報不足」と一括しなかった。不足を、local target relevance、local safeguard、response-transfer validation、action specification、access、infrastructure、authority、resource capacity、current site/weather/fuel/safety/permission等へ分解し、最終的な実務表現を次の三部構造として閉じた。

```text
WHERE
  Management Review Scope Map

WHY
  Candidate Action × Decision-Gate Blocker Matrix

WHAT NEXT
  Candidate Action × Required-Input Checklist
```

さらに、完全にsyntheticな2例について、許可されたlocal/current factual inputsをすべて満たした場合を検証したが、野焼き単独と年2回刈取りの両方で `RESEARCH_EVIDENCE_REQUIRED` が残った。これは、現場情報の充足がmanagement-response evidenceやresponse transferabilityを自動的に代替しないことを示す。

本研究の中心的成果は、推奨管理地図ではない。**「公開情報でどこまで進めるか」と「どこから先はlocal facts・current facts・新しい科学的検証が必要か」を、unknownをzeroへ変換せずに機械追跡可能な形で分離したこと**である。

---

# 1. 研究の背景

## 1.1 半自然草原では「保護する」だけでは管理問題を解けない

半自然草原は、長期間にわたる野焼き、放牧、採草・刈取りなどの人為管理と結びついて成立・維持されてきた場合がある。そのような生態系では、利用や攪乱を止めることが必ずしも保全と同義ではない。管理停止によってリター蓄積、植生遷移、木本化等が進む可能性がある一方、管理を強めれば常に良いわけでもない。

管理効果は、少なくとも次の要素に依存する。

```text
management effect
 = f(
     management type,
     intensity,
     timing,
     conservation target,
     background management,
     ecological context
   )
```

したがって、実務的な問いは単純な

```text
管理する / 管理しない
```

ではない。

より適切な問いは、

> **どの保全対象に対して、どの管理を、どの強度・時期・条件で検討できるのか。**

である。

## 1.2 NAP-001が明らかにした「計算より前の不足」

前身研究NAP-001では、公開情報だけを用いて阿蘇半自然草原のmanagement-specific spatial planningを構築しようとした。

その研究では、

- 193 planning units
- operational management regimes
- feature-specific conservation architecture
- 13 candidate management regimes
- 11 conservation/supporting features
- 公開GIS・公開植生base state
- source-relative / source-local management-response evidence
- planning softwareへのstructural mapping

までを構築した。

しかしformal spatial allocationには、それだけでは足りない。

たとえば、ある論文で特定の放牧強度や刈取り時期に対する反応が観察されても、その効果を別のplanning unitへそのまま数値転移できるとは限らない。source siteの履歴、背景管理、処理定義、測定対象、立地条件等が異なるためである。

NAP-001はこの点を、次のように閉じた。

```text
Q3 planning-response features           = 0
Q4 local management × context models   = 0
coefficient-ready regimes              = 0
zone contribution rows                 = 0
formal optimizer authorized            = false
```

重要なのは、solverを動かせなかったのではなく、**solverへ渡す数値を科学的に正当化できなかった**ことである。

## 1.3 NAP-002の出発点

この結果には実務上の問題が残る。

研究者が

> 「ここから先は証拠が足りない」

と正しく結論しても、行政担当者や現場管理者は依然として、

```text
どこを検討対象にするのか
どの候補管理について何が分かっているのか
何が判断を止めているのか
次に何を確認すべきなのか
```

を知る必要がある。

NAP-002は、このgapを仮のmanagement effectやactionability scoreで埋めるのではなく、**gapそのものをdecision-support objectへ変換する**ことを目的とした。

---

# 2. 研究対象 — 阿蘇半自然草原

Study 1はNAP-001と同じ、熊本県阿蘇地域の凍結された193 planning unitsを対象とする。

阿蘇は、野焼き、放牧、採草などの人間活動と長期的に関係しながら維持されてきた半自然草原を含む。そのため、単純なland-cover mapだけではなく、管理行為と保全対象の関係を意思決定へ接続する必要がある。

NAP-002は、新しい生態学的response datasetを収集した研究ではない。NAP-001が凍結したpublic evidence、management vocabulary、feature vocabulary、transfer ceiling、公開空間状態を継承し、**それらを意思決定上のgateへ変換する研究**である。

---

# 3. 研究目的と中心的研究質問

NAP-002 Study 1の中心的研究質問は次のとおりである。

> **定量的な管理効果の空間転移や最適配分を科学的に正当化できない状況においても、既存証拠の限界と不確実性を保持したまま、実務者が「どこで、どの管理について、次に何を確認・判断すべきか」を再現可能に提示する意思決定支援を構築できるか。**

より形式的には、

> **NAP-001が凍結したpublic-evidence boundaryを壊さず、planning-unit ecological effectsやaction recommendationsを捏造することなく、management-review scope、decision blockers、required informationへ変換できるか。**

である。

Study 1のformal answerは、

```text
YES
```

として閉じられた。

ただし、このYESは

```text
管理推奨ができる
```

という意味ではない。

支持されたendpointは、

```text
scope
+
blocker
+
information need
```

である。

---

# 4. 先行研究との関係

## 4.1 decision support under uncertainty自体は新しくない

自然資源管理・保全分野では、Structured Decision Making（SDM）、adaptive management、Value of Information（VoI）、robust decision making、spatial conservation planning under uncertainty、multi-action prioritization、uncertainty visualization、map-based decision support等に広い先行研究がある。

したがってNAP-002は、次の一般概念を新規発明として主張しない。

```text
decisionを要素へ分解すること
uncertaintyを明示すること
information needを特定すること
mapでuncertaintyを示すこと
複数actionを比較する枠組み
monitoringをdecision needから設計すること
```

## 4.2 Value of Informationとの違い

VoIは、どの不確実性を解消することが意思決定価値を高めるかを評価する成熟した枠組みである。

しかしVoIを正式に計算するには、少なくともaction、outcome、objective、uncertaintyの関係が十分に規定されている必要がある。

NAP-001の中心結果は、planning-unit action-response layerそのものが十分に閉じていないことだった。その状態で、blockerへ重みを付けて「重要度」を計算すると、別のpseudo-precisionを導入する危険がある。

そのためStudy 1は、

```text
blockerを識別する
!=
blockerのValue of Informationを推定する
```

とした。

## 4.3 NAP-002が主張できる範囲

Study 1の現在のcontribution ceilingは、一般理論の発明ではなく、次のapplied evidence-translation architectureである。

```text
frozen evidence-readiness ceiling
+
anti-pseudo-precision rules
+
planning unit × candidate action × gate provenance
+
non-transferability retained as output
+
unknown / missing local facts retained as blockers
+
machine-readable ruleからpresentationを生成
```

この組合せを阿蘇のpublic-only evidence chainに適用し、実装・検証したことがStudy 1の位置づけである。

---

# 5. public-only研究設計

## 5.1 使用した情報

Study 1は、以下だけを研究入力として使用した。

- NAP-001で凍結済みの公開source/evidence architecture
- 公開GIS由来の193 planning-unit identity/geometry
- 公開植生情報から構築された凍結済みvegetation composition
- 公開植生情報から構築されたopen-grassland state
- NAP-001で整理済みの13 candidate management regimes
- NAP-001で整理済みのconservation/supporting features
- NAP-001のregime–feature evidence relations
- NAP-001のresponse-transfer audit
- synthetic conditional prototype用の架空入力

## 5.2 使用しなかった情報

Study 1は以下を使用していない。

- restricted/private ecological data
- 許可制・申請制の牧野カルテ等のlocal raw data
- non-public rare-species locations
- confidential producer data
- new field measurements
- real practitioner inputs
- real local operational feasibility values
- real current weather/safety/permission values

したがって、Study 1はpublic-only successor studyとして完結している。

## 5.3 NAP-001の歴史状態を変更しない

Study 1の重要なintegrity ruleは、NAP-001のnegative/non-ready resultを書き換えないことである。

最終監査では、

```text
analysis/nap001 files modified by NAP-002 = 0
```

であった。

また、全ecological decision recordsで、

```text
transfer_to_zone_contribution = false
```

をhard guardとして保持した。

---

# 6. 再現性の前提 — M1–M3 exact materialization

NAP-002は、過去のplanning geometryやvegetation outputsを「だいたい同じ」状態で復元して分析を始めなかった。

最初のempirical planning-unit recordを作る前に、次の3つのhistorical identitiesをraw-byte SHA-256でexact verificationした。

## M1 — planning geometry / identity

```text
p001_p4_aso_pastures_193.geojson
features = 193
unique pu_id = 193
SHA-256 = 46ef4dd475ec7645d78130722e8ddfcae9f51c6244ce95dc22ea9f7dcaa6609d
```

## M2 — vegetation composition

```text
planning_unit_vegetation_composition.csv
rows = 1426
planning units = 193
SHA-256 = ac01c133cf8d366dc02d0da2b8e1ff1334e5b997f31b74553cd60f0afc4b5413
```

## M3 — open-grassland state

```text
planning_unit_open_grassland_state.csv
rows = 193
unique planning_unit_id = 193
SHA-256 = c6aee6d304e223189289e8b2f8bcbf5f42042b037304741365091eea77a4a93c
```

M1とM3のplanning-unit ID集合は完全一致し、M2のID集合もM1に完全に含まれた。

このgateを設けた理由は、地理データの再serializationや再生成による微妙な変更を、Study 1の新しいempirical stateとして無意識に混入させないためである。

---

# 7. 方法 — Decision-Gate architecture

## 7.1 なぜ一つのactionability scoreにしなかったのか

意思決定が進められない理由には、異なる種類がある。

```text
対象がいるか分からない
管理効果の証拠がない
source responseをこの場所へ移せない
safeguardが閉じていない
現場で実行できるか分からない
人・予算・機械等のcapacityが分からない
当日の条件・許可が分からない
```

これらを1つの0–100 scoreにまとめると、たとえば「高いevidence」が「未確認のsafety」を相殺するような誤読を生む。

Study 1では、各gateを**non-compensatory**に保持した。

## 7.2 G1–G8

| Gate | 問い | 主な情報レベル |
|---|---|---|
| G1 | このplanning unitはsemi-natural-grassland management reviewの対象か | public spatial state |
| G2 | relevant conservation targetがこの場所に関係するか | public + local target relevance |
| G3 | このcandidate actionに対するmanagement-response evidenceがあるか | source evidence |
| G4 | source responseをplanning unitへ転移できるか | response-transfer validation |
| G5 | ecological safeguard / conflictが閉じているか | source + local safeguard |
| G6 | actionをこの場所でoperationalに実行可能か | local operational facts |
| G7 | 必要なresource / capacityがあるか | local capacity |
| G8 | current operational conditionsが満たされるか | current/time-sensitive facts |

## 7.3 二層data model

G2–G5は保全対象によって状態が異なり得る。したがって、`planning unit × candidate action`だけに潰すと、target-specific conflictを失う。

このため二層とした。

```text
Tier E — ECOLOGICAL_FEATURE
planning_unit_id × candidate_action_id × feature_id
G2 target relevance
G3 management-response evidence
G4 response transferability
G5 ecological safeguard/conflict

Tier A — ACTION_CONTEXT
planning_unit_id × candidate_action_id
G1 management-review scope
G6 local operational feasibility
G7 resource/capacity feasibility
G8 current operational conditions
```

## 7.4 gate-state vocabulary

Study 1で使用した状態は次の8つである。

```text
PASS_PUBLIC
SUPPORTED_SOURCE_LOCAL
CONDITIONAL
UNRESOLVED
LOCAL_INPUT_REQUIRED
CURRENT_INPUT_REQUIRED
NOT_APPLICABLE
DO_NOT_INFER
```

ここで重要なのは、`SUPPORTED_SOURCE_LOCAL` がG3でのみ使われ、planning-unit applicabilityを意味しないことである。

```text
SUPPORTED_SOURCE_LOCAL at G3
!=
PASS_PUBLIC at G4
```

また、

```text
NOT_APPLICABLE
!=
SAFE
```

である。たとえばG5で `NOT_APPLICABLE` なら、「そのrowに登録済みのG5-specific conflictがない」というだけで、action全体の安全性を意味しない。

## 7.5 blocking states

次をdecision blockerとして扱った。

```text
CONDITIONAL
UNRESOLVED
LOCAL_INPUT_REQUIRED
CURRENT_INPUT_REQUIRED
DO_NOT_INFER
```

unknownをzero、neutral、safeへ変換するdefault ruleは設けていない。

---

# 8. G1 Management Review Scope

## 8.1 prospectively frozen rule

G1のprimary classification ruleは、empirical class distributionを見る前に凍結した。

```text
mapped_open_grassland_area_m2 > 0
    -> PASS_PUBLIC

mapped_open_grassland_area_m2 == 0
and mapped_state_unknown_fraction > 0
    -> UNRESOLVED

mapped_open_grassland_area_m2 == 0
and mapped_state_unknown_fraction == 0
    -> NOT_APPLICABLE
```

`> 0` はmanagement priority thresholdではない。公開overlay上にpositive-areaのmapped open/semi-natural-grassland candidateが存在するかという存在判定である。

## 8.2 結果

```text
PASS_PUBLIC      = 188 / 193 = 97.4093%
UNRESOLVED       =   1 / 193 =  0.5181%
NOT_APPLICABLE   =   4 / 193 =  2.0725%
```

したがって、公開空間情報だけでも、193 planning unitsの大部分についてsemi-natural-grassland management reviewの対象範囲を定義できた。

これは、

```text
どこで野焼きすべきか
どこで放牧すべきか
どこで刈取りすべきか
```

を示す結果ではない。

G1が示すのは、**管理判断を検討する空間scope**だけである。

リポジトリ版では、G1 mapを次に保持する。

`doc/figures/NAP002_G1_MANAGEMENT_REVIEW_SCOPE_MAP.svg`

## 8.3 planning unit 189をpost-hocに変更しなかった理由

primary classification後のdiagnosticで、planning unit 189のunknown signalは、実質的なunmapped vegetation classではなく、floating-point-scaleのcoverage residualのみであることが分かった。

しかしprimary ruleは結果を見る前に凍結していたため、Study 1は189を後から `NOT_APPLICABLE` へ変更しなかった。

これは、小さな数値残差を生態学的に重要視したという意味ではない。

むしろ、

> **primary resultとpost-primary diagnosticを分離し、結果を見た後でruleを都合よく動かさない**

というprospective integrityを優先した。

---

# 9. G2–G5 ecological gates

Tier Eは、G1で `NOT_APPLICABLE` となった4 unitsを除く189 units、13 candidate actions、9 decision featuresについて構築した。

```text
189 × 13 × 9 = 22,113 records
```

## 9.1 G2 — target relevance

```text
PASS_PUBLIC          =  4,888
LOCAL_INPUT_REQUIRED = 14,742
UNRESOLVED           =     26
NOT_APPLICABLE       =  2,457
```

公開vegetation stateから、T2 vegetation composition / successionとT4 open-grassland structureについては広いrelevanceを判断できる。

一方、named assemblage、rare/conservation-priority taxa、grassland-dependent fauna等のT1/T3/T5については、planning-unit local relevanceを公開情報から確定しない。

ここでも、

```text
public no-record
!=
absence
```

を保持した。

## 9.2 G3 — management-response evidence

```text
SUPPORTED_SOURCE_LOCAL =  5,670
UNRESOLVED              = 15,687
DO_NOT_INFER            =    756
```

5,670 recordsではsource-grounded response evidenceが存在した。

しかしこれは、

```text
source studyでresponseがある
```

ことを意味するだけで、

```text
このplanning unitで同じresponseが期待できる
```

ことを意味しない。

## 9.3 G4 — response transferability

```text
DO_NOT_INFER    =  5,670
NOT_APPLICABLE = 16,443
PASS_PUBLIC     =      0
```

Study 1の最も重要な境界の一つである。

G3でsource-supportedだった5,670 recordsは、**すべてG4でtransferを禁止された。**

つまり、

```text
source-local/source-relative response
!=
planning-unit response
```

というNAP-001の境界をそのまま保持した。

G3 evidenceがないrowでG4が `NOT_APPLICABLE` なのは、transfer問題が解決したという意味ではない。transfer以前に、転移するsource response自体がないためである。

## 9.4 G5 — safeguard / conflict

```text
CONDITIONAL    =  1,323
UNRESOLVED     =  2,079
NOT_APPLICABLE = 18,711
```

G5は、target-specificなadverse/conflicting signalやexplicit safeguardを保持する。

たとえば、管理のtimingやintensityが特定targetに対して競合する場合があっても、それを1つの平均benefit scoreで相殺しない。

---

# 10. 13 candidate actionsのblocker profile

Study 1では、candidate actionを「推奨候補」ではなく、レビュー対象となるoperational/action familyとして扱う。

以下の数値は**evidence coverage / blocker structure**であり、action rankingではない。

| Candidate action | G3 source-supported | G3 unresolved | G3 context-only | G4 transfer validation | G5 conditional | G5 unresolved | G7 | G8 |
|---|---:|---:|---:|---:|---:|---:|---|---|
| Burning without grazing | 5 | 3 | 1 | 5 | 0 | 1 | LOCAL_INPUT_REQUIRED | CURRENT_INPUT_REQUIRED |
| Burning + low grazing | 3 | 6 | 0 | 3 | 0 | 1 | LOCAL_INPUT_REQUIRED | CURRENT_INPUT_REQUIRED |
| Burning + customary grazing | 3 | 6 | 0 | 3 | 0 | 1 | LOCAL_INPUT_REQUIRED | CURRENT_INPUT_REQUIRED |
| Burning + high grazing | 3 | 6 | 0 | 3 | 1 | 1 | LOCAL_INPUT_REQUIRED | CURRENT_INPUT_REQUIRED |
| July mowing | 3 | 6 | 0 | 3 | 2 | 0 | LOCAL_INPUT_REQUIRED | CURRENT_INPUT_REQUIRED |
| September mowing | 3 | 6 | 0 | 3 | 1 | 1 | LOCAL_INPUT_REQUIRED | CURRENT_INPUT_REQUIRED |
| July + September mowing | 3 | 6 | 0 | 3 | 3 | 0 | LOCAL_INPUT_REQUIRED | CURRENT_INPUT_REQUIRED |
| Biennial September mowing | 2 | 7 | 0 | 2 | 0 | 1 | LOCAL_INPUT_REQUIRED | CURRENT_INPUT_REQUIRED |
| Burning + grazing intensity unresolved | 0 | 8 | 1 | 0 | 0 | 1 | UNRESOLVED | CURRENT_INPUT_REQUIRED |
| Grazing without recorded burning, intensity unresolved | 0 | 8 | 1 | 0 | 0 | 1 | UNRESOLVED | CURRENT_INPUT_REQUIRED |
| Active grassland management unresolved | 2 | 7 | 0 | 2 | 0 | 1 | UNRESOLVED | NOT_APPLICABLE |
| Management cessation / abandonment trajectory | 3 | 6 | 0 | 3 | 0 | 1 | NOT_APPLICABLE | NOT_APPLICABLE |
| Neither recorded burning nor grazing | 0 | 8 | 1 | 0 | 0 | 1 | UNRESOLVED | NOT_APPLICABLE |

全actionについて、G6はG1 non-excluded unitsで `LOCAL_INPUT_REQUIRED` である。

また、source-supported feature数が多いactionを「より良い」「より推奨される」と解釈してはならない。たとえばburn-onlyは13 actions中でsource-supported coverageが最も広いが、その5 featuresはすべてG4 transfer barrierに達する。

---

# 11. G6–G8 action-context gates

Tier Aは193 planning units × 13 candidate actionsで構築した。

```text
193 × 13 = 2,509 records
```

## 11.1 G1 row distribution in Tier A

```text
PASS_PUBLIC    = 2,444  (= 188 × 13)
UNRESOLVED     =    13  (= 1 × 13)
NOT_APPLICABLE =   52  (= 4 × 13)
```

## 11.2 G6 — local operational feasibility

```text
LOCAL_INPUT_REQUIRED = 2,457
NOT_APPLICABLE       =    52
PASS_PUBLIC          =     0
```

G6が必要とするのは、actionによって、

- action specification
- access
- infrastructure
- authority / institutional feasibility

等である。

公開regional contextから、planning-unit levelの実行可能性を補完しなかった。

## 11.3 G7 — resource / capacity

```text
LOCAL_INPUT_REQUIRED = 1,512
UNRESOLVED           =   756
NOT_APPLICABLE       =   241
PASS_PUBLIC          =     0
```

人員、労働、家畜、機械、予算、組織capacity等を、regional evidenceからplanning unitへ自動転移しない。

## 11.4 G8 — current operational conditions

```text
CURRENT_INPUT_REQUIRED = 1,890
NOT_APPLICABLE         =   619
PASS_PUBLIC            =     0
```

特にfield actionでは、historical public dataでは代替できないcurrent/time-stamped情報がある。

野焼きfamilyでは、例として次を区別する。

```text
CURRENT_SITE_CONDITION
CURRENT_WEATHER_CONDITION
CURRENT_FUEL_OR_BIOMASS_CONDITION
CURRENT_SAFETY_ARRANGEMENT
CURRENT_PERMISSION_OR_RESTRICTION
```

Study 1はこれらを取得しておらず、real-time `GO / NO-GO` systemではない。

---

# 12. 完全なdecision engine

Tier EとTier Aを統合したmachine-readable engineは次の規模になった。

```text
Tier E — ECOLOGICAL_FEATURE = 22,113
Tier A — ACTION_CONTEXT     =  2,509
TOTAL                        = 24,622
```

完全validatorの結果は、

```text
records validated = 24,622
validation errors = 0
transfer_to_zone_contribution=true = 0
```

であった。

schema、candidate-action version、feature identity、blocker derivation、required-input gate対応、Tier-A / Tier-E link等を機械検査した。

---

# 13. Study 1の中心結果 — action-ready combinationは0

G1で `NOT_APPLICABLE` ではない189 unitsについて、13 actionsを組み合わせると、

```text
189 × 13 = 2,457 planning-unit × candidate-action combinations
```

となる。

この2,457 combinationsすべてで、

```text
ecological blocking feature count = 9 / 9
G6 local operational feasibility   = blocked
```

であった。

さらに、

```text
G4 PASS_PUBLIC = 0
G6 PASS_PUBLIC = 0
G7 PASS_PUBLIC = 0
G8 PASS_PUBLIC = 0
```

である。

したがって、

```text
public-only action-ready combinations = 0
```

と閉じた。

この結果を、

> すべての管理が不適切である

とは解釈しない。

正しい解釈は、

> **現在のpublic evidence chainだけでは、planning-unit operational recommendationに必要なdecision chainが完全には閉じない。**

である。

---

# 14. 「情報不足」をinformation-need architectureへ変換する

Study 1の実務的な進展は、単に

```text
more data are needed
```

と結論しなかった点にある。

不足を、性質の異なる情報へ分解した。

## 14.1 local ecological facts

```text
LOCAL_TARGET_RELEVANCE
LOCAL_SAFEGUARD_STATUS
```

これは、特定targetがそのplanning unitに関係するか、local safeguard conditionがどうなっているかという現地・local contextの事実である。

## 14.2 scientific research / validation

```text
RESPONSE_TRANSFER_VALIDATION
missing / unusable G3 management-response evidence
```

これは同日の現場checklistで埋められる情報ではない。

source responseをplanning unitへ intended precisionで適用できることを正当化する新しい科学的検証が必要である。

## 14.3 local operational facts

```text
LOCAL_ACTION_SPECIFICATION
LOCAL_ACCESS_FEASIBILITY
LOCAL_INFRASTRUCTURE_FEASIBILITY
LOCAL_AUTHORITY_FEASIBILITY
```

candidate regimeの一部には、source-defined / unresolvedなaction dimensionが残る。したがって、管理名だけで「実行可能なaction specificationが確定している」と仮定しない。

## 14.4 local resource / capacity

```text
LOCAL_RESOURCE_CAPACITY
```

地域に草原管理の歴史や支援制度があることと、特定planning unitで必要な人員・予算・家畜・機械・作業時間が確保できることは別問題である。

## 14.5 current / time-sensitive facts

```text
CURRENT_SITE_CONDITION
CURRENT_WEATHER_CONDITION
CURRENT_FUEL_OR_BIOMASS_CONDITION
CURRENT_SAFETY_ARRANGEMENT
CURRENT_PERMISSION_OR_RESTRICTION
```

これらはhistoric public GISから推定して埋めるべきではない。

---

# 15. 最終的な実務表現 — WHERE / WHY / WHAT NEXT

Study 1のprimary applied outputは、一枚のrecommendation mapではなく、三つの役割を分けた構造である。

## WHERE — Management Review Scope Map

G1は、

> **どのplanning unitをsemi-natural-grassland management decisionのレビュー対象に含めるか**

を示す。

## WHY — Decision Blocker Matrix

Candidate Action × G2–G8 matrixは、

> **なぜその候補行動を現在のpublic evidenceだけでは選択できないのか**

を示す。

## WHAT NEXT — Required-Input Checklist

Checklistは、

> **そのcandidate actionをさらに評価するなら、次に何を確認・取得・研究すべきか**

を示す。

この三部構造は、

```text
レビュー対象
判断不能理由
次に必要な情報
```

を分離する。

---

# 16. なぜ13枚のaction-specific public mapを作らなかったのか

現在のpublic-only evidenceでは、同じcandidate actionについて、188のG1-PASS unitsのG2–G8 blocker profileは同一である。

planning unitごとの差を作るはずの情報、すなわち、

```text
local target occurrence
local safeguard status
validated response transfer
local action specification
local feasibility
local resources
current conditions
```

が、まさに未観測だからである。

この状態で13 action-specific mapsを作ると、ほぼ同じG1 footprintを異なるlegendで塗り直すことになる。

地図があることで空間的なaction differentiationが存在するように見える可能性があるため、Study 1はこれを**pseudo-spatial precision**として避けた。

「地図を作らない」という判断も、研究結果の一部である。

---

# 17. 実務での読み方

Study 1のoutputは、次の順序で読むことを想定する。

```text
1. G1 scope mapでplanning unitを確認する。

2. NOT_APPLICABLEなら、
   current semi-natural-grassland decision frameworkの対象として扱わない。

3. UNRESOLVEDなら、
   spatial/relevance uncertaintyを先に確認する。

4. PASS_PUBLICなら、
   candidate actionを「承認」ではなく「レビュー対象」として選ぶ。

5. blocker matrixを横に読む。

6. required-input checklistで、
   local / current / research information needを確認する。

7. missing informationをdefault passへ変換しない。
```

これにより、public evidenceからlocal decision workへのhand-offを明示できる。

---

# 18. 例 — Burning without grazing

G1が `PASS_PUBLIC` のplanning unitで、

```text
CR_BURN_ONLY
Burning without grazing
```

をレビューするとする。

public-only engineは概略として次を返す。

```text
G1  management-review scope       PASS_PUBLIC

G2  target relevance              T2/T4はbroad public relevance
                                   T1/T3/T5はlocal relevanceが必要

G3  response evidence             5 / 9 source-supported
                                   3 / 9 unresolved
                                   1 / 9 context-only

G4  response transfer             5 source-supported featuresすべて
                                   transfer validationが必要

G5  safeguard/conflict            safeguard unresolved

G6  local feasibility             local input required

G7  resource capacity             local input required

G8  current conditions            current input required
```

このとき正しいoutputは、

> **candidate action remains blocked; local/current checks and scientific response-transfer validation are required.**

である。

誤ったoutputは、

```text
burning is recommended
burning is optimal
this unit should be burned
burning is safe
G1 PASS means permission to burn
```

である。

---

# 19. Synthetic Conditional Decision Prototype

## 19.1 なぜsynthetic prototypeを使ったのか

Study 1ではreal local/current inputsを取得していない。

それでも、decision architectureが、local/current factual gapsを埋めた後にどのように振る舞うかを検証する必要があった。

そこで完全にsyntheticな2 scenariosを事前定義した。

```text
S1  Burning without grazing
S2  July + September mowing
```

実在planning unit ID、実在のweather、permission、target occurrence、safety status等は使用していない。

## 19.2 factual completionとscientific evidenceを分ける

synthetic scenariosでは、許可されたlocal/current factual requirementsを「すべて満たされた」と仮定した。

しかし、次は仮定しなかった。

```text
missing G3 management-response evidence
G4 response transferability
```

これらをlocal checklist項目としてsynthetically passさせると、科学的evidence gapを現場入力で偽装することになるためである。

## 19.3 結果

```text
S1 burn-only             -> RESEARCH_EVIDENCE_REQUIRED
S2 July+September mowing -> RESEARCH_EVIDENCE_REQUIRED
```

burn-onlyでは、local/current factual completion後も、G3 unresolved/context-onlyとG4 transfer blockersが残った。

July+September mowingでも、G3 unresolvedとG4 transfer blockersが残った。

このprototypeは、次を明示する。

> **現場情報をすべて集めれば、自動的にmanagement recommendationへ到達するわけではない。local/current factsとscientific response evidenceは別の情報階層である。**

---

# 20. 再現性と検証

## 20.1 machine validation

Study 1は、会話内の手計算や手作業の地図分類だけで閉じていない。

GitHub Actions上で、frozen repository inputsからmaterializationとvalidationを再実行した。

主要runは次のとおりである。

```text
31669720153  G2-G5 validation                 SUCCESS
31670157556  full G1-G8 engine                SUCCESS
31670617847  blocker presentation             SUCCESS
31671128589  synthetic conditional prototype  SUCCESS
```

latest synthetic-inclusive workflow artifact:

```text
artifact id = 9169736427
SHA-256 = 65b99e38df61fd5e00dfd8672dce7f39cfc349d5ec2931e1c437cabf403f9475
```

## 20.2 full-engine validation

```text
records validated = 24,622
record tiers:
  ECOLOGICAL_FEATURE = 22,113
  ACTION_CONTEXT     =  2,509

error_count = 0
transfer_to_zone_contribution_true_count = 0
```

## 20.3 adversarial audit

Study 1 closure時には、再現性だけでなく、以下のfailure modeを明示的に監査した。

```text
NAP-001 historical evidenceの書換え
unknown -> zero / neutral / safe
public no-record -> ecological absence
Q2/source-local -> Q3 planning-unit response promotion
G4の暗黙pass
weighted actionability score
hidden ecological roll-up
action ranking
management recommendation
action authorization
manual post-result recoloring
real local/current valuesの混入
restricted/private empirical dataの混入
```

最終監査は `PASS` で閉じた。

---

# 21. 本研究が支持すること

NAP-002 Study 1は、少なくとも以下を支持する。

1. 公開植生stateからsemi-natural-grassland management-review scopeをplanning-unit単位で再現可能に定義できる。
2. management decisionをtarget relevance、response evidence、transferability、safeguard、feasibility、capacity、current conditionsへ分解できる。
3. source-supported evidenceとplanning-unit applicabilityを別gateとして保持できる。
4. target-specific ecological evidenceを一つの平均action scoreへ潰さず保持できる。
5. unknown / missing local/current factsをdecision blockerとしてmachine-readableに表現できる。
6. 「more data needed」を具体的なrequired-input categoryへ分解できる。
7. public evidenceで空間差がないところに、action-specific mapを人工的に作らないpresentation ruleを実装できる。
8. local/current factual completion後もscientific evidence gapが残り得ることをsynthetic prototypeで示せる。
9. negative/non-ready stateを、実務で次に何を確認するかへ接続できる。
10. NAP-001のnon-transferability ceilingを変更せずに、より実務的なdecision-support representationへ進める。

---

# 22. 本研究が支持しないこと

Study 1は以下を主張しない。

- 188 `PASS_PUBLIC` unitsで何らかの管理を実行すべきである
- 野焼き、放牧、刈取りのどれかが最適である
- source-supported feature数が多いactionほど望ましい
- `NOT_APPLICABLE` がecologically safeを意味する
- G1 `PASS_PUBLIC` がaction authorizationを意味する
- public no-recordがtarget absenceを意味する
- regional capacity evidenceがplanning-unit capacityを意味する
- historical dataがcurrent weather/safety/permissionを代替できる
- source-local responseをplanning unitへ直接転移できる
- 13 candidate actionsのrankを提示した
- action suitability surfaceを構築した
- optimal management mosaicを構築した
- real-time controlled-burning go/no-go systemを構築した
- practitioner decision qualityを改善した
- 新しい一般的decision-science theoryを発明した

特に、**practitioner benefitはStudy 1では測定していない。**

---

# 23. 実務的含意

NAP-002 Study 1の重要な実務的含意は、decision-support systemが必ずしも「推奨」を返す必要はないことである。

不確実なworking landscapeでは、より安全で有用なoutputが、

```text
ここはレビュー対象である
このactionにはこの証拠がある
ここから先は転移できない
このsafeguardが未確認である
このlocal factが必要である
このcurrent checkが必要である
この点は現場確認ではなく新しい研究が必要である
```

という**decision boundaryの可視化**である場合がある。

これは「判断を先送りする」こととは異なる。

何が決められないかを理由別に分解すれば、

- 行政が確認すべきlocal fact
- 現場が当日に確認すべきcondition
- 研究者が新たに検証すべきresponse transfer

を混同せずに次の作業へ接続できる。

---

# 24. 研究上の限界

## 24.1 public-only ceilingは「世界に情報がない」という意味ではない

Study 1が示すのは、凍結されたpublic-only evidence architectureのdecision ceilingである。

restricted/local datasets、新規field survey、最新の現場情報に追加情報が存在する可能性を否定しない。

## 24.2 G1はreview scopeでありhabitat quality scoreではない

G1の `PASS_PUBLIC` はmapped open/semi-natural-grassland candidateのpositive-area presenceを意味する。

面積の大小、habitat quality、管理優先度、target valueを表さない。

## 24.3 G2–G8の空間差が少ないこと自体がcurrent evidenceの限界である

188 G1-PASS unitsでaction-specific G2–G8 profileが同じであることは、阿蘇の全planning unitsが実質的に同じという意味ではない。

それらを区別するlocal/current factsがcurrent public corpusにないことを意味する。

## 24.4 response-transfer validationは未実施である

G4 `PASS_PUBLIC = 0` はStudy 1の重要な結果である。

将来、prospective transfer-validation studyによって一部が進む可能性はあるが、Study 1の歴史結果を後から書き換えるものではない。

## 24.5 practitioner usability / effectivenessは未評価である

transparentなblocker matrixやuncertainty representationが、人間の意思決定を実際に改善するとは自動的に言えない。

表示が複雑すぎる、誤読される、必要な情報が現場workflowに合わない等の可能性がある。

これを検証するには、別のprospective Study 2が必要である。

## 24.6 Study 1はformal VoIを計算していない

どのinformation gapを解消する価値が高いかという順位は、objective / outcome / response modelが十分に閉じていない状態では正当化できない。

required-inputのbox数を「優先度」へ変換してはならない。

---

# 25. 第三者資料・データの取扱い

このpublic reportは単体公開を想定し、第三者著作物やsensitive/local dataの再配布を必要最小限にしている。

本レポートには、

- 原著論文PDFそのもの
- 原著論文の図表画像
- 大量な逐語引用
- restricted/local raw data
- sensitive rare-species coordinates
- 個人・生産者識別情報
- real current operational/safety information

を収録していない。

本文では、前身NAP-001が凍結したsource-level evidence architectureと、NAP-002が実装したdecision-support transformationを要約している。

研究内部の再現性・監査資料はrepositoryに保持するが、それらを読まなくても本文の主要結論は理解できるようにした。

---

# 26. Study 2との境界

Study 1の完了後に最も自然なsuccessorの一つは、

> **NAP-002 Study 2 — practitioner / administrative decision-support validation**

である。

Study 2が扱うべき問いは、Study 1とは異なる。

Study 1:

> 証拠制約を保持しながら、scope / blocker / information needへ変換できるか。

Study 2:

> そのrepresentationは、実際の行政担当者・草原管理者・意思決定者に理解可能で、workflowに適合し、判断の透明性・一貫性・適切な情報取得に寄与するか。

Study 2がpositive resultでもnegative resultでも、Study 1のformal resultは変更しない。

たとえばStudy 2で、

```text
情報量が多すぎる
matrixより別のUIが理解しやすい
required-input terminologyが現場用語と合わない
scope mapは有用だがblocker matrixは使いにくい
```

等が示された場合、それはStudy 1の失敗ではなく、**scientifically conservative architectureとhuman-facing interfaceの間に新しいdesign problemがある**ことを示す独立結果になる。

---

# 27. その他の将来研究

Study 1を再解析してdirect recommendation mapを作るのではなく、必要に応じて独立prospective studyとして進める。

## 27.1 response-transfer validation

G4を直接扱う研究である。

必要となり得るのは、

- operational management definition
- source / target contextの比較可能性
- target-relevant outcome
- context modifier
- repeated observation
- transfer ruleの事前定義
- external / temporal validation

等である。

## 27.2 Future Stage A — restricted/local empirical data

application/permission-based datasetsやlocal operational informationを用いる可能性がある。

ただしrestrictedであること自体は科学的妥当性を保証しない。新しいstudy protocolとeligibility auditが必要である。

## 27.3 Future Stage B — prospective field / monitoring

管理応答やcausal / monitoring gapを新しいprospective designで測定する。

Study 1のblocker structureは、どの情報がdecision-relevantかを整理する入力にはなり得るが、monitoring design自体の新規性は主張しない。

## 27.4 operational pilot

real decision-time local/current input protocolを別途設計し、permission、safety、weather等のtime-sensitive informationを扱う可能性がある。

これはStudy 1のpublic-only engineとは別systemとして扱う必要がある。

---

# 28. 再現性識別子

Study 1の主要なfrozen identitiesを以下にまとめる。

## planning-unit geometry

```text
count = 193
SHA-256 = 46ef4dd475ec7645d78130722e8ddfcae9f51c6244ce95dc22ea9f7dcaa6609d
```

## vegetation composition

```text
rows = 1426
SHA-256 = ac01c133cf8d366dc02d0da2b8e1ff1334e5b997f31b74553cd60f0afc4b5413
```

## open-grassland state

```text
rows = 193
SHA-256 = c6aee6d304e223189289e8b2f8bcbf5f42042b037304741365091eea77a4a93c
```

## complete decision engine

```text
Tier E records = 22,113
Tier A records =  2,509
TOTAL          = 24,622
validation errors = 0
```

## latest synthetic-inclusive CI artifact

```text
artifact id = 9169736427
SHA-256 = 65b99e38df61fd5e00dfd8672dce7f39cfc349d5ec2931e1c437cabf403f9475
```

## public report provenance

```text
repository = nkkmd/natural-area-planning
study-state snapshot commit = 93a6ab91c0dbcbac2dfa32c4ff11700f745770fc
study date = 2026-08-13 JST
```

---

# 29. Repository内の主要な再現性資料

このレポート単体で主要結論を理解できるが、完全なprovenanceとmachine auditはrepositoryで追跡できる。

主要資料:

```text
doc/checkpoints/2026-08-13-nap002-study1-formal-closure.md
doc/nap002/NAP002_STUDY1_REPRODUCIBILITY_ADVERSARIAL_AUDIT.md
analysis/nap002/study1_reproducibility_audit.json

doc/nap002/NAP002_DECISION_GATE_SCHEMA_PROTOCOL.md
analysis/nap002/decision_record_schema.json
analysis/nap002/validate_decision_records.py

doc/nap002/NAP002_G1_MANAGEMENT_REVIEW_SCOPE_PROTOCOL.md
analysis/nap002/g1_management_review_scope_states.csv

doc/nap002/NAP002_G2_G5_ECOLOGICAL_GATE_PROTOCOL.md
analysis/nap002/decision_blocker_action_matrix.csv

doc/nap002/NAP002_G6_G8_ACTION_CONTEXT_PROTOCOL.md
analysis/nap002/required_input_action_checklist.csv

doc/nap002/NAP002_SYNTHETIC_CONDITIONAL_DECISION_PROTOCOL.md
analysis/nap002/synthetic/conditional_decision_results.csv
```

---

# 30. 結論

Natural Area Planning / NAP-002 Study 1は、NAP-001が到達したpublic-only evidence ceilingを、unsupported management recommendationで覆い隠すのではなく、**実務で次に何を確認すべきかを示すdecision-support architectureへ変換できること**を示した。

公開空間情報だけでも、

```text
188 / 193 planning units
```

についてmanagement-review scopeを定義できた。

しかし、24,622 recordsからなる完全decision engineでは、

```text
G4 PASS_PUBLIC = 0
G6 PASS_PUBLIC = 0
G7 PASS_PUBLIC = 0
G8 PASS_PUBLIC = 0
public-only action-ready combinations = 0
```

であった。

この結果は、public-only decision supportが無意味であることを示さない。

むしろ、

```text
どこをレビューするか
何が判断を止めているか
どのlocal factが必要か
どのcurrent conditionが必要か
どこは新しい科学的検証が必要か
```

を分離して提示できる。

Study 1の最終到達点は、次の一文に要約できる。

> **証拠が候補行動の選択まで届かないとき、意思決定支援はその不足を隠して推奨を作るのではなく、どこまで分かっており、何が判断を止め、何を次に確認すべきかを明示できる。**

NAP-001が「証拠が止まるところでoptimizationを止める」研究だったとすれば、NAP-002 Study 1は、**その停止点を実務上のdead endにせず、次の確認・研究・意思決定へ接続する方法を実装した研究**である。

---

# 用語

**planning unit**  
空間的な評価・計画の単位。本研究ではNAP-001から継承した193 unitsを固定した。

**candidate action / candidate management regime**  
意思決定上レビューする管理行動の候補。候補に含まれること自体は推奨・安全・許可を意味しない。

**decision gate**  
候補行動を判断する前に個別に確認する条件・証拠interface。Study 1ではG1–G8を使用した。

**decision blocker**  
候補行動を現在の情報だけでは先へ進められない状態。Study 1では `CONDITIONAL`、`UNRESOLVED`、`LOCAL_INPUT_REQUIRED`、`CURRENT_INPUT_REQUIRED`、`DO_NOT_INFER`をblocking statesとした。

**information need**  
blockerを解消するために必要な追加情報の型。local fact、current fact、scientific validationを区別する。

**response transferability**  
source studyで観測されたmanagement responseを、別のplanning contextへ intended precisionで適用できるかという問題。

**SUPPORTED_SOURCE_LOCAL**  
G3でsource-grounded response evidenceがある状態。planning-unit responseが確立したことを意味しない。

**DO_NOT_INFER**  
現在のevidence boundaryを超えた推論を禁止する状態。

**pseudo-precision**  
証拠が正当化しない数値・順位・空間差を、計算やvisualizationの都合で精密に見せること。

---

# Selected methodological references / prior art

NAP-002 Study 1は以下を含むdecision-science / uncertainty / decision-support literatureをprior artとして扱い、一般的方法論のnoveltyを主張しない。

- Martin et al. (2009). Structured decision making / decision-threshold literature. DOI: `10.1890/08-0255.1`
- Robinson et al. (2016). Large-scale wildlife Structured Decision Making application. DOI: `10.1002/ecs2.1613`
- Schwartz et al. (2017). Conservation decision-support frameworks. DOI: `10.1111/conl.12385`
- Hemming et al. (2022). Decision science in conservation. DOI: `10.1111/cobi.13868`
- Lyons et al. (2008). Decision-driven monitoring / Structured Decision Making. Identifier: `USGS_5224905`
- Williams & Johnson (2015). Value of Information in natural-resource management. DOI: `10.1002/wsb.575`
- Runge, Converse & Lyons (2011). Value of Information / identifying decision-relevant uncertainty. DOI: `10.1016/j.biocon.2010.12.020`
- Lawson et al. (2022). Qualitative Value of Information. DOI: `10.1111/csp2.12732`
- Regan et al. (2005). Robust conservation decision making under severe uncertainty. DOI: `10.1890/03-5419`
- Sierra-Altamiranda et al. (2020). Spatial conservation planning under uncertainty. DOI: `10.1016/j.ecolmodel.2020.109016`
- Cattarino (2018). Multi-action spatial management prioritization under response uncertainty. DOI: `10.1111/1365-2664.13147`
- Moore et al. (2021). Conservation resource allocation among multiple threats/actions. DOI: `10.1111/cobi.13748`
- Aerts, Clarke & Keuper (2003). Spatial uncertainty visualization in decision support. DOI: `10.1559/152304003100011180`
- Gallo & Goodchild (2012). Mapping uncertainty in conservation assessment/planning. DOI: `10.1080/08941920.2011.578119`
- Korporaal, Ruginski & Fabrikant (2020). Effects of uncertainty visualization on map-based decisions. DOI: `10.3389/fcomp.2020.00032`
- Arciniegas, Janssen & Rietveld (2013). Collaborative map-based spatial decision support evaluation. DOI: `10.1016/j.envsoft.2012.02.021`

Companion predecessor report:

- Natural Area Planning / NAP-001 — Public Research Report (2026), `doc/publication/NAP001_PUBLIC_RESEARCH_REPORT.md`

---

## 引用時の推奨表記

本レポートを引用する場合は、少なくとも次の情報を含めることを推奨する。

```text
Natural Area Planning / NAP-002 Study 1 (2026).
Public Research Report: 公開情報だけで阿蘇半自然草原の管理判断をどこまで支援できるか
— 「何をすべきか」を断定せず、「何が判断を止めているか」を明示する —.
Version 1.0.1, 2026-08-13.
Study snapshot: nkkmd/natural-area-planning @ 93a6ab91c0dbcbac2dfa32c4ff11700f745770fc.
```

この `Study snapshot` はStudy 1の凍結された科学的状態を指し、その後のpublication packagingやeditorial-only commitを指すものではない。

著者名、所属、DOI、恒久公開URL等が後日正式に付与された場合は、それらを優先して引用情報へ追加する。

---

**Study-1 formal status: COMPLETE / FROZEN. Scientific findings unchanged in v1.0.1.**