# Natural Area Planning / G4-RTV-2026-08-14-v1 — Public Research Report

**source-localな管理応答はplanning contextへ転移できるか**  
**― 転移条件を結果より先に固定した結果、公開情報だけでは独立検証contextを確保できなかった ―**

- **レポート版:** v1.0
- **作成日:** 2026-08-14 JST
- **研究プロジェクト / Study ID:** `G4-RTV-2026-08-14-v1`
- **研究状態:** `COMPLETE / FROZEN`
- **formal outcome:** `INDETERMINATE`
- **formal outcome reason:** `NO_ELIGIBLE_INDEPENDENT_HELDOUT_RESPONSE_VALIDATION_CONTEXTS`
- **研究状態スナップショット commit:** `1e56878979d3163638768a7b769408a82c4b3629`
- **base main snapshot:** `2087e6c40108e991d586174b5619ad16dfd9176e`
- **本文の性格:** 外部公開用・単体完結型研究レポート
- **editorial state:** 本文作成は上記scientific-state snapshot後のpublication packagingであり、formal scientific stateを変更しない

> この文書は、元のGitHubリポジトリや内部artifactを参照しなくても、本研究の背景、問い、方法、主要結果、限界、再現性、解釈境界を理解できるように構成している。完全なprovenanceを確認する場合は末尾のrepository内主要資料を参照されたい。

---

# 要旨

Natural Area Planningの先行研究では、公開資料から管理と生態学的対象との関係を調査しても、**source contextで観察されたmanagement responseをplanning unitのresponseとして自動的に扱うことはできない**という境界が残った。NAP-002 Study 1では、この状態を`SUPPORTED_SOURCE_LOCAL`からG4 `DO_NOT_INFER`へ送るruleとして固定し、historical `transfer_to_zone_contribution=false`を維持していた。

本研究は、このG4 barrierそのものを扱う新しい独立prospective studyとして実施した。目的はG4をPASSさせることではなく、source-localなmanagement responseを別contextへ転移してよい条件と、転移判断を止める条件を**結果を見る前に定義し、再現可能に検証すること**であった。

研究開始前に、management definition、outcome definition、source/target context、duplicate source handling、transfer state、failure state、validation design、reproducibility firewallをfreezeした。8つのprimary relation familyを定義し、初期public-only evidence frameとして6つの外部候補sourceをprospectively固定した。うち5 sourceではprimary PDFを公式公開経路から取得してSHA-256をfreezeでき、1 sourceは公式publisher routeがHTTP 403となったため`ACQUISITION_INCOMPLETE`として扱った。

Phase 7では、外部sourceの**response方向をscoreする前に**provenance、独立性、management compatibility、outcome compatibility、mandatory context completenessを監査した。その結果は次のとおりである。

```text
external candidate sources                  = 6
public-primary sources hash-frozen          = 5
acquisition-incomplete sources              = 1
external source x relation rows             = 12
eligible held-out source x relation rows    = 0
eligible independent held-out clusters      = 0
structurally excluded rows                  = 12
held-out ecological response values read    = false
```

したがって、8つのprimary relation familyはすべて、事前に登録したstate vocabularyに従い、

```text
NO_INDEPENDENT_VALIDATION_EVIDENCE
```

として閉じた。`TRANSFER_NOT_SUPPORTED`とは判定していない。転移に失敗したのではなく、**転移の成否を独立に検証できるeligible contextが0だった**ためである。

study-level formal outcomeは、prospectively freezeしたPhase-8 ruleにより、

```text
INDETERMINATE
```

となった。

本研究は、公開情報だけでG4 transferabilityが成立することも、成立しないことも示していない。むしろ、management labelや場所の類似性だけではresponse transfer validationにならず、management vector、outcome construct、context domain、source independenceの整合を同時に確保する必要があること、そしてその条件を満たす独立public evidenceを今回のv1 frameでは確保できなかったことを明示した。

historical `transfer_to_zone_contribution=false`は変更していない。planning-unit ecological response、management recommendation、zone coefficient、optimizer input、human usability、practitioner benefitのいずれも承認していない。NAP-002 Study 2Bは引き続き`DEFERRED / NOT STARTED`である。

---

# 1. 研究の背景

## 1.1 先行研究で残ったG4 barrier

NAP-001 public-only Stage Cは、公開情報だけを用いて阿蘇半自然草原のmanagement-specific planningへ進める範囲を検証し、次のendpointで閉じた。

```text
Q3 planning-response features = 0
Q4 local response models = 0
zone contribution rows = 0
formal optimizer authorized = false
```

ここで重要だったのは、sourceで観察された管理応答が存在することと、planning unitでその応答が成立することは別の主張だという点である。

NAP-002 Study 1は、そのboundaryを壊さずにdecision supportへ翻訳した。G3でsource-grounded response evidenceが`SUPPORTED_SOURCE_LOCAL`になっても、G4では、

```text
SUPPORTED_SOURCE_LOCAL
-> DO_NOT_INFER + RESPONSE_TRANSFER_VALIDATION
```

と扱った。

つまり、

```text
source-local response
!=
planning-unit response
```

がprogram-level barrierとして残っていた。

## 1.2 なぜ独立研究が必要だったか

このbarrierを解消するには、既存Study 1のruleを結果に合わせて緩めるのではなく、**response transferabilityそのものを独立したprospective studyとして検証する**必要がある。

本研究は、NAP-001、NAP-002 Study 1、NAP-002 Study 2Aのformal endpointを変更・救済・再解釈するための研究ではない。

また、NAP-002 Study 2Bが扱うhuman usabilityとは科学的問いが異なる。

```text
human usability
!=
ecological response transferability
```

---

# 2. 研究対象 / study object

本研究が対象としたのは、**management response relationのcontext間transferability**である。

基本単位は、概念的には次の組合せとして定義した。

```text
source context
x source cluster
x management exposure definition
x comparator definition
x ecological outcome construct
x outcome target
x measurement definition
x observation window
x response precision
```

primary precisionは、

```text
DIRECTIONAL_RESPONSE
```

とした。

本研究のprimary objectではないものは次のとおりである。

```text
human usability
practitioner preference
workflow fit
management recommendation
operational feasibility
safety / permission
planning-unit optimization coefficient
```

---

# 3. 研究目的と中心的研究質問

中心的研究質問は、prospective protocolで次の趣旨として固定した。

> source contextで観察されたmanagement responseを、management definition、outcome definition、source/target contextの事前定義されたcompatibility条件のもとで、独立したheld-out contextへ同じprecisionで転移できるか。また、その条件を満たさない場合、どのnon-transfer / insufficient-evidence stateとして閉じるべきか。

結果stateは、単純なbinary `transferable / not transferable`ではなく、次を含む形で事前登録した。

```text
SUPPORTED_FOR_TRANSFER
CONDITIONAL_TRANSFER
TRANSFER_NOT_SUPPORTED
INSUFFICIENT_CONTEXT
INCOMPATIBLE_MANAGEMENT
INCOMPATIBLE_OUTCOME
NO_INDEPENDENT_VALIDATION_EVIDENCE
INDETERMINATE
```

negative、blocked、insufficient、indeterminateのいずれも正式な科学的結果になり得る設計とした。

---

# 4. 先行研究・novelty boundary

response transferability、external validity、generalizability、transportability自体は新しい概念ではない。

生態学ではWenger & Olden (2012)が、通常のrandom splitだけではなく、空間・時間・その他のdistinct groupをnon-randomにhold outすることで、別location・time period・datasetへのtransferabilityを評価する重要性を論じている。

因果推論ではBareinboim & Pearl (2013)が、heterogeneous source domainからtarget domainへeffectをtransportできる条件をformalに扱っている。Dahabreh et al. (2020)も、source populationからtarget populationへinferenceを拡張する際に、参加・target差やeffect modifierを明示的に扱う枠組みを示している。

より近年の生態学でも、Dumandan et al. (2024)はnovel biotic conditionsに対するecological forecasting modelのtransferabilityを長期実験で直接評価している。

したがって、本研究は`transferability`そのものを新規概念として主張しない。

本研究で独自に実装した対象固有の部分は、Natural Area Planningに残ったG4 barrierに対し、

```text
management-definition compatibility
x outcome-definition compatibility
x relation-specific context-domain rule
x source independence / duplicate firewall
x explicit non-transfer / insufficient-evidence states
x historical-endpoint firewall
```

を一体化したprospective gateとして実装し、public-only evidenceで実際にestimableかを検証した点にある。

---

# 5. 使用した情報 / 使用しなかった情報

## 5.1 historical frozen inputs

NAP-001 / NAP-002の既存artifactは、結果を書き換えず、

```text
historical frozen artifact
-> new G4 study input
```

として参照した。

主なhistorical inputには次が含まれる。

```text
analysis/nap001/t2_t4_management_response_transfer_evidence.csv
analysis/nap001/source_exposure_regime_crosswalk.csv
analysis/nap001/regime_feature_evidence_relations.csv
analysis/nap001/management_regimes.csv
analysis/nap001/conservation_features.csv
analysis/nap002/g3_evidence_class_definitions.csv
analysis/nap002/g2_g5_transformation_rules.csv
```

## 5.2 public-only external candidate frame

initial Phase-7 frameとして、次の6 sourceをdiscovery candidateとしてfreezeした。

| Source ID | DOI / identifier | 主な候補relation | Phase-7 provenance state |
|---|---|---|---|
| `NAP-T24-EXT-001` | `10.14941/grass.42.307` | T2/T4, artificial-pressure context | public primary PDF hash-frozen |
| `NAP-T24-EXT-002` | `10.14941/grass.53.28` | cutting / burning | public primary PDF hash-frozen |
| `G4-EXT-003` | `10.14941/grass.60.102` | burning / grazing / vegetation | public primary PDF hash-frozen |
| `G4-EXT-004` | `10.14941/grass.51.143` | managed vs abandoned grassland | public primary PDF hash-frozen |
| `G4-EXT-005` | `10.20848/kontyu.6.2_89` | *Shijimiaeoides divinus asonis* habitat/population | public primary PDF hash-frozen |
| `G4-EXT-006` | `10.1111/1440-1703.12494` | grazing / butterfly community | `ACQUISITION_INCOMPLETE_OFFICIAL_ROUTE_403` |

## 5.3 使用しなかった情報

本研究では次を使用していない。

```text
restricted/local non-public data
real-person participant data
Study 2B human-validation data
planning-unit direct ecological response panel
post-Phase-7 rescue source
excluded source response values for G4 scoring
LLM-generated ecological effect estimate
```

Phase 7でsourceがstructurally ineligibleになった後、そのsourceのresponse方向を見てeligibility ruleを緩めることも行っていない。

---

# 6. prospective governance / freeze

## 6.1 independent study identity

研究開始時に、

```text
Study ID = G4-RTV-2026-08-14-v1
Study type = prospective independent public-only response-transfer validation
```

としてNAP-001 / NAP-002とは独立したstudy identityを作成した。

## 6.2 結果より前に固定した事項

少なくとも次をheld-out response scoringより前にfreezeした。

- primary relation family
- management compatibility dimension
- outcome compatibility dimension
- context-dimension materiality
- source duplicate / independence handling
- public-primary provenance requirement
- missing-data rule
- transfer / non-transfer state vocabulary
- full-support minimum rule
- failure / insufficient-evidence rule
- target-context availability ceiling
- adversarial expected cases
- response-inspection firewall

## 6.3 support rule

full `SUPPORTED_FOR_TRANSFER`のproject-level minimum authorization ruleは、概ね次の形でfreezeした。

```text
>= 1 derivation/source context
>= 2 independent assessable held-out contexts
all assessable in-domain validation contexts concordant
any in-domain discordance blocks full support
mandatory management/outcome/context gates must pass
```

これは普遍的な生態法則としてのthresholdではなく、本studyでfull transfer authorizationを与えるための保守的なruleである。

## 6.4 duplicate firewall

publication countをreplication countに変換しないため、source clusterを事前に固定した。

例として、

```text
A-04 + A-05 + Murata/Nohara 2003
-> one conservative Aso Shijimiaeoides research-program cluster
```

とし、pre-response methods情報からindependenceを証明できない限り複数contextに数えないこととした。

---

# 7. 方法

## 7.1 8つのprimary relation family

formal studyは次の8 familyを対象とした。

| ID | management contrast | ecological outcome |
|---|---|---|
| `G4-RF-01` | active/continued management vs cessation/abandonment | T2 succession / composition |
| `G4-RF-02` | active/continued management vs cessation/abandonment | T4 open-grassland structure |
| `G4-RF-03` | mowing timing / frequency | T2 succession / composition |
| `G4-RF-04` | mowing timing / frequency | *Primula sieboldii* |
| `G4-RF-05` | mowing timing / frequency | ケルリソウ / *Cynoglossum asperrimum* |
| `G4-RF-06` | grazing intensity | *Shijimiaeoides divinus asonis* |
| `G4-RF-07` | grazing intensity | butterfly-community response |
| `G4-RF-08` | grazing intensity | rare-grassland-butterfly response |

## 7.2 management-definition compatibility

「同じ放牧」「同じ火入れ」「同じ刈取り」というlabelだけではPASSにしなかった。

relationに応じて、次をcritical dimensionとして扱った。

```text
management type
intensity
timing
frequency
duration / history
background burning
grazing background
mowing background
biomass removal
cessation / continuation state
livestock type
comparator definition
```

特にmowingでは、

```text
July
September
twice-yearly
biennial
```

をgeneric mowingへcollapseすることを禁止した。

A-04/A-05由来のgrazing intensityでも、binary `grazed`だけからLOW / CUSTOMARY / HIGHへ割り当てることを禁止した。

## 7.3 outcome compatibility

response variableについて、

```text
same construct?
same target?
same unit / measurement meaning?
same temporal scale?
same comparator orientation?
```

をrelation-specificに監査した。

代表的なhard ruleは次である。

```text
NDVI != direct T4 open-grassland structure
generic species richness != T2 succession/composition
focal butterfly species != butterfly community
occurrence != target-species population response
non-significance != zero / neutral / safe
```

## 7.4 context-domain compatibility

weighted global similarity scoreは使用しなかった。

代わりに、relationごとに必要なcontext dimensionを、

```text
MANDATORY_MATERIAL
OPTIONAL_DESCRIPTIVE
NOT_MATERIAL_FOR_THIS_RELATION
```

として事前固定した。

mandatory dimensionの例は次である。

```text
vegetation/ecological state
management history
background disturbance
observation window
climate/weather regime
target-species presence
phenology
host-plant context
nectar-resource context
landscape setting
```

mandatory contextが不明な場合、類似値をimputeせず`INSUFFICIENT_CONTEXT`とした。

## 7.5 target planning context

NAP-001 planning unitsにはP3 public GIS、P4 remote-sensing evidence-state等のpublic context情報が存在する。

しかし、G4 primary outcomeとcompatibleなplanning-unit direct ecological response panelは、v1開始時点でfrozen available evidenceとして存在しなかった。

したがって、

```text
planning-unit context similarity
!=
planning-unit response validation
```

とした。

## 7.6 Phase 7: pre-response eligibility audit

外部sourceを取得した後、response resultをscoreする前に、

1. provenance
2. duplicate / independence
3. management compatibility
4. outcome-definition compatibility
5. mandatory context completeness

を評価した。

eligible held-out contextになるには、少なくとも、

```text
public primary provenance frozen
management compatibility = PASS
outcome compatibility = PASS
context = IN_DOMAIN
source cluster = independent
```

をすべて満たす必要があった。

## 7.7 Phase 8: zero-validation rule

Phase 7でeligible independent contextが0になったため、excluded sourceのresponseを開いて救済するのではなく、結果materializationより前に次のruleをfreezeした。

```text
if eligible independent held-out context count == 0:
    relation_state = NO_INDEPENDENT_VALIDATION_EVIDENCE
    transfer_authorization = NOT_AUTHORIZED
```

さらに、8 relationすべてがこのstateなら、

```text
formalOutcome = INDETERMINATE
```

とした。

---

# 8. 主要結果

## 8.1 Phase 7 evidence eligibility

```text
external candidate sources                  = 6
public-primary sources hash-frozen          = 5
acquisition-incomplete sources              = 1
external source x relation rows             = 12
eligible held-out source x relation rows    = 0
eligible independent held-out clusters      = 0
structurally excluded rows                  = 12
```

## 8.2 structural exclusionの主な理由

Phase 7では、responseの良し悪しではなく、pre-response structureにより候補が除外された。

例を挙げると、次のような問題があった。

- Kantoのlong-term artificial-pressure studyはT2 compositionに関係するoutcome constructを持つ一方、RF01で必要なcritical background-management/historyおよびmandatory contextを十分に閉じられなかった。
- cutting-vs-burning studyは明示的なmanagement contrastが存在するが、RF01/RF02のactive-management-vs-cessation contrastそのものではなかった。
- Mt. Sanbe studyはdirect vegetation structureを測定していたためRF02 outcome constructには適合可能性があったが、full management-background vectorとmandatory contextをv1 ruleで閉じられなかった。
- central Japanのmanaged-vs-abandoned comparisonでは、継続側がfire + cutting + grazingのbundleであり、単一source management vectorのcontinuation/cessation comparisonとしては扱えなかった。
- Murata/Nohara 2003はA-04/A-05と同じAso *Shijimiaeoides* research-program clusterとして保守的に扱い、独立replicationへ数えなかった。
- `G4-EXT-006`は公式publisher routeでprimary sourceを再現可能に取得できず、`ACQUISITION_INCOMPLETE`となった。

## 8.3 relation-level formal result

```text
G4-RF-01  NO_INDEPENDENT_VALIDATION_EVIDENCE
G4-RF-02  NO_INDEPENDENT_VALIDATION_EVIDENCE
G4-RF-03  NO_INDEPENDENT_VALIDATION_EVIDENCE
G4-RF-04  NO_INDEPENDENT_VALIDATION_EVIDENCE
G4-RF-05  NO_INDEPENDENT_VALIDATION_EVIDENCE
G4-RF-06  NO_INDEPENDENT_VALIDATION_EVIDENCE
G4-RF-07  NO_INDEPENDENT_VALIDATION_EVIDENCE
G4-RF-08  NO_INDEPENDENT_VALIDATION_EVIDENCE
```

すべてのrelationで、

```text
transfer authorization = NOT_AUTHORIZED
```

となった。

## 8.4 study-level formal result

```text
formal outcome = INDETERMINATE
formal outcome reason = NO_ELIGIBLE_INDEPENDENT_HELDOUT_RESPONSE_VALIDATION_CONTEXTS
```

これはcompleted formal outcomeであり、unfinished analysisではない。

## 8.5 response scoringは実施していない

```text
held-out response values read for G4 scoring = false
ecological response concordance computed     = false
excluded-source response rescue              = false
```

したがって、本研究の`INDETERMINATE`は、transfer effect estimateの不確実性ではなく、**transfer validationを実行できるeligible independent evidenceが0だったこと**に由来する。

---

# 9. 本研究が支持すること

本研究は、少なくとも次を支持する。

1. source-local responseをplanning contextへ転移するには、management labelの一致だけでは不十分である。
2. management intensity、timing、frequency、duration/history、background management、comparator等を明示的に合わせる必要がある。
3. outcome targetとmeasurement constructも独立に合わせる必要がある。
4. context similarityをweighted scoreへまとめるだけでは、ecological response validationの代替にならない。
5. publication数を独立replication数へ変換してはいけない。
6. public provenance、management、outcome、context、independenceをすべて事前gateにすると、利用可能なvalidation evidenceが0になること自体があり得る。
7. その場合、`TRANSFER_NOT_SUPPORTED`ではなく`NO_INDEPENDENT_VALIDATION_EVIDENCE`として閉じる方が科学的に正確である。
8. `INDETERMINATE`は、事前ruleから得られた正式な完了結果になり得る。

---

# 10. 本研究が支持しないこと

本研究は次を支持しない。

```text
G4 transfer is supported
G4 transfer is not supported
planning-unit ecological response is known
source-local effect is zero outside source context
unknown response is neutral or safe
context similarity validates ecological response
candidate action is a recommendation
management-review scope is action priority
operational feasibility is ecological desirability
current safety/permission is response evidence
human usability has been validated
practitioner benefit has been demonstrated
Study 2B is complete
transfer_to_zone_contribution=true
zone contribution coefficient is available
formal optimizer input is available
```

特に、

```text
NO_INDEPENDENT_VALIDATION_EVIDENCE
!=
TRANSFER_NOT_SUPPORTED
```

である。

---

# 11. 実務的含意

本研究から得られる実務的含意は、特定管理を推奨することではない。

むしろ、外部研究のmanagement responseを現地planningへ持ち込む前に、最低限、

```text
何を管理したのか
どれくらいの強度か
いつ・何回・何年間か
同時に何が管理されていたか
何をresponseとして測ったのか
どの時間scaleで測ったのか
sourceとtargetで何がmaterially違うのか
sourceは本当に独立replicationか
```

を確認する必要があることを示している。

これらが不明な場合、数値を埋めてplanning modelを完成させるのではなく、

```text
transfer not authorized
```

と明示する方が適切である。

---

# 12. 研究上の限界

## 12.1 v1 source frameのcoverage ceiling

本研究の最も大きな限界は、初期にfreezeしたpublic-only source frameから、eligible independent held-out contextを1つも確保できなかったことである。

したがって、response transferabilityそのものを経験的にscoreできていない。

## 12.2 conservative ruleによるestimability低下

management/background/context ruleとduplicate handlingは意図的に保守的である。

このため、より緩い研究設計なら比較可能とみなすsourceも、本研究では除外され得る。

ただし、この保守性は結果を見て導入したものではなく、unsupported precisionを避けるために事前に設定した。

## 12.3 planning-unit direct responseの不足

Aso planning unitsにはpublic GISやremote-sensing contextはあるが、primary ecological responseとcompatibleなplanning-unit direct validation panelは凍結されたpublic inputとして存在しなかった。

したがって、external context間でtransferabilityが将来支持されたとしても、それだけで個々のplanning unit responseが確定するとは限らない。

## 12.4 acquisition limitation

1 sourceは公式publisher routeでHTTP 403となり、primary sourceをprospectively fixed rule下で取得できなかった。

非公式copyで穴埋めしなかったため、public-only evidence coverageはその分縮小した。

## 12.5 response-scoring validationを実行していない

eligible contextが0だったため、concordance test、sensitivity analysis、negative-control response scoring等のresponse-level validationは実行していない。

実行不能なvalidationを「実施済み」とは扱っていない。

## 12.6 broader literatureはv1 rescueに使用していない

Phase 7 closure後に追加文献が見つかり得ること自体は否定しない。

しかし、zero-eligibility resultを見た後にsource frameを広げるとpost-result rescueになるため、v1へ追加していない。

追加sourceを評価する場合は、別のprospective extension / successor stageが必要である。

---

# 13. 第三者資料・データの取扱い

本研究は公開primary sourceのidentity、DOI、publisher route、hash等を再現性情報として管理した。

第三者PDFそのものをrepositoryへ再配布することを研究成果の要件とはしていない。

公開reportにも第三者論文の長文転載、図表転載、個人情報、restricted dataは含めていない。

---

# 14. 後続研究との境界

## 14.1 G4 v1は閉鎖済み

G4 v1 source frameは閉じている。

```text
G4-RTV-2026-08-14-v1
COMPLETE / FROZEN
formal outcome = INDETERMINATE
```

追加sourceをv1へ入れて結果を救済してはいけない。

## 14.2 public-evidence extension

新しいpublic primary evidenceを評価する価値がある場合は、

```text
G4 extension / successor study
```

として、新しいsource frame、eligibility rule、duplicate rule、context ruleをresponseを見る前にfreezeする必要がある。

その研究がpositive resultを得ても、G4 v1のhistorical endpointは変更しない。

## 14.3 Study 2B

NAP-002 Study 2Bは引き続き、

```text
Practitioner / Administrative Decision-Support Validation
DEFERRED / NOT STARTED
```

である。

G4 v1の完了はhuman validationの完了を意味しない。

## 14.4 restricted/local data / prospective field evidence

public-only evidenceだけではmanagement historyやtarget responseを十分に閉じられない場合、restricted/local empirical dataやprospective ecological monitoringが将来的に必要となる可能性がある。

ただし、それらは新しいstudy/stageとして独立にgovernする必要がある。

---

# 15. 再現性・検証

## 15.1 Phase 7 closure

```text
passed = true
eligible held-out source x relation rows = 0
eligible independent clusters = 0
held-out response values read = false
```

## 15.2 formal-result validation

```text
passed = true
errors = []
relation rows validated = 8
formal outcome = INDETERMINATE
```

## 15.3 reproducibility audit

```text
passed = true
errors = []
recomputed relation states = 8 x NO_INDEPENDENT_VALIDATION_EVIDENCE
recomputed formal outcome = INDETERMINATE
```

## 15.4 adversarial closure audit

```text
passed = true
registered adversarial cases = 24
response-scoring adversarial execution performed = false
```

response-scoring adversarial pathを実行しなかったのは、eligible validation contextが0であり、response-scoring code自体が科学的に不要・未認可だったためである。

structural counterfactual testでは、

```text
eligibleCount = 0 -> NO_INDEPENDENT_VALIDATION_EVIDENCE
eligibleCount = 1 -> ABORT_ZERO_EVIDENCE_MATERIALIZER
eligibleCount = 2 -> ABORT_ZERO_EVIDENCE_MATERIALIZER
```

となることを確認した。

## 15.5 GitHub Actions

scientific-state snapshot commitに対するvalidation run:

```text
run ID = 31771397548
head   = 1e56878979d3163638768a7b769408a82c4b3629
study-validation job = PASS
external provenance re-acquisition job = PASS
```

artifact:

```text
g4-study-validation
  artifact ID = 9208252017
  digest = sha256:1dea8e5185b622b73e0295ffd9845f2ed060b82de58bc4cd49f272672dbb213c

g4-phase7-external-provenance
  artifact ID = 9208260068
  digest = sha256:ef6c577be3dfc66dd87dd51240d01555c31566d9aab247d5324cf1d7cda414cb
```

---

# 16. 再現性識別子

```text
Study ID:
G4-RTV-2026-08-14-v1

Scientific-state snapshot:
1e56878979d3163638768a7b769408a82c4b3629

Base main snapshot:
2087e6c40108e991d586174b5619ad16dfd9176e

Formal outcome:
INDETERMINATE

Formal outcome reason:
NO_ELIGIBLE_INDEPENDENT_HELDOUT_RESPONSE_VALIDATION_CONTEXTS

Primary relation families:
8

Eligible independent held-out clusters:
0

Formal relation-result SHA-256:
cf9143e376ec4ec7fb01b7b5f4bcf1310b88c5d80c4852a629c9da400a2d64d4

Formal result SHA-256:
2fef26ad26d1edb95cfbca1c2aae3f479072ff155d282f7e7244d5c159c90eac

Formal validation SHA-256:
77323b94aa46c85b3a8751a5b51cd1730fe4c17bb171884a9d4e133676116ae6

Reproducibility audit SHA-256:
1a9a8d8093a1f781ab162f70e30bf56355f976cb3f51a3f21b5b44d66ef11481

Adversarial closure audit SHA-256:
060c1393caf40970285757e894c92c5f37a0a4d7a7dab67aa09407ffe360c925
```

---

# 17. Repository内の主要資料

## Protocol / prospective freeze

- `doc/g4/G4_RESPONSE_TRANSFER_VALIDATION_PROSPECTIVE_PROTOCOL.md`
- `doc/g4/G4_PHASE7_EVIDENCE_DISCOVERY_FREEZE.md`
- `doc/checkpoints/2026-08-14-g4-response-transfer-prospective-design-freeze.md`
- `doc/checkpoints/2026-08-14-g4-phase7-acquisition-incomplete-exclusion-rule.md`
- `doc/checkpoints/2026-08-14-g4-phase8-zero-validation-result-rule-freeze.md`

## Phase 7 registries

- `analysis/g4/g4_relation_family_registry.csv`
- `analysis/g4/g4_evidence_identity_registry.csv`
- `analysis/g4/g4_source_cluster_registry.csv`
- `analysis/g4/g4_management_compatibility_rules.csv`
- `analysis/g4/g4_outcome_compatibility_rules.csv`
- `analysis/g4/g4_context_dimension_materiality_registry.csv`
- `analysis/g4/g4_target_context_availability.csv`
- `analysis/g4/g4_source_preanalysis_eligibility_registry.csv`
- `analysis/g4/g4_methods_only_audit_registry.csv`
- `analysis/g4/g4_external_evidence_acquisition_manifest.json`
- `analysis/g4/g4_phase7_closure_validation.json`

## Formal results / audits

- `analysis/g4/g4_phase8_result_rules.json`
- `analysis/g4/g4_formal_relation_results.csv`
- `analysis/g4/g4_formal_result.json`
- `analysis/g4/g4_formal_result_validation.json`
- `analysis/g4/g4_reproducibility_audit.json`
- `analysis/g4/g4_adversarial_closure_audit.json`
- `analysis/g4/g4_study_closure_registry.json`

## Implementations

- `analysis/g4/acquire_g4_external_evidence.py`
- `analysis/g4/validate_g4_preanalysis_package.py`
- `analysis/g4/materialize_g4_formal_result.py`
- `analysis/g4/validate_g4_formal_result.py`
- `analysis/g4/audit_g4_formal_closure.py`

## Checkpoints / status

- `doc/checkpoints/2026-08-14-g4-phase7-preanalysis-closure.md`
- `doc/checkpoints/2026-08-14-g4-response-transfer-formal-closure.md`
- `doc/g4/CURRENT_STATUS.md`

---

# 18. 結論

G4 Response-Transfer Validation Study v1は、source-localなmanagement responseをplanning contextへ転移してよい条件を、結果を見る前に定義して検証した。

しかし、prospectively frozen public-only evidence frameでは、provenance、source independence、management definition、outcome definition、mandatory contextをすべて満たす独立held-out contextを確保できなかった。

したがって、転移が成功したとも失敗したとも判定していない。

最終結果は、

```text
G4-RTV-2026-08-14-v1
COMPLETE / FROZEN
formal outcome = INDETERMINATE
reason = NO_ELIGIBLE_INDEPENDENT_HELDOUT_RESPONSE_VALIDATION_CONTEXTS
```

である。

本研究の中心的な成果は、G4をPASSさせたことではない。**「source-local responseをplanning responseへ変換する前に必要な比較可能性を明示し、その条件を満たす証拠がなければ、transferを推測せず正式に止める」**という再現可能な境界を実装したことにある。

historical `transfer_to_zone_contribution=false`は変更されず、planning-unit response、management recommendation、optimizer input、human validationのいずれも新たに承認されていない。

---

# 用語

**source-local response**  
ある研究・場所・管理条件・measurement contextで観察されたmanagement response。本研究では、そのままplanning-unit responseへ昇格させない。

**held-out context**  
derivation/source contextとは独立に、transferabilityを検証するためのcontext。

**eligible held-out context**  
public provenance、independence、management compatibility、outcome compatibility、mandatory contextのprospective gateを満たしたheld-out context。

**NO_INDEPENDENT_VALIDATION_EVIDENCE**  
独立検証contextが存在しないため、transfer success / failureをscoreできないrelation-level state。

**INDETERMINATE**  
本studyのformal study-level outcome。本研究では、8 relationすべてが`NO_INDEPENDENT_VALIDATION_EVIDENCE`となったために生じたcompleted result。

**transfer authorization**  
frozen rule下でsource responseを指定domainへ転移してよいかを示すauthorization state。本研究の全relationは`NOT_AUTHORIZED`である。

---

# Selected methodological references / prior art

1. Wenger, S. J., & Olden, J. D. (2012). *Assessing transferability of ecological models: an underappreciated aspect of statistical validation*. Methods in Ecology and Evolution, 3, 260–267. DOI: `10.1111/j.2041-210X.2011.00170.x`.
2. Bareinboim, E., & Pearl, J. (2013). *Meta-Transportability of Causal Effects: A Formal Approach*. Proceedings of the Sixteenth International Conference on Artificial Intelligence and Statistics, PMLR 31, 135–143.
3. Dahabreh, I. J., Robertson, S. E., Steingrimsson, J. A., Stuart, E. A., & Hernán, M. A. (2020). *Extending inferences from a randomized trial to a new target population*. Statistics in Medicine, 39, 1999–2014. DOI: `10.1002/sim.8426`.
4. Dumandan, P. K. T., Simonis, J. L., Yenni, G. M., Ernest, S. K. M., & White, E. P. (2024). *Transferability of ecological forecasting models to novel biotic conditions in a long-term experimental study*. Ecology, 105(11), e4406. DOI: `10.1002/ecy.4406`.

## G4 v1でprovenanceをfreezeした主要external sources

- Yamamoto et al. DOI: `10.14941/grass.42.307`.
- Yamamoto et al. DOI: `10.14941/grass.53.28`.
- Takahashi et al. DOI: `10.14941/grass.60.102`.
- Chen et al. DOI: `10.14941/grass.51.143`.
- Murata & Nohara. DOI: `10.20848/kontyu.6.2_89`.
- Nakahama et al. DOI: `10.1111/1440-1703.12494` — G4 v1ではofficial publisher route acquisition incompleteのためvalidation sourceとして使用していない。

---

# 引用時の推奨表記

> Natural Area Planning / G4-RTV-2026-08-14-v1 (2026). **Public Research Report: source-localな管理応答はplanning contextへ転移できるか ― 転移条件を結果より先に固定した結果、公開情報だけでは独立検証contextを確保できなかった ―**. Version 1.0, 2026-08-14. Formal outcome: INDETERMINATE. Study snapshot: nkkmd/natural-area-planning @ `1e56878979d3163638768a7b769408a82c4b3629`.