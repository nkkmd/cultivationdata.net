# Natural Area Planning / NAP-POERT-2026-08-24-v1 — Public Research Report

**公開オンライン情報による生態学的応答転移・局所生態証拠の探索と検証**  
**― independent ecological evidence, local applicability, and transferability boundary ―**

- レポート版: **v1.0**
- 作成日: **2026-08-25 JST**
- Study ID: `NAP-POERT-2026-08-24-v1`
- 研究状態: **COMPLETE / FROZEN**
- response-transfer formal outcome: **INDETERMINATE**
- local-applicability formal outcome: **PARTIALLY_SUPPORTED**
- overall formal outcome: **INDETERMINATE**
- scientific-state snapshot: `nkkmd/natural-area-planning @ dc1633205b84e6bf9409c9fb360278bee9cb840b`
- 本文の性格: **単体公開用研究レポート**

この文書は、元のGitHubリポジトリや内部artifactを参照しなくても、本研究の背景、研究質問、prospective design、使用した公開情報、主要結果、限界、再現性、解釈境界、および今後の研究課題を理解できるように構成している。

## 要旨

Natural Area Planningでは、先行するG4 Response-Transfer Validation Study v1が、凍結済みevidence universeとprospective ruleの下で**eligible independent held-out response-validation contextを0件しか確認できず、formal outcome `INDETERMINATE`**で完了した。この結果は「生態学的転移が失敗した」ことを意味せず、`NO_INDEPENDENT_VALIDATION_EVIDENCE != TRANSFER_NOT_SUPPORTED` がcontrolling boundaryである。

本研究はG4 v1を救済・再解析するのではなく、新しいprospective独立研究として、**インターネット上で一般公開された情報だけを用いて、阿蘇Natural Area Planningに関連する独立ecological response-transfer evidenceとlocal ecological evidenceをどこまで取得・検証できるか**を検討した。行政照会、管理主体への直接問い合わせ、現地確認、field measurement、private communication、restricted-access records、human participant interactionは使用していない。

研究開始前に、Track R（response transfer）とTrack L（local ecological evidence）を分離し、evidence class、検索語14系列、source group 4系列、eligibility、independence、held-out、management/outcome compatibility、spatial/temporal ceiling、duplicate rule、missingness rule、conflict rule、formal outcome vocabulary、stopping rule、24 adversarial casesをfreezeした。

Track Rでは、回収できた候補をresponse方向を見る前に構造監査した結果、`G4-RF-01`（continued/active management vs cessation/abandonment → grassland community composition/successional state）について、Mt. SanbeのTakahashi et al. 2014と関東Miscanthus草地のYamamoto et al. 1997の**2 independent held-out clusters**がeligibleとなった。response extraction後、2件とも凍結済み阿蘇source relationと方向が一致した。

```text
RF01 eligible independent held-out = 2
RF01 concordant = 2
RF01 discordant = 0
```

これは回収済みeligible setだけを見れば、事前固定したfull-support ruleの「件数」と「concordance」部分を満たす。しかしformal outcomeはsupportへ昇格させなかった。理由は、凍結した検索戦略が各query-family × service-groupについてfirst 100 unique records（100件未満なら全件）を要求した一方、現在のexecution interfaceではdeterministicなservice-native paginationを保証できず、**56/56 search cellsがterminalにはなったが、formal search depthは0/56しか満たせなかった**ためである。未走査tailにdiscordant、concordant、null、conflictingなeligible contextが残る可能性を排除できない。

したがってTrack R全体は `INDETERMINATE` とした。

Track Lでは、NAP-001で既にpublic-onlyかつhash-frozenされていた環境省「現存植生図2024」九州沖縄ブロックと193 planning unitsのoverlayをread-only evidenceとして独立監査した。その結果、193/193 planning unitsでspatial closure QAが成立しており、mapped vegetation compositionについて `PU_LINKED_STATE` を確認できた。mapped open semi-natural grassland candidateは14,551.751418 ha、planning-unit frameの62.286442%であった。ただしこの値は**mapped state**であり、management response、biodiversity score、target attainment、recommendationではない。植生図のcreation-year supportは2001・2007・2022にまたがり、「現存植生図2024」というproduct nameを共通観測年2024と解釈することも禁止した。

Track Lは、L-01 mapped vegetation composition/stateを `SUPPORTED_WITHIN_FROZEN_SCOPE`、その他のlocal state/context constructsを明示的ceiling付きで `PARTIALLY_SUPPORTED` とし、Track L全体を `PARTIALLY_SUPPORTED` とした。

さらに、RF05の凍結済みtarget identityについて重要な整合性問題を確認した。凍結G4/POERT relationは「ケルリソウ / `Cynoglossum asperrimum`」と記録していたが、元となるA03一次公開資料ではケルリソウを `Trigonotis radicans` として扱っている。結果を見た後のsilent taxon correctionはprospective integrityに反するため、本研究ではRF05を修正せず `NON_ESTIMABLE` とした。

最終結果は次のとおりである。

```text
responseTransferOutcome = INDETERMINATE
localApplicabilityOutcome = PARTIALLY_SUPPORTED
studyOutcome = INDETERMINATE
```

本研究の中心的貢献は、public-online informationだけでも阿蘇のplanning-unit-linked ecological state evidenceと、少なくとも一部relationについて独立response evidenceを取得できることを示した一方、**公開情報の存在とformal transfer validationの成立は同じではない**ことをprospectively明確化した点にある。

# 1. 研究の背景

## 1.1 先行研究から残った問題

G4 Response-Transfer Validation Study v1は、Natural Area Planningのsource-local ecological responseを、独立したheld-out contextsで再現できるかをprospectively検証した。しかし、その凍結済みevidence universeとrulesではeligible independent held-out contextsが0件であった。

G4 v1のformal endpointは現在も次のままである。

```text
G4 v1
  COMPLETE / FROZEN
  formal outcome = INDETERMINATE
  eligible independent held-out contexts = 0
```

この0件は、

```text
transfer failure
```

ではない。

正しい解釈は、

```text
under the frozen G4 v1 evidence universe and rules,
response-transfer validation could not be estimated
```

である。

またOperational Evidence Acquisition v1は、30 operational targetsのうち2件をresolveし、28件をunresolvedとして `PARTIAL_TARGET_INPUT_RESOLUTION` で閉じた。しかしoperational/current factとecological response evidenceは別種の証拠である。

## 1.2 本研究を独立研究とした理由

本研究で新しいpublic sourceが見つかっても、G4 v1に後付けして「G4 v1にもeligible contextがあった」と書き換えることはできない。そのため、検索universe、eligibility、source clustering、held-out definition、formal outcomesを新しくprospectively freezeする独立studyとして開始した。

# 2. 研究対象

本研究の対象は、阿蘇Natural Area Planningに関連する次の2種類のevidenceである。

```text
Track R = RESPONSE_TRANSFER
Track L = LOCAL_ECOLOGICAL_EVIDENCE
```

Track Rはmanagement exposureとecological responseの独立再現性を扱う。

Track Lは阿蘇のecological state、species/habitat state、management background、environmental context、spatial/temporal applicabilityを扱う。

両者は代替関係にない。

```text
Track L evidence != Track R validation
environmental similarity != response-transfer validation
remote-sensing similarity != management-response evidence
```

# 3. 研究目的と中心的研究質問

中心的研究質問は次のとおりである。

> prospectively frozenしたPUBLIC-ONLINE ONLY evidence universeのもとで、阿蘇Natural Area Planningに関連する管理―生態応答について独立したheld-out ecological response contextを取得・検証できるか。また、それとは独立に、阿蘇に対する局所生態学的state/applicability evidenceをどこまで取得できるか。その際、local state、environmental similarity、remote-sensing similarityをresponse-transfer validationの代用品とせず、それぞれの推論上限を明示できるか。

# 4. 先行研究とのnovelty boundary

本研究は、external validation、transportability、ecological transferability、publication-family clustering、response-blinded eligibilityといった一般的方法論を新規発明として主張しない。

本研究固有の検証対象は、既に阿蘇Natural Area Planningで凍結されていたmanagement × ecological-response relation familiesとlocal ecological constructsに対し、public-online evidenceだけでprospective gateをどこまで通過できるかである。

# 5. 使用した情報と使用しなかった情報

## 5.1 使用した情報

使用可能source classは、一般公開かつ個別許可・直接連絡・restricted credentialを不要とするものに限定した。

主なsource classは次のとおりである。

- peer-reviewed primary studies
- J-STAGE等のpublic scholarly records
- public institutional repositories
- government / municipal public documents
- NARO等のpublic research-result pages
- public GIS / geospatial data
- public remote-sensing products
- public biodiversity / vegetation / ecosystem information
- public scientific conference abstracts
- public APIs / catalog metadata where reproducible provenance could be retained

## 5.2 使用しなかった情報・手段

```text
行政照会
行政・管理主体への直接問い合わせ
現地確認
field visit
直接観測
field measurement
private communication
human participant interaction
restricted-access records
credential-gated local data
個別許可が必要な非公開資料
```

たとえば、関連性の高い論文でpublic full textが取得できず、ResearchGate等で著者へrequestする経路だけが存在した場合、そのrequestは行わなかった。

# 6. Prospective governance / design freeze

formal evidence acquisition前に次を固定した。

- Study ID
- Track R / Track L
- unit of analysis
- 7 evidence classes
- 8 response relation families
- 8 local constructs
- source classes
- 14 query families
- 4 service groups
- search depth
- stopping rule
- eligibility
- management/outcome compatibility
- independence / publication-family rule
- held-out rule
- spatial linkage state
- temporal validity
- missingness rule
- conflict rule
- formal outcome vocabulary
- 24 adversarial cases
- historical-study firewall

response方向を見た後のeligibility relaxationは禁止した。

# 7. 方法

## 7.1 Track Rの独立unit

独立unitは論文本数ではなく、

```text
relation family
× underlying study-site-context
× management-contrast cluster
× ecological-response construct
```

とした。

同じsite、experiment、sampling campaign、cohortを共有する複数publicationは原則1 clusterとした。

## 7.2 Response-blinded structural preanalysis

held-out responseを正式に読む前に、少なくとも以下を判定した。

```text
source identity
publication family / duplicate lineage
management exposure
comparator
response construct
measurement compatibility
mandatory context completeness
spatial domain
temporal window
independence
held-out eligibility
```

このstructural gateを通らないcandidateは、responseがsourceと一致しそうであってもvalidationへ進めなかった。

## 7.3 Track Lのspatial ceiling

spatial stateは次を区別した。

```text
PU_LINKED_STATE
ASO_LOCAL
REGIONAL_CONTEXT
EXTERNAL_CONTEXT
UNRESOLVED_SPATIAL_LINKAGE
```

`PU_LINKED_STATE`はplanning-unit geometryへreproducibly linkできるstateを意味するが、management responseではない。

## 7.4 Search strategy

14 query familiesを4 service groupsへ適用するmatrixをfreezeした。

```text
14 query families × 4 service groups = 56 frozen cells
```

検索停止はpositive evidence発見ではなく、すべてのcellがterminal stateになることとした。

# 8. 主要結果

## 8.1 Search closure

```text
frozen cells = 56
terminal cells = 56
formal-depth satisfied cells = 0
terminal state = ACCESS_ROUTE_UNAVAILABLE
```

現在のexecution interfaceでは、J-STAGE、CiNii Research、NDL Search、publisher/DOI route、official web、open-data catalog、general web等を用いたcandidate discoveryとsource resolutionは可能であった。一方、凍結した「service/queryごとfirst 100 unique records、100未満なら全件」という深度をdeterministically証明できるnative pagination routeを利用できなかった。

したがって、

```text
search stopping rule satisfied = true
search universe exhausted = false
```

である。

## 8.2 RF01 — succession/composition response

RF01は、active/continued management relative to cessation/abandonmentのcommunity composition/successional responseを対象とする。

新研究のstructural gateを通過した独立held-out clustersは次の2件であった。

1. Takahashi et al. 2014 — Mt. Sanbe Miscanthus-type semi-natural grassland
2. Yamamoto et al. 1997 — Kanto Miscanthus-type long-term artificial-pressure experiment

両contextについて、management/background、direct composition/successional construct、observation window、climate/domain情報等をresponse extraction前に監査した。

formal response extraction後は次のとおりであった。

```text
Aso frozen source direction = DECREASE
Sanbe held-out direction = DECREASE
Kanto held-out direction = DECREASE

eligible held-out = 2
concordant = 2
discordant = 0
```

ここで`DECREASE`は、frozen comparator orientationにおいてactive/continued management側でsuccessional advancementが小さいことを表す。

この回収済みsetはfull-support ruleの件数・concordance条件を満たす。しかし未走査tailを排除できないため、formal resultは `INDETERMINATE` とした。

## 8.3 RF02 — open-grassland structure

Sanbe contextの1 clusterがeligibleとなった。

sourceとheld-outのdirect structureは、height、dominance、biomass、litter等が一方向に揃う単一scalarではないため、weighted master scoreを作らず `MIXED_OR_NONMONOTONIC` とした。

```text
eligible held-out = 1
concordant = 1
formal outcome = INDETERMINATE
```

searchが完全であったとしても、1 held-out contextは凍結rule上full supportに達しない。

## 8.4 RF03 / RF04 / RF06 / RF07 / RF08

回収済みcandidate setではeligible held-out contextを確認できなかった。

ただし検索深度未達のため、これを

```text
NO_ELIGIBLE_INDEPENDENT_CONTEXTS
```

とはしない。

formal outcomeはすべて `INDETERMINATE` とした。

除外理由には、management timing/background mismatch、target taxon mismatch、publication-family overlap、same-site reuse、outcome incompatibility、mandatory context不足などが含まれた。

## 8.5 RF05 — target identity conflict

RF05ではresponse以前のtarget identityに問題が見つかった。

凍結済みrelation/outcome definition:

```text
ケルリソウ / Cynoglossum asperrimum
```

一次A03公開資料:

```text
ケルリソウ = Trigonotis radicans
```

current authoritative form:

```text
Trigonotis radicans var. radicans
```

exact target identityはRF05のcritical gateであり、研究開始後にsilent correctionすることはできない。

```text
RF05 formal outcome = NON_ESTIMABLE
```

とした。

## 8.6 Track L — planning-unit-linked ecological state

NAP-001でpublic-onlyにmaterialize済みの現存植生図overlayをread-only evidenceとして監査した。

```text
planning units = 193
planning units with mapped coverage = 193
planning units passing area-closure QA = 193
positive-area intersections = 4255
```

planning-unit frame内のmapped class summaryは次のとおりである。

```text
total planning-unit area                 23,362.630734 ha
OPEN_SEMINATURAL_GRASSLAND_CANDIDATE     14,551.751418 ha  62.286442%
OTHER_GRASSLAND_OR_MODIFIED_GRASSLAND     3,170.322441 ha  13.570058%
WOODY_OR_NON_GRASSLAND                    5,483.641581 ha  23.471850%
UNRESOLVED_OR_UNMAPPED                      156.915294 ha   0.671651%
```

creation-year support:

```text
2001
2007
2022
```

であり、same-date 2024 observationではない。

L-01 vegetation composition/stateはplanning-unit-linked mapped compositionとして `SUPPORTED_WITHIN_FROZEN_SCOPE` とした。

L-02はmapped open-grassland presence/stateまで確認できるが、height、woody fraction、litter等のdirect structureではないため `PARTIALLY_SUPPORTED` とした。

その他L-03〜L-08も、source-site、ASO_LOCAL、regional、station-reference、historical、proxy等のceilingを保持して `PARTIALLY_SUPPORTED` とした。

# 9. Formal results

## 9.1 Track R

| Relation | Eligible held-out scored | Recovered pattern | Formal outcome |
|---|---:|---|---|
| RF01 | 2 | 2 concordant / 0 discordant | `INDETERMINATE` |
| RF02 | 1 | 1 concordant | `INDETERMINATE` |
| RF03 | 0 | no eligible context identified in recovered set | `INDETERMINATE` |
| RF04 | 0 | no eligible context identified in recovered set | `INDETERMINATE` |
| RF05 | 0 | anchor target identity conflict | `NON_ESTIMABLE` |
| RF06 | 0 | no new independent compatible context identified | `INDETERMINATE` |
| RF07 | 0 | no new independent compatible context identified | `INDETERMINATE` |
| RF08 | 0 | no new independent compatible context identified | `INDETERMINATE` |

Track R:

```text
responseTransferOutcome = INDETERMINATE
```

## 9.2 Track L

| Construct | Highest observed linkage | Formal outcome |
|---|---|---|
| L-01 vegetation composition/state | `PU_LINKED_STATE` | `SUPPORTED_WITHIN_FROZEN_SCOPE` |
| L-02 open-grassland mapped state | `PU_LINKED_STATE` | `PARTIALLY_SUPPORTED` |
| L-03 focal species state | `ASO_LOCAL` | `PARTIALLY_SUPPORTED` |
| L-04 butterfly community state | `ASO_LOCAL` | `PARTIALLY_SUPPORTED` |
| L-05 rare grassland butterfly state | `ASO_LOCAL` | `PARTIALLY_SUPPORTED` |
| L-06 management/background context | `ASO_LOCAL` | `PARTIALLY_SUPPORTED` |
| L-07 environmental context | `ASO_LOCAL` | `PARTIALLY_SUPPORTED` |
| L-08 temporal context | `PU_LINKED_STATE` | `PARTIALLY_SUPPORTED` |

Track L:

```text
localApplicabilityOutcome = PARTIALLY_SUPPORTED
```

## 9.3 Study-level

```text
studyOutcome = INDETERMINATE
```

Track Lのpartial supportによってTrack Rのindeterminateを相殺していない。

# 10. 本研究が支持すること

本研究は、少なくとも次を支持する。

1. public-online evidenceだけでも、阿蘇Natural Area Planningの一部ecological stateをplanning-unit geometryへreproducibly linkできる。
2. response-transfer candidateは、publication数ではなくunderlying independent contextで評価可能である。
3. RF01では、回収できた2 independent held-out contextsがsource directionとconcordantであり、独立response evidenceが実際に存在することを確認できた。
4. local state evidenceとresponse-transfer evidenceを分離することで、remote sensingやenvironmental similarityを誤ってresponse validationへ昇格させずに利用できる。
5. historical anchor自体のtaxonomic identity問題を、positive/negative responseとは独立したintegrity findingとして検出できる。

# 11. 本研究が支持しないこと

本研究は以下を支持しない。

```text
RF01 transfer is formally proven
all relevant public evidence was exhausted
no discordant evidence exists
no eligible RF03/RF04/RF06/RF07/RF08 context exists
mapped vegetation class = management response
PU_LINKED_STATE = PU_MANAGEMENT_RESPONSE
environmental similarity = response validation
NDVI = direct open-grassland structure
local occurrence = management response
evidence found = management recommended
```

また、以下は一切生成していない。

```text
management recommendation
action ranking
optimization coefficient
planning-unit exact management response
ecological safety claim
permission claim
operational feasibility claim
human usability / practitioner acceptance claim
transfer_to_zone_contribution = true
```

# 12. 実務的含意

public-online evidenceのみでも、planning supportのうち**ecological state/context layerの充実**には実質的な前進が可能である。特にmapped vegetation compositionは、193 planning units全体へ再現可能にlinkできる。

一方、管理行為の効果をplanning-unit responseへ変換する段階は依然として別問題である。RF01で独立concordant evidenceが得られたことは科学的に重要だが、これをそのままmanagement coefficientやaction recommendationへ変換することはできない。

# 13. 研究上の限界

## 13.1 最大の限界 — search depth

最大のlimitationは、prospectively要求したsearch depthをcurrent execution interfaceで満たせなかったことである。

このlimitationはpositive resultを否定しないが、formal completeness claimを制限する。

## 13.2 Public-online availability

public bibliographic recordが存在してもprimary full textが公開されていない場合がある。本研究は著者へのrequestやcredential-gated routeを用いなかったため、そのsourceはformal response scoringに進めなかった。

## 13.3 Local evidenceの時間的不均一性

現存植生図はproduct nameが2024であっても、planning-unit内で利用されたcreation-year supportは2001、2007、2022である。したがってcurrent same-date ecological stateではない。

## 13.4 Species/current-state gap

focal species、butterfly community、rare butterfly assemblageについて、publicly available current planning-unit-level monitoring layerは確立していない。

## 13.5 RF05 taxonomic identity

RF05はanchor identityが不整合であり、v1内で修正不能である。

# 14. 第三者資料・データの取扱い

本研究は第三者論文・政府資料・公開GIS等をsourceとして利用したが、本レポートでは第三者著作物の本文・図表・restricted coordinatesを再配布しない。

rare speciesのsensitive exact locationsを推測・公開することも行っていない。

# 15. 後続研究との境界

後続研究として少なくとも次を保存する。

1. **Public-online response-transfer successor**  
   service-native pagination/API等により、凍結相当のsearch depthを実行できる環境で新規prospective studyとして再検証する。

2. **Trigonotis radicans var. radicans response study**  
   正しいtarget identityを研究開始前にfreezeした新studyとしてmowing responseを検証する。

3. **FUTURE / DEFERRED / PRESERVED routes**  
   行政照会、現地確認、direct observation/field measurement、適法なlocal/restricted records、human validation。

これらはすべて別study identityで実施し、本研究やG4 v1をretroactively rewriteしない。

# 16. 再現性・検証

formal closure validation:

```text
PASS
```

formal gates:

```text
PE0–PE10 = PASS within named scope
PE11 = PASS FOR CLOSURE WITH MATERIAL SEARCH-COVERAGE LIMITATION
```

adversarial audit:

```text
registered = 24
passed = 24
failed = 0
```

重要なadversarial checksには、duplicate publication inflation、same-site held-out inflation、management mismatch、proxy promotion、environmental-similarity promotion、regional→PU promotion、positive-result early stopping、post-result eligibility relaxation、historical endpoint rewrite、master-score collapse等を含む。

# 17. 再現性識別子

```text
Study ID:
  NAP-POERT-2026-08-24-v1

Base main:
  1656c54e505bd23838e5876981ade8ef1938db78

Scientific-state snapshot:
  dc1633205b84e6bf9409c9fb360278bee9cb840b

Formal result blob:
  ccfdd67a82019878a33f039ec21242a4704f8ea9

Planning-unit geometry SHA-256:
  46ef4dd475ec7645d78130722e8ddfcae9f51c6244ce95dc22ea9f7dcaa6609d

Frozen planning_unit_vegetation_composition.csv SHA-256:
  ac01c133cf8d366dc02d0da2b8e1ff1334e5b997f31b74553cd60f0afc4b5413

Frozen planning_unit_open_grassland_state.csv SHA-256:
  c6aee6d304e223189289e8b2f8bcbf5f42042b037304741365091eea77a4a93c

Search cells:
  56 terminal / 0 formal-depth-satisfied

Formal outcomes:
  responseTransferOutcome = INDETERMINATE
  localApplicabilityOutcome = PARTIALLY_SUPPORTED
  studyOutcome = INDETERMINATE
```

# 18. Repository内の主要資料

## Protocol / design

```text
doc/public-online-ecological-transfer/PUBLIC_ONLINE_ECOLOGICAL_RESPONSE_TRANSFER_PROSPECTIVE_PROTOCOL.md
analysis/public-online-ecological-transfer/study_spec.json
analysis/public-online-ecological-transfer/search_strategy.json
analysis/public-online-ecological-transfer/relation_anchor_registry.csv
analysis/public-online-ecological-transfer/local_construct_registry.csv
```

## Search / acquisition

```text
analysis/public-online-ecological-transfer/acquisition_start.json
analysis/public-online-ecological-transfer/search_execution_log.csv
analysis/public-online-ecological-transfer/search_matrix_terminalization.csv
analysis/public-online-ecological-transfer/search_service_access_audit.json
analysis/public-online-ecological-transfer/search_closure.json
```

## Results

```text
analysis/public-online-ecological-transfer/formal_result.json
analysis/public-online-ecological-transfer/formal_relation_results.csv
analysis/public-online-ecological-transfer/formal_local_construct_results.csv
analysis/public-online-ecological-transfer/response_extraction_batch5.csv
analysis/public-online-ecological-transfer/track_l_historical_spatial_inheritance_audit.json
analysis/public-online-ecological-transfer/track_l_conflict_audit.json
analysis/public-online-ecological-transfer/historical_anchor_identity_audit.json
```

## Validation

```text
analysis/public-online-ecological-transfer/formal_validation.json
analysis/public-online-ecological-transfer/adversarial_expected_cases.csv
analysis/public-online-ecological-transfer/adversarial_closure_audit.json
analysis/public-online-ecological-transfer/validate_formal_closure.py
```

## Checkpoints

```text
doc/checkpoints/2026-08-24-public-online-ecological-transfer-design-freeze.md
doc/checkpoints/2026-08-25-public-online-ecological-transfer-acquisition-start.md
doc/checkpoints/2026-08-25-public-online-ecological-transfer-formal-closure.md
```

# 19. 結論

本研究の問いに対する回答は、**public-online evidenceだけでも相当量の局所生態証拠と一部の独立response evidenceへ到達できるが、今回凍結したformal response-transfer validationを完遂するには不十分だった**、である。

局所生態証拠については、193 planning unitsへlinkされたmapped vegetation composition/stateを確認でき、Track Lを `PARTIALLY_SUPPORTED` とするだけの実質的成果が得られた。

response transferについては、RF01で2 independent held-out contextsが両方concordantという重要な支持パターンを得た。しかし、prospectively要求したsearch depthを満たせなかった以上、その結果をformal full supportへ昇格させることはできない。

したがって最終結論は、

```text
responseTransferOutcome = INDETERMINATE
localApplicabilityOutcome = PARTIALLY_SUPPORTED
studyOutcome = INDETERMINATE
```

である。

この`INDETERMINATE`は、transfer failureを意味しない。またRF01の支持的観測を消すものでもない。意味するのは、**回収できた独立証拠は有望である一方、凍結したformal evidence universeを十分な深度で閉じられなかったため、formal transfer conclusionを確定しない**ということである。

## 用語

**PUBLIC-ONLINE ONLY**  
一般に公開され、個別許可・直接連絡・restricted credentialを必要とせずアクセスできる情報だけをformal evidence universeとする方針。

**held-out context**  
source/derivation contextとはunderlying data lineageが独立し、management・outcome・mandatory context gateをprospectively通過したvalidation context。

**PU_LINKED_STATE**  
frozen planning-unit geometryへ再現可能にlinkされたecological state。management responseを意味しない。

**INDETERMINATE**  
positive/negativeを意味せず、凍結ruleの下でformal conclusionを確定できない完了状態。

**NON_ESTIMABLE**  
必要なestimand/target identity等が研究の凍結定義上成立せず、そのrelationを適法に推定できない状態。

## Selected methodological references / prior art

- Wenger, S. J. & Olden, J. D. (2012). Assessing transferability of ecological models across space and time. *Methods in Ecology and Evolution*.
- Bareinboim, E. & Pearl, J. (2013). A general algorithm for deciding transportability of experimental results. *JMLR Workshop and Conference Proceedings / PMLR*.
- Dahabreh, I. J. et al. (2020). Methods for transporting trial results to target populations. *Statistics in Medicine*.
- Yamamoto, Y. et al. (2002). 阿蘇地域の半自然草地における火入れ中止にともなう植生の変化. *日本草地学会誌* 48(5):416–420. DOI `10.14941/grass.48.416`.
- Yamamoto, K. et al. (1997). Ordination of Vegetation of Miscanthus-type Grassland under the Some Artificial Pressure. DOI `10.14941/grass.42.307`.
- Takahashi et al. (2014). Effect of Cattle Grazing Associated with Burning on Vegetation and Species Diversity in Miscanthus-type Grassland at the Foot of Mount Sanbe. DOI `10.14941/grass.60.102`.
- Yasunaka et al. (2015). Assessing the effect of controlled burning and grazing on vegetation change in the grasslands of Aso region using satellite image analyses. DOI `10.14962/jass.31.4_117`.
- Ministry of the Environment Biodiversity Center. 現存植生図2024 — 九州沖縄ブロック.

## 引用時の推奨表記

```text
Natural Area Planning / NAP-POERT-2026-08-24-v1 (2026).
Public Research Report:
「公開オンライン情報による生態学的応答転移・局所生態証拠の探索と検証」.
Version 1.0, 2026-08-25.
Formal outcome: INDETERMINATE
(response transfer: INDETERMINATE;
 local applicability: PARTIALLY_SUPPORTED).
Study snapshot: nkkmd/natural-area-planning @ dc1633205b84e6bf9409c9fb360278bee9cb840b.
```