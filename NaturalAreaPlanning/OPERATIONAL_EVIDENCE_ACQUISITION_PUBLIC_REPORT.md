# Natural Area Planning / NAP-OEA-2026-08-16-v1 — Public Research Report

**Operational Pilotで未解決だった運用入力を、固定T0でどこまで解消できるか**  
**― provenance・時間・地理・authority条件を結果より先に固定したprospective evidence acquisition ―**

- レポート版: **v1.0**
- 作成日: **2026-08-24 JST**
- 研究ID: `NAP-OEA-2026-08-16-v1`
- 研究状態: **COMPLETE / FROZEN**
- formal outcome: **`PARTIAL_TARGET_INPUT_RESOLUTION`**
- 正式評価時点: **2026-08-24 12:00 JST**
- scientific-state snapshot: **`3788ba409956cc9806d0877a3bfa94e6fdd6258a`**
- 本文の性格: **外部公開用・単体完結型研究報告**

この文書は、元のGitHubリポジトリや内部artifactを参照しなくても、本研究の背景、問い、方法、主要結果、限界、再現性、解釈境界を理解できるように構成している。完全なprovenanceとmachine-readableな正本は、末尾に示すprotocol・formal result・checkpoint等に保持されている。

# 要旨

Operational Pilot v1は、local/current operational factsが不足・未検証であっても、それをpermission、safety、feasibility、recommendation、ecological meaningへ変換せずdecision-support workflowへ接続できることを検証した。一方で、正式pilotでは192のrequired operational inputsのうちusable inputは0であり、実際に未解決入力を追加の局所・制限付き・公式情報からどこまで解消できるかは未検証だった。

本研究はその未解決点を別の独立prospective studyとして扱い、Operational Pilot v1の凍結状態から値を見ずに選択した30 target cellsについて、適法に利用可能なlocal/restricted/public recordsまたはprotocolled direct observationを用いて、固定されたdecision time `T0 = 2026-08-24T12:00:00+09:00` にどこまで解決できるかを検証した。

結果は次のとおりである。

```text
target cells = 30
resolved = 2
unresolved = 28
blockers released = 2
EA1-EA11 = PASS
adversarial cases = 22 / 22 PASS
formal outcome = PARTIAL_TARGET_INPUT_RESOLUTION
```

解決したのは2件の `CURRENT_WEATHER_CONDITION` のみで、JMA AMeDAS `86111 / 阿蘇乙姫` を事前固定した `NEAREST_OFFICIAL_REFERENCE_WEATHER` として使用した。残る28件は条件を緩和せず未解決のまま保持した。

本研究が示したのは、厳格なprovenance・temporal validity・geographic linkage・authority ceilingを維持したまま、事前固定した未解決operational inputsの一部をT0時点で解消できたという**evidence-acquisition result**である。管理行為の推奨、順位付け、安全性、許可、望ましさ、生態学的効果、人間による受容性を示すものではない。

# 1. 研究の背景

Natural Area Planningでは、公開情報からmanagement-specific planningへ到達する際のevidence ceilingをNAP-001で検証し、その後NAP-002 Study 1で、recommendationを作らずにmanagement-review scope、decision blockers、required informationを表現するdecision-support workflowを構築した。

NAP-002 Study 2Aはその表現をhuman participantなしでpre-validationし、G4 Response-Transfer Validation Study v1はsource-local ecological responseを別contextへ転移するための独立validation evidenceを評価した。G4 v1はeligible held-out contextが0で `INDETERMINATE` となった。

Operational Pilot v1はさらに、local/current operational factsをworkflowへ接続するprospective validationを行った。そのformal outcomeは `WORKFLOW_VALIDATED_WITHIN_PILOT_SCOPE` だったが、usable required inputsは0/192で、30 pilot casesのいずれもoperational-workflow-readyではなかった。

そのため次の科学的問いとして、**未解決operational inputsそのものを、事前固定したルールのもとで実際にどこまで解消できるか**を独立studyとして検証する必要が生じた。

# 2. 研究対象

本研究のstudy objectは、Operational Pilot v1で未解決だったrequired operational-input cellsのうち、値を見ずにprospectively選択した30 cellsである。

対象には次のoperational domainsが含まれた。

```text
LOCAL_AUTHORITY_FEASIBILITY
LOCAL_ACTION_SPECIFICATION
LOCAL_ACCESS_FEASIBILITY
LOCAL_INFRASTRUCTURE_FEASIBILITY
CURRENT_PERMISSION_OR_RESTRICTION
CURRENT_SITE_CONDITION
CURRENT_WEATHER_CONDITION
CURRENT_FUEL_OR_BIOMASS_CONDITION
```

本研究の対象ではないものは次のとおりである。

```text
management recommendation
action ranking
ecological response estimation
planning-unit ecological coefficient
human-participant usability validation
G4 ecological transfer reinterpretation
optimization
```

# 3. 研究目的と中心的研究質問

中心的研究質問は次のとおりである。

> Operational Pilot v1で未解決だった運用入力のうち、結果を見る前に固定した30 target cellsについて、適法に利用可能でprovenance・時間・地理・authority条件を満たすevidenceを用いたとき、固定T0時点で何件を再現可能に解決できるか。

formal outcome vocabularyは結果を見る前に固定され、all gates passのもとで `0 < resolved < targetCount` の場合は `PARTIAL_TARGET_INPUT_RESOLUTION` とするルールが採用された。

# 4. 先行研究との関係とnovelty boundary

本研究は、provenance capture、prospective protocol、temporal validity、authority hierarchy、structured decision supportといった一般的方法論自体を新規発明として主張しない。

対象固有の新規性は、Natural Area Planningの凍結済みOperational Pilot stateから未解決operational inputsを値非依存で選択し、固定T0に対して、raw-byte provenance、source authority、geographic linkage、construct-specific freshness window、explicit unresolved stateを同時に維持しながらformal acquisition endpointを評価した点にある。

本研究はpredecessor studyのnegative/null/indeterminate resultを救済するための再解析ではない。

# 5. 使用した情報・使用しなかった情報

本研究では、prospective protocol上許可されたpublic-online authoritative informationを第一経路とした。lawfully accessible restricted/local recordやprotocolled direct observationもstudy design上のevidence modeとして定義されたが、formal resolutionに必須ではなかった。

本研究では次を使用していない。

```text
human participant data
unlawfully accessed restricted data
repository privacyをaccess authorizationとみなした情報
valueを見た後に選択した代替target
favorable conditionを選ぶためのsource switching
weatherをfuel conditionへ代用した値
generic regional proxyをPU-specific evidenceとみなした値
post-T0 evidenceによるfrozen endpointの救済
```

# 6. Prospective governance / freeze

新しいoperational valuesを確認する前に、少なくとも次を固定した。

```text
study identity
target-selection rules
30-cell target frame
evidence modes A-D
authority ceilings
raw provenance rules
lawful-access rules
temporal-validity rules
conflict hierarchy
missing/unresolved state vocabulary
direct-observation protocol
formal gates EA1-EA11
22 adversarial cases
formal outcome vocabulary
exact T0
Package 003 domain-specific freshness windows
weather station and spatial ceiling
```

pre-execution validationは `PRE1-PRE11 = PASS`、T0 freeze validationもPASSした後にacquisition authorizationが発効した。

# 7. 方法

## 7.1 Target frame

30 target cellsはOperational Pilot v1の凍結stateからdeterministicに選択した。新しいlocal/current valueはselection、balancing、replacementに使用していない。

Package構成は次のとおりである。

```text
Package 001: LOCAL_AUTHORITY_FEASIBILITY = 7 targets
Package 002: non-short-window operational inputs = 16 targets
Package 003: short-window current-condition inputs = 7 targets
```

## 7.2 Provenance gate

formal evidenceとして使用するraw responseは、解析前に少なくともbyte sizeとSHA-256を記録することを要求した。

```text
raw acquisition
-> byte count + SHA-256 materialization
-> parse
-> derived evidence state
```

検索snippetやdiscovery-only materialはformal evidenceへ昇格させなかった。

## 7.3 Temporal validity

retrieval timeとeffective / phenomenon / observation timeを分離した。retrieval timeをobservation timeの代用にはしていない。

Package 003の主要windowは次のとおりである。

```text
CURRENT_SITE_CONDITION
  2026-08-24 06:00–12:00 JST

fuel moisture / combustibility
  2026-08-24 09:00–12:00 JST

standing biomass / height / cover
  2026-08-23 12:00–2026-08-24 12:00 JST

CURRENT_WEATHER_CONDITION official observation
  2026-08-24 11:00–12:00 JST
```

## 7.4 Weather selection

weatherについては、値を見る前にstation `86111 / 阿蘇乙姫` を、target planning-unit source geometryのbounding-box midpointに対するnearest JMA AMeDAS stationとして固定した。

spatial classは次のとおりである。

```text
NEAREST_OFFICIAL_REFERENCE_WEATHER
```

これはstation-local authoritative observationをPUへのreference proxyとして使用する分類であり、PU内観測またはPU-exact microclimate measurementではない。

required tupleは次の5項目で固定した。

```text
hourly precipitation
hourly air temperature
hourly relative humidity
hourly mean wind speed
hourly wind direction
```

11:00–12:00 JST内のlatest complete official hourly tupleを使用し、forecast fallback、favorable-value selection、station switchingを禁止した。

## 7.5 Unresolved handling

凍結条件を満たすcase-specific authoritative recordを取得できない場合、条件を緩和してresolutionを作らず、`NO_COMPLIANT_RECORD_ACQUIRED` 等のterminal unresolved stateを保持した。

# 8. 主要結果

formal endpointは次のとおりである。

```text
target cells = 30
resolved = 2
unresolved = 28
blockers released = 2
resolution rate = 2 / 30
EA1-EA11 = PASS
formal outcome = PARTIAL_TARGET_INPUT_RESOLUTION
```

Package別結果:

```text
Package 001 = 0 / 7 resolved
Package 002 = 0 / 16 resolved
Package 003 site = 0 / 3 resolved
Package 003 fuel / biomass = 0 / 2 resolved
Package 003 weather = 2 / 2 resolved
```

## 8.1 解決した2件

解決したのは、事前固定された2件の `CURRENT_WEATHER_CONDITION` のみである。

T0時点のJMA raw responseはparse前に次のidentityを記録した。

```text
raw bytes = 8375
SHA-256 = f7b7e2c570b8ea924619eb831bb23f5ed689a39ea10b7dddf7ea630fcc6eb0e2
```

凍結済み11:00–12:00 JST window内で利用可能だったlatest complete exact-hour tupleは11:00 JST観測だった。

```text
station = 86111 / 阿蘇乙姫
phenomenon time = 2026-08-24T11:00:00+09:00
1時間降水量 = 0.0 mm
気温 = 29.7 C
相対湿度 = 60 %
平均風速 = 1.2 m/s
風向コード = 11
verification ceiling = VERIFIED_PERMITTED_NONCONTROLLING_SOURCE
```

## 8.2 解決しなかった28件

Package 001の7件、Package 002の16件、Package 003 site conditionの3件、fuel/biomass conditionの2件は解決しなかった。

適格なrecordを取得できなかった場合は条件を緩和せず未解決を維持した。`NO_COMPLIANT_RECORD_ACQUIRED` は、現実世界で情報、権限、現況、燃料状態が存在しないことを意味しない。

# 9. 本研究が支持すること

本研究は次を支持する。

1. 事前固定した30 unresolved operational-input targetsのうち、厳格なevidence rulesを維持したままT0時点で2件をformalに解決できた。
2. provenance gate、temporal validity、spatial ceiling、authority ceiling、missingness disciplineを保持したままformal materializationを完了できた。
3. 28件について適格evidenceが取得できなかった場合にも、target replacementやproxy relaxationを行わずunresolved stateを保持できた。
4. primary materializationとindependent recomputationが同一の30/2/28およびformal outcomeを再現した。

# 10. 本研究が支持しないこと

本研究は次を支持しない。

```text
2件のweather conditionが「好条件」だった
weather referenceがPU-exact microclimateを表す
blockerが2件減ったためmanagement actionを推奨できる
resolution rateがaction priorityを表す
NO_COMPLIANT_RECORD_ACQUIREDが情報や現況のabsenceを表す
current operational evidenceがecological response evidenceを表す
G4 ecological transferabilityが解決した
human validationが完了した
planning coefficientやoptimizer inputが得られた
```

# 11. 実務的含意

本研究の実務的含意は、operational decision supportに必要なcurrent/local factsを扱う際、**取得できた値だけでなく、取得できなかった状態をformalなblockerとして保持する必要がある**という点にある。

また、公式sourceであっても地理的にreference proxyである場合には、そのceilingを明示して利用する必要がある。今回のweather evidenceはその例であり、official station observationであることとPU-exact observationであることは区別された。

# 12. 研究上の限界

主な限界は次のとおりである。

1. **Evidence coverage ceiling**: 30 targets中28件はformal resolutionに至らなかった。
2. **Temporal limitation**: current-condition evidenceは固定T0と短いfreshness windowに強く依存する。
3. **Spatial limitation**: weatherはnearest official referenceであり、planning-unit内部観測ではない。
4. **Access limitation**: restricted/local recordsが存在しても、lawful access、case linkage、effective interval、authority条件を満たさなければformal evidenceにはできない。
5. **Direct observation limitation**: direct observationはprotocol上許可可能だったが、本研究のformal resolutionを構成する必須経路ではなく、今回の解決2件はpublic official weatherによる。
6. **Ecological limitation**: operational evidence acquisitionはmanagement responseやecological effectivenessを測定しない。
7. **Human-validation limitation**: practitioner / administrative participantを使用していない。
8. **Retrieval limitation**: public-online sourceの検索・公開timingによって、現実に存在する情報を取得できない可能性がある。no-recordはabsenceではない。

# 13. 第三者資料・データの取扱い

本研究では、repositoryがprivateであることをlawful-access basisやredistribution rightとはみなしていない。restricted/confidential raw dataをrepositoryへcommitすることをdefaultにはしていない。

第三者sourceについては、formal reproductionに必要なprovenance identity、derived status、hash等を記録し、不必要な個人情報やrestricted raw contentsの再配布を避ける方針を採用した。

# 14. 後続研究との境界

OEA v1はscientific closure済みであり、後から取得したpost-T0 evidenceを追加して28 unresolved targetsを救済してはならない。

後続研究で追加のoperational evidence acquisitionを行う場合は、新study identity、新T0またはtime frame、新target frame、新source/access rulesをprospectively固定する必要がある。

G4 successor、NAP-002 Study 2B、Future Stage A/B、NAP-003はそれぞれ別の研究routeであり、OEA v1の結果から自動的には開始されない。

# 15. 再現性・検証

formal gates:

```text
EA1  PASS  target traceability
EA2  PASS  provenance identity
EA3  PASS  geographic / jurisdiction linkage
EA4  PASS  temporal validity
EA5  PASS  source / authority ceiling
EA6  PASS  state-transition integrity
EA7  PASS  evidence-only blocker release
EA8  PASS  missingness / conflict discipline
EA9  PASS  historical / ecological / human firewall
EA10 PASS  independent recomputation
EA11 PASS  registered adversarial cases
```

Adversarial validation:

```text
registered = 22
passed = 22
```

Primary materialization and independent terminal-group recomputationは、いずれも次を返した。

```text
30 total
2 resolved
28 unresolved
PARTIAL_TARGET_INPUT_RESOLUTION
```

# 16. 再現性識別子

```text
study ID = NAP-OEA-2026-08-16-v1
scientific-state snapshot = 3788ba409956cc9806d0877a3bfa94e6fdd6258a
exact T0 = 2026-08-24T12:00:00+09:00
formal outcome = PARTIAL_TARGET_INPUT_RESOLUTION
targets = 30
resolved = 2
unresolved = 28
blockers released = 2
materialization canonical SHA-256 = b5516c84628bb038b18fe6520e17bd01f85e47c7b84aeebe275330ca3c83ba89
T0 weather raw SHA-256 = f7b7e2c570b8ea924619eb831bb23f5ed689a39ea10b7dddf7ea630fcc6eb0e2
```

# 17. Repository内の主要資料

## Protocol / governance

- `doc/operational-evidence-acquisition/OPERATIONAL_EVIDENCE_ACQUISITION_PROSPECTIVE_PROTOCOL.md`
- `analysis/operational-evidence-acquisition/oea_design_registry.json`
- `analysis/operational-evidence-acquisition/oea_target_selection_rules.json`
- `analysis/operational-evidence-acquisition/oea_temporal_validity_rules.json`
- `analysis/operational-evidence-acquisition/oea_provenance_access_rules.json`
- `analysis/operational-evidence-acquisition/oea_t0_registry.json`

## Package 003 execution

- `analysis/operational-evidence-acquisition/oea_package_003_execution_rule.json`
- `analysis/operational-evidence-acquisition/oea_package_003_site_condition_status.json`
- `analysis/operational-evidence-acquisition/oea_package_003_fuel_biomass_status.json`
- `analysis/operational-evidence-acquisition/oea_pkg003_weather_t0_raw_identity.json`
- `analysis/operational-evidence-acquisition/oea_pkg003_weather_t0_result.json`
- `analysis/operational-evidence-acquisition/oea_package_003_formal_status.json`

## Formal result / validation

- `analysis/operational-evidence-acquisition/oea_formal_result.json`
- `analysis/operational-evidence-acquisition/oea_formal_validation.json`
- `analysis/operational-evidence-acquisition/oea_formal_reproducibility.json`
- `doc/checkpoints/2026-08-24-operational-evidence-acquisition-formal-closure.md`
- `doc/operational-evidence-acquisition/CURRENT_STATUS.md`

# 18. 結論

事前固定した30 unresolved operational-input targetsのうち、厳格なprovenance・temporal・geographic・authority条件を維持したまま、T0時点でformalに解決できたのは2件だった。残る28件は条件を緩和せず未解決として保持された。

したがってformal outcomeは `PARTIAL_TARGET_INPUT_RESOLUTION` である。

この結果の中心的意味は「2件が管理上好ましい状態だった」ことではなく、**2件について必要な形式のoperational evidenceをprospective rulesのもとで取得・検証でき、28件については証拠不足を証拠不足のまま保持できた**ことにある。

# 用語

**T0**  
formal operational evidence snapshotを評価する共通decision time。本研究では2026-08-24 12:00 JST。

**NO_COMPLIANT_RECORD_ACQUIRED**  
凍結されたsource・construct・geographic・temporal条件を満たすrecordを取得できなかったterminal acquisition state。absenceを意味しない。

**NEAREST_OFFICIAL_REFERENCE_WEATHER**  
target planning unitに対して事前固定されたnearest official weather stationのstation-local observationをreference proxyとして用いるclass。PU-exact observationではない。

**VERIFIED_PERMITTED_NONCONTROLLING_SOURCE**  
source自体は正式・検証可能だが、target contextを直接controlするsourceではない場合のverification ceiling。

**blocker release**  
required inputがformal evidenceによって解決され、対応するdecision blockerがevidence-only ruleに従って解除されること。recommendationやpriorityを意味しない。

# Selected methodological references / prior art

本研究が依拠する一般的方法論は、prospective study governance、data provenance、temporal validity、authority/source hierarchy、structured decision support、explicit uncertainty/missingness handlingである。これら一般的方法の発明を本研究のnoveltyとして主張しない。

Natural Area Planning内の直接的predecessorとして、次の公開報告を参照する。

- `NAP001_PUBLIC_RESEARCH_REPORT.md`
- `NAP002_STUDY1_PUBLIC_RESEARCH_REPORT.md`
- `NAP002_STUDY2A_NONPARTICIPANT_PREVALIDATION_PUBLIC_REPORT.md`
- `G4_RESPONSE_TRANSFER_VALIDATION_PUBLIC_REPORT.md`
- `OPERATIONAL_PILOT_PUBLIC_REPORT.md`

# 引用時の推奨表記

```text
Natural Area Planning / NAP-OEA-2026-08-16-v1 (2026).
Public Research Report:
「Operational Pilotで未解決だった運用入力を、固定T0でどこまで解消できるか
― provenance・時間・地理・authority条件を結果より先に固定したprospective evidence acquisition ―」
Version 1.0, 2026-08-24.
Formal outcome: PARTIAL_TARGET_INPUT_RESOLUTION.
Study snapshot: nkkmd/natural-area-planning @ 3788ba409956cc9806d0877a3bfa94e6fdd6258a.
```