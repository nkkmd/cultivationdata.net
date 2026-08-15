# Natural Area Planning / NAP-OP-2026-08-15-v1 — Public Research Report

**local/current operational factsは意思決定支援workflowへ安全に接続できるか**  
**― 30 cases・192 required inputsをprospective ruleで処理し、利用可能情報が0件でも未知・未検証を正しくblockできることを検証した ―**

- **レポート版:** v1.0
- **作成日:** 2026-08-15 JST
- **研究プロジェクト / Study ID:** `NAP-OP-2026-08-15-v1`
- **研究状態:** `COMPLETE / FROZEN`
- **formal outcome:** `WORKFLOW_VALIDATED_WITHIN_PILOT_SCOPE`
- **研究状態スナップショット commit:** `f1a1f63ed76e960da1355def19bd823027d29aa6`
- **base main snapshot:** `d3c930293de6fd0126b58233d706075f371c9726`
- **common decision-time reference:** `T0 = 2026-08-15T11:13:35+09:00`
- **本文の性格:** 外部公開用・単体完結型研究レポート
- **editorial state:** 本文作成とclosure packagingは上記scientific-state snapshot後に行われ、formal scientific stateを変更しない

> この文書は、元のGitHubリポジトリや内部artifactを参照しなくても、本研究の背景、研究質問、方法、主要結果、限界、再現性、解釈境界を理解できるように構成している。完全なprovenanceを追う場合は末尾のrepository内主要資料を参照されたい。

---

# 要旨

Natural Area Planningの先行研究では、公開情報から生態学的evidence boundaryとdecision-support representationを構築できても、実際の管理判断に進むには、**その時点・その場所・その候補行為についてのlocal/current operational facts**が別途必要になることが明らかになっていた。たとえば、候補行為の具体的仕様、現地へのアクセス、使用できるインフラ、人員や機材、管理権限、現地状態、気象、燃料・バイオマス状態、安全体制、許可・規制である。

しかし、こうした情報をworkflowへ追加する際には新しい危険が生じる。情報が見つからないときに`UNKNOWN`を「問題なし」と扱ったり、古い公式情報を`CURRENT`として再利用したり、行政区域内にあることを許可と読み替えたり、気象や安全情報を生態学的な好ましさへ転換したりすれば、既存研究で守ってきたevidence boundaryを壊すことになる。

そこで本研究は、完了済みのNAP-001 public-only Stage C、NAP-002 Study 1、NAP-002 Study 2A、G4 Response-Transfer Validation Study v1を変更しない新しい独立prospective pilotとして、次を検証した。

> planning unit × candidate action × decision-time snapshotを単位として、local/current operational factsを、source identity、authority、geographic scope、effective/phenomenon time、freshness/expiryを明示したまま取得・分類し、`UNKNOWN`、`UNAVAILABLE`、`NOT_VERIFIED`、`STALE`等を補完せず、既存のevidence-constrained decision-support workflowへ再現可能に接続できるか。

結果を見る前に、10のoperational domain、provenance rule、temporal-validity rule、state vocabulary、blocker semantics、30-case pilot frame、25のadversarial case、formal validation gate OP1–OP9を固定した。historical planning-unit geometryはSHA-256でexact rehydrationし、2026年1月1日時点の国土数値情報「行政区域」熊本県データをoverlayして市町村jurisdictionを固定した。その結果、planning unit 0・2・5・8は阿蘇市のみ、1・3は阿蘇市と産山村の双方に跨るため、少数面積側を切り捨てずmulti-jurisdictionとして保持した。

source routeとfreshness ruleを先にfreezeした後、共通decision timeとして、

```text
T0 = 2026-08-15T11:13:35+09:00
```

を固定し、formal acquisitionを実施した。阿蘇市、産山村、熊本県阿蘇地域振興局、環境省阿蘇くじゅう国立公園管理事務所、気象庁等の公式source routeを確認したが、今回のpublic-source / non-participant実行では、選定されたplanning unit × candidate actionに対して、authority、geographic specificity、temporal validityを同時に満たすcase-specific usable operational factは得られなかった。direct field observationも実施していない。

そのためformal snapshotは次の結果となった。

```text
pilot cases                              = 30
operational domains                      = 10
case x domain rows                       = 300
required operational-input rows          = 192
not-required rows                        = 108
usable required inputs                   = 0 / 192
blocked required inputs                  = 192 / 192
operational-workflow-ready cases         = 0 / 30

required availability:
  AVAILABLE                              = 0
  UNKNOWN                                = 156
  UNAVAILABLE                            = 36

required blockers:
  BLOCKED_BY_MISSING_INFORMATION         = 114
  BLOCKED_BY_VERIFICATION                = 42
  BLOCKED_BY_UNAVAILABLE_INFORMATION     = 36
```

一見すると「利用可能情報が0件」であるためnegative resultのように見える。しかし、本研究のformal endpointは「好条件が見つかったか」ではない。prospective protocolでは、`UNKNOWN`や`UNAVAILABLE`も正式な観測状態であり、それらを**false / safe / feasible / approved / no constraintへ変換せず、正しいblockerとしてworkflowへ伝播できること**自体を検証対象とした。

formal CIでは、complete schema、provenance、temporal validity、state transition、blocker semantics、workflow connection、historical/ecological firewall、reproducibility、25 adversarial casesを評価するOP1–OP9がすべてPASSした。独立2回のmaterializationもbyte-identicalであった。

したがってformal outcomeは、事前に固定したruleに従い、

```text
WORKFLOW_VALIDATED_WITHIN_PILOT_SCOPE
```

となった。

ただし、このpositive workflow resultは、**どの候補行為についてもoperational feasibilityを示していない**。0/30 casesがoperational-workflow-readyであり、0/192 required inputsがusableだったからである。また、permission、safety、ecological desirability、ecological response、management effectiveness、recommendation、ranking、human usability、practitioner acceptanceのいずれも実証していない。

G4 Response-Transfer v1は`COMPLETE / FROZEN / INDETERMINATE`のままであり、8 relation familyの`NO_INDEPENDENT_VALIDATION_EVIDENCE`も変更しない。historical `transfer_to_zone_contribution=false`も維持される。NAP-002 Study 2Bは引き続き`DEFERRED / NOT STARTED`、human validationは`NOT PERFORMED`である。

---

# 1. 研究の背景

## 1.1 Natural Area Planningで既に解決したこと

Natural Area Planningでは、公開情報だけを用いたplanning researchを、evidenceの強さ以上に進めないことを基本原則としている。

NAP-001 public-only Stage Cでは、公開情報から得られるplanning evidenceを整理した一方、planning-unit-specificなmanagement response modelやoptimizer coefficientまで進むことは承認しなかった。

NAP-002 Study 1では、そのevidence boundaryを保持したまま、

```text
WHERE      どこをreview scopeとして見るか
WHY        何が判断を止めているか
WHAT NEXT  次に何の情報が必要か
```

を表現するdecision-support workflowへ変換した。

NAP-002 Study 2Aでは、人間参加者を用いず、traceability、evidence-boundary cue、baseline equivalence、adversarial consistency、public-document compatibilityをpre-validationした。ただしformal outcomeは`INDETERMINATE`であり、human usabilityは検証していない。

G4 Response-Transfer Validation Study v1では、source-local management responseを別contextへ転移できる条件をprospectively検証したが、eligible independent held-out contextが0だったため`INDETERMINATE`で閉じた。

## 1.2 それでも実際の判断に不足するもの

これらの研究が揃っても、実際の候補行為を検討する時点では、歴史的・生態学的evidenceとは別に、次のようなoperational factsが必要になる。

```text
候補行為の具体的な実施仕様
現地へのアクセス可否
必要インフラの存在・利用可否
誰が管理・許可・実施権限を持つか
人員・機材・予算等のresource capacity
現在の現地状態
現在の気象条件
現在の燃料・バイオマス状態
現在の安全体制
現在の許可・規制
```

これらは「現時点で何が実行可能か」を考えるための情報であり、生態学的なmanagement response evidenceとは異なる。

## 1.3 operational factsを足すだけでは危険な理由

operational dataは、古くなる、取得できない、地域粒度が粗い、管轄が重複する、source authorityが異なる、といった特徴を持つ。

そのため、単に「検索して見つかった値」をworkflowへ追加すると、次の誤りが起こり得る。

```text
UNKNOWN -> 問題なし
UNAVAILABLE -> 制約なし
古い公式情報 -> CURRENT
retrieval time -> effective time
市町村内に位置する -> 市町村が管理者
一般的な許可制度 -> 個別ケースが許可済み
一般的な野焼き日程 -> 選定planning unitの実施仕様
最寄り気象観測所 -> planning unitそのものの気象
現在の安全体制 -> 生態学的に安全
operational feasibility -> ecological desirability
```

本研究は、この誤変換を防ぎつつlocal/current factsをworkflowへ接続できるかを独立に検証した。

---

# 2. 研究対象 / study object

本研究のstudy objectは、**local/current operational factsを扱うdecision-support workflow**である。

formal operational unitは次の三者積として固定した。

```text
planning_unit_id
× candidate_action_id
× decision_time_snapshot
```

今回のprimary pilot frameでは、6 planning unitsと5 representative candidate actionsを用いた。

planning unit:

```text
[0, 1, 2, 3, 5, 8]
```

representative candidate actions:

```text
CR_BURN_ONLY
CR_MOW_JULY
CR_BURN_GRAZE_INTENSITY_UNRESOLVED
CR_GRAZE_WITHOUT_BURN_INTENSITY_UNRESOLVED
CR_ACTIVE_MANAGEMENT_UNRESOLVED
```

これらは候補行為であり、recommendationやpriorityではない。

本研究のprimary objectではないものは次のとおりである。

```text
ecological response validation
management effectiveness
biodiversity benefit
management recommendation
action ranking / prioritization
optimizer execution
human usability
practitioner acceptance
human participant research
```

---

# 3. 研究目的と中心的研究質問

中心的研究質問は次の趣旨でprospectively固定した。

> 実際のdecision timeにおいて、planning unit × candidate actionに必要なlocal/current operational factsを、source provenance、authority、geographic scope、effective/phenomenon time、freshness/expiryを明示したまま取得・分類し、unknown / unavailable / not verified / stale等を補完せず、既存のevidence-constrained workflowへ再現可能に接続できるか。

formal outcome vocabularyは、少なくとも次を区別する設計とした。

```text
WORKFLOW_VALIDATED_WITHIN_PILOT_SCOPE
WORKFLOW_NOT_VALIDATED
INDETERMINATE
```

重要なのは、`WORKFLOW_VALIDATED_WITHIN_PILOT_SCOPE`が「候補行為が実行可能だった」を意味しないことである。

---

# 4. 先行研究・novelty boundary

provenance、temporal validity、uncertainty handling、structured decision making、adaptive management、operational readiness、situational awareness等は既存の方法論・実務概念である。

たとえばW3C PROVは、entity・activity・agent等の関係を用いてprovenanceを表現する標準的枠組みを提供する。OGC Observations, Measurements and Samplesはobservationのphenomenon time / result time / feature of interest等を区別する。ISO 19157-1は地理空間data qualityの一般原則を扱う。USGSのstructured decision making / adaptive managementの文献も、不確実性を明示しながらdecision processを構成する一般的枠組みを提供している。

したがって、本研究はこれらの一般概念自体を新規発明として主張しない。

本研究で対象固有に実装・検証したのは、Natural Area Planningの既存evidence-boundary workflowに対し、

```text
local/current operational facts
× source authority / provenance
× geographic scope
× temporal validity / expiry
× explicit UNKNOWN / UNAVAILABLE / NOT_VERIFIED states
× blocker semantics
× historical/ecological firewall
```

を一体化し、「値が取れない場合でも誤推論せずにworkflowを閉じられるか」をformal pilotとして検証した点である。

---

# 5. 使用した情報 / 使用しなかった情報

## 5.1 使用したhistorical frozen inputs

case frameとrequired-input structureの定義には、NAP-002 Study 1の凍結済みartifactをhistorical inputとして参照した。

これらは再解析してendpointを変更するためではなく、

```text
historical frozen decision-support state
-> new prospective operational pilot input
```

として用いた。

## 5.2 exact planning-unit geometry

location-dependent acquisitionの前に、historical planning-unit geometryをexact rehydrationした。

```text
file = p001_p4_aso_pastures_193.geojson
byte size = 1,619,871
SHA-256 = 46ef4dd475ec7645d78130722e8ddfcae9f51c6244ce95dc22ea9f7dcaa6609d
features = 193
ID field = pu_id
```

raw-byte hashがhistorical frozen identityと一致することを確認してから構造QAを行った。

## 5.3 行政区域data

市町村jurisdictionを推測で割り当てないため、国土交通省「国土数値情報（行政区域）」の2026年1月1日時点・熊本県archiveを用いた。

```text
file = N03-20260101_43_GML.zip
byte size = 9,780,499
SHA-256 = e38cdc6102af404f05c2bac5f5219858c3c1d401fb9ff0acf410edd70ae88bd0
snapshot date = 2026-01-01
```

## 5.4 formal source routes

post-T0 formal acquisitionでは、主に次のofficial source routeを使用した。

```text
阿蘇市 経済部 農政課
阿蘇市 土木部 建設課
阿蘇市 総務部 危機管理防災課
阿蘇市 土木部 住環境課
阿蘇市 農業委員会事務局
産山村 経済建設課
産山村 農業委員会
熊本県 阿蘇地域振興局 農林部
熊本県 阿蘇地域振興局 土木部
環境省 阿蘇くじゅう国立公園管理事務所
気象庁 地域気象観測システム（アメダス）
```

## 5.5 使用しなかった情報

本study v1では次を使用していない。

```text
human participant data
practitioner interview / survey
未承認restricted local record
private pasture-manager records
protocol-compliant field observation
post-hoc favorable source substitution
pre-T0に偶発的に見えたoperational values
```

pre-T0 source discovery中に検索snippetから一部のoperational schedule/statusやmeteorological valueが見えたが、すべて`QUARANTINED_NOT_FORMAL_DATA`としてformal acquisitionから除外し、T0後に同一sourceを利用する場合もde novo retrievalを要求した。

---

# 6. prospective governance / freeze

結果を見る前に、次をfreezeした。

```text
study identity
scientific question
scope / non-scope
pilot population
operational unit
10 operational domains
source provenance rule
source authority ceiling
geographic-scope rule
retrieval/effective/phenomenon-time distinction
freshness / expiry rule
state vocabulary
blocker semantics
pilot case frame
workflow output
success / failure criteria
25 adversarial cases
reproducibility firewall
historical/ecological firewall
```

特に、次のstateを別軸で保持した。

```text
availability
verification
temporal validity
workflow use
blocker
```

単一の「OK / NG」へ潰さないことで、`UNKNOWN`と`UNAVAILABLE`、`NOT_VERIFIED`、`STALE`等を区別できるようにした。

---

# 7. operational domain

10 domainは次のとおりである。

| Domain | 趣旨 |
|---|---|
| `LOCAL_ACTION_SPECIFICATION` | 候補行為の具体的な実施仕様 |
| `LOCAL_ACCESS_FEASIBILITY` | 現地への到達・道路・route条件 |
| `LOCAL_INFRASTRUCTURE_FEASIBILITY` | 必要asset / facility / infrastructure |
| `LOCAL_AUTHORITY_FEASIBILITY` | land/management/action authority |
| `LOCAL_RESOURCE_CAPACITY` | 人員・機材・予算等のcapacity |
| `CURRENT_SITE_CONDITION` | decision time近傍の現地状態 |
| `CURRENT_WEATHER_CONDITION` | decision/action timeの気象 |
| `CURRENT_FUEL_OR_BIOMASS_CONDITION` | 燃料・バイオマス状態 |
| `CURRENT_SAFETY_ARRANGEMENT` | operation-specific safety arrangement |
| `CURRENT_PERMISSION_OR_RESTRICTION` | 現在の許可・規制・制約 |

これらはすべてoperational domainであり、生態学的response evidenceではない。

---

# 8. geometryとjurisdictionの固定

## 8.1 なぜadministrative overlayが必要だったか

planning unitがどの自治体に位置するかを中心点だけで割り当てると、行政界を跨ぐplanning unitのminority areaを消してしまう。permission、road authority、local ordinance等では少数面積側のjurisdictionも重要になり得る。

そこで、exact planning-unit polygonとMLIT administrative polygonをarea intersectionし、**positive-area intersectionをすべて保持**した。

## 8.2 結果

```text
PU 0: 阿蘇市                       100%
PU 1: 阿蘇市 99.1456% + 産山村 0.8544%
PU 2: 阿蘇市                       100%
PU 3: 阿蘇市 99.4090% + 産山村 0.5910%
PU 5: 阿蘇市                       100%
PU 8: 阿蘇市                       100%
```

したがって、

```text
single-municipality units = [0,2,5,8]
multi-municipality units  = [1,3]
```

と固定した。

ここから導けるのは「市町村地理範囲が重なる」という事実だけである。

```text
municipal containment
!= land ownership
!= pasture management authority
!= permission
```

---

# 9. provenance / authority rule

formal operational factとして使うsourceには、可能な限り次を要求した。

```text
source identity
source URI / identifier
source class
access level
authority status
issuing / owning authority
retrieval timestamp
effective / phenomenon timestamp
result / publication timestamp
validity start
validity end / expiry condition
geographic scope
verification state
snapshot / hash method
normalization provenance
```

source classの強さは、情報の秘密性ではなく、**そのoperational factに対するauthorityとscope**で決まる。

したがって、

```text
restricted source
!= intrinsically stronger scientific evidence
```

とした。

同じunderlying sourceの複製は独立verificationとして数えない。equally controlling sourceが衝突し、freeze済みprecedenceで解消できなければ`CONFLICTING_SOURCES`とし、都合の良いsourceを選択しない。

---

# 10. temporal-validity rule

local/current factsでは、「いつ取得したか」と「いつの状態を表すか」を分離した。

```text
retrieval time
!= effective / phenomenon time
!= result / publication time
```

retrieval timestampをeffective timeの代用にすることは禁止した。

また、全domain共通の便利なTTLを後付けせず、source-stated validityを優先し、必要なdomainにのみprospective freshness windowを設定した。

例:

```text
current site direct observation        <= 6 h before T0
current weather observation            <= 1 h before T0
fuel moisture / combustibility         <= 3 h before T0
standing biomass / height / cover      <= 24 h before T0
infrastructure direct observation      <= 24 h before T0
```

sourceが明示するvalidity intervalがある場合はそれを優先した。validityを確認できない場合は`VALIDITY_UNKNOWN`とし、last-known valueを自動的にCURRENTへ保持しない。

---

# 11. common decision time T0

source routeとfreshness ruleのfreezeがCIでPASSし、残る唯一のpre-execution blockerが`COMMON_T0_NOT_FROZEN`になったことを確認してから、

```text
T0 = 2026-08-15T11:13:35+09:00
```

を固定した。

T0固定後のpre-execution validationは、

```text
designPackagePassed = true
pilotExecutionAuthorized = true
localCurrentDataAcquisitionAuthorized = true
executionBlockers = []
```

となり、その後にのみformal value acquisitionを開始した。

---

# 12. formal acquisition

formal acquisitionでは、pre-T0に偶発的に見えた値を再利用せず、freeze済みsource routeからpost-T0にde novo retrievalした。

しかし、official pageが存在することと、selected planning unit × actionのformal operational factが得られることは別である。

代表例は次のとおりである。

## 12.1 一斉野焼き情報

阿蘇市のofficial noticeは地域の野焼き実施に関する重要な行政情報であるが、牧野組合ごとに実施時刻等が異なり、選定planning unit × candidate actionの具体的なoperation specification、manager、permit、安全体制を直接確定するものではなかった。

したがって、一般情報をselected caseへlocalizeしなかった。

## 12.2 road / access

阿蘇市・熊本県にはofficial road-information routeがある。しかしpilot geometryだけから、各planning unitへのactual approach routeとそのcontrolling road authorityをprospectively確定できなかった。

したがって、近隣道路が通行可能そうであることを`LOCAL_ACCESS_FEASIBILITY=AVAILABLE`へ変換しなかった。

## 12.3 permission

阿蘇市には各種permission / consultation routeがあり、産山村には火入れに関する条例がある。しかし、一般的な制度・条例が存在することはselected planning-unit/actionが許可済みであることを意味しない。

産山村の火入れ制度では、火入地、地図、所有者・管理者の承諾等、個別案件に結び付く情報が必要になる。今回のformal acquisitionではselected caseに対応するpermit recordやverified land/manager identityを取得していない。

したがって、

```text
public no-record
!= approval
!= confirmed absence of restriction
```

を維持した。

## 12.4 national park applicability

環境省のofficial routeは確認したが、公開area mapだけでは最新拡張を含むexact boundaryを判断できないという制約があり、selected planning unitごとのNatural Parks Act applicabilityを地図の見た目から推定しなかった。

## 12.5 weather

気象庁AMeDASはofficial primary observation sourceである。しかし、station observationをplanning-unit weatherへ転換するにはspatial representativenessのprospective linkageが必要である。

今回、そのlinkageを値を見た後に追加することを避けたため、station valueをselected planning unitのformal weatherとして昇格させなかった。

## 12.6 direct observation

protocol-compliantな現地direct observationは実施しなかった。

したがって、current site condition、fuel/biomass condition、infrastructure等について、観察していない値を推測で補わなかった。

---

# 13. 主要結果

## 13.1 formal frame

```text
planning units                    = 6
representative actions            = 5
pilot cases                       = 30
operational domains               = 10
case x domain rows                = 300
required rows                     = 192
not-required rows                 = 108
```

## 13.2 required-input availability

```text
AVAILABLE                         = 0
UNKNOWN                           = 156
UNAVAILABLE                       = 36
```

## 13.3 blocker distribution

```text
BLOCKED_BY_MISSING_INFORMATION     = 114
BLOCKED_BY_VERIFICATION            = 42
BLOCKED_BY_UNAVAILABLE_INFORMATION = 36
```

## 13.4 workflow readiness

```text
usable required inputs             = 0 / 192
blocked required inputs            = 192 / 192
operational-workflow-ready cases   = 0 / 30
```

## 13.5 formal gates

```text
OP1_SCHEMA_COMPLETENESS                 PASS
OP2_PROVENANCE_INTEGRITY               PASS
OP3_TEMPORAL_VALIDITY_INTEGRITY        PASS
OP4_STATE_TRANSITION_INTEGRITY         PASS
OP5_BLOCKER_SEMANTICS_INTEGRITY        PASS
OP6_WORKFLOW_CONNECTION_INTEGRITY      PASS
OP7_HISTORICAL_AND_ECOLOGICAL_FIREWALL PASS
OP8_REPRODUCIBILITY                     PASS
OP9_ADVERSARIAL_CONSISTENCY            PASS
```

25 adversarial casesをすべて評価し、すべてprospectively expected behaviorと整合した。

## 13.6 formal outcome

以上により、formal outcomeは、

```text
WORKFLOW_VALIDATED_WITHIN_PILOT_SCOPE
```

となった。

---

# 14. なぜ「usable 0件」でもworkflow validationはPASSなのか

この点は本研究で最も誤解されやすい。

もしendpointが、

> 30 casesのうち何件が実行可能か

であれば、0 ready casesは明確に異なる意味を持つ。

しかし本研究のendpointは、

> operational factsをprospective evidence ruleで扱い、取得不能・未検証・時間的に不十分な情報を、誤ってsafe / feasible / approvedへ変換せず、blockerとしてworkflowへ接続できるか

である。

したがって、情報不足の多い環境では、正しいworkflowは多数の`UNKNOWN`や`UNAVAILABLE`を返す可能性がある。

実際、今回の結果は、

```text
UNKNOWN / UNAVAILABLEを埋めなかった
-> required inputsはすべてblockされた
-> 0 casesがreadyになった
-> それでもschema/provenance/time/state/blocker/firewall/reproducibilityはすべてPASSした
```

というものである。

これは「管理可能性が高い」というpositive resultではなく、**不十分な情報を不十分なまま安全に表現できた**というworkflow validation resultである。

---

# 15. 本研究が支持すること

本研究は、凍結済みpilot scopeの範囲で、次を支持する。

1. local/current operational factsを、ecological evidenceとは別の情報層として扱える。
2. source identity、authority、geographic scope、temporal validityをformal workflowへ組み込める。
3. planning unitが行政界を跨ぐ場合、minority jurisdictionを消さずに保持できる。
4. `UNKNOWN`、`UNAVAILABLE`、`NOT_VERIFIED`、`VALIDITY_UNKNOWN`を補完せず保持できる。
5. required inputが利用不能であれば、explicit blockerへ決定論的に変換できる。
6. blockerをcandidate actionのrequired-input structureへ接続できる。
7. unfavorable / missing resultでもhistorical/ecological boundaryを維持できる。
8. 同じfrozen inputからformal materializationを再現できる。
9. 誤推論を狙った25 adversarial casesに対してguardを維持できる。

---

# 16. 本研究が支持しないこと

本研究は次を支持しない。

```text
どのcandidate actionが実行可能か
どのcandidate actionが安全か
どのcandidate actionが許可されているか
どのcandidate actionを選ぶべきか
candidate actionのranking / priority
management recommendation
planning-unit ecological response
management effectiveness
biodiversity benefit
ecological desirability
optimizer coefficient
human usability
practitioner acceptance
public-document compatibilityのhuman benefitへの転換
```

特に、

```text
operational feasibility
!= ecological desirability

current safety / permission
!= management-response evidence

local/current facts
!= ecological response evidence
```

である。

---

# 17. predecessor studiesとの関係

本研究はpredecessorを救済・変更する研究ではない。

formal closure時点でも次はそのままである。

```text
NAP-001 public-only Stage C  COMPLETE / FROZEN
NAP-002 Study 1             COMPLETE / FROZEN
NAP-002 Study 2A            COMPLETE / FROZEN / INDETERMINATE
NAP-002 Study 2B            DEFERRED / NOT STARTED
G4 Response-Transfer v1     COMPLETE / FROZEN / INDETERMINATE
human validation            NOT PERFORMED
```

G4については、

```text
NO_INDEPENDENT_VALIDATION_EVIDENCE
!= TRANSFER_NOT_SUPPORTED
```

を維持する。

Operational Pilotがworkflow validationにPASSしたことは、G4 transferabilityを支持したことにも、否定したことにもならない。

historical:

```text
transfer_to_zone_contribution = false
```

も変更していない。

---

# 18. 実務的含意

## 18.1 「情報が足りない」を正式な出力にできる

実務支援systemでは、「値がない」ことをerrorとして捨てると、人間が空欄を都合よく解釈する余地が生まれる。

本研究では、missing / unavailable / not verifiedを明示的なworkflow stateとblockerにした。

たとえば、

```text
必要なpermission recordが見つからない
```

場合、出力は「許可なし」でも「許可済み」でもなく、

```text
UNKNOWN
BLOCKED_BY_MISSING_INFORMATION
```

となる。

## 18.2 operational readinessを急いで作らない

今回0/30 casesがreadyだったことは、workflowの失敗ではなく、public-sourceだけではcase-specific local operational knowledgeが不足することを示すdescriptive observationである。

実際のplanningへ進むには、今後、適法にアクセス可能なlocal management record、land/manager identity、actual access route、operation-specific safety plan、permit、protocolled field observation等が必要になり得る。

ただし、それらを追加する場合は本study v1を後から書き換えず、新しいextension / studyとして扱う必要がある。

---

# 19. 研究上の限界

## 19.1 public-source ceiling

今回のformal acquisitionはofficial public source routeを中心とした。local manager record、private operational plan、restricted authorized record等を使用していない。

したがって、「必要情報が存在しない」と結論したのではなく、**今回のfrozen acquisition frameではformal usable stateへ到達しなかった**と解釈すべきである。

## 19.2 direct observation未実施

current site、fuel/biomass、infrastructure等についてfield observationを実施していない。

このため`UNAVAILABLE`が多く発生した。

## 19.3 human validation未実施

実在するpractitionerやadministrative decision-makerが、このworkflowを理解しやすいか、実務で使えるか、benefitがあるかは検証していない。

```text
workflow validation
!= human usability validation
```

である。

## 19.4 temporal snapshot

formal decision timeは2026-08-15 11:13:35 JSTである。operational factsは時間依存であり、別日の結果は異なり得る。

本studyのformal endpointを別日のcurrent informationでretroactively更新してはならない。

## 19.5 pilot frameの限定

対象は6 planning units × 5 representative candidate actions = 30 casesである。

他のplanning units、他地域、他のmanagement systemへ一般化するには別途validationが必要である。

## 19.6 source-route completeness

source routeはprospectively固定したが、現実世界の全authority / manager / private operational recordを網羅したことを意味しない。

## 19.7 ecological validationではない

本studyのpositive formal outcomeはecological endpointを持たない。

よって、management response、species response、habitat outcome、landscape outcome等を導くことはできない。

---

# 20. 第三者資料・データの取扱い

本研究では、第三者のadministrative/geospatial dataとofficial web materialをsourceとして参照した。

raw historical planning-unit GeoJSONとMLIT archiveはhash identityを記録したが、外部公開report本文には第三者dataのfull raw contentsを再配布しない。

formal source auditでは、必要なsource identity、URI、scope、formal-use ceilingを記録し、公式pageの全文複製は行わない。

個人情報、human participant data、private pasture-manager recordは含まれていない。

---

# 21. implementation incident

最初のformal CI attempt `31858950145`では、Python runner内にJSON literal `false`を用いた実装ミスがあり、formal materialization completion前に停止した。またmaterialization pipelineに`pipefail`がなく、例外のfail-fast性が不足していた。

このrunではformal scientific outcomeを作成していない。

修正は、

```text
false -> False
provenance validatorのfield参照を既存凍結keyへ整合
materialization pipelineへpipefail追加
```

に限定した。

case frame、source route、freshness rule、T0、state vocabulary、gate、outcome rule、interpretation boundaryは変更していない。

修正後のformal run `31858992089`で全gateを評価した。

---

# 22. 再現性・検証

successful formal validation:

```text
GitHub Actions run ID = 31858992089
job ID = 94948791967
conclusion = success
artifact ID = 9239914641
artifact ZIP SHA-256 = d42993da91032d9c320f738192c4e751d5fd605ca9042060c1726c53e3387c25
```

formal runnerは独立2回実行され、OP8でbyte-identical outputを確認した。

主要output identity:

```text
operational_input_snapshot.csv
SHA-256 = d490ae1d2625729ef7602d91aba8b4054140aa302ebf9fb917081a47bffb7338
bytes = 110,672

operational_blocker_matrix.csv
SHA-256 = fc3a445f415437121c983878a89f176418540bb434775e0a1d3dbfa93f83577f
bytes = 4,531

operational_formal_summary.json
SHA-256 = 9129ffc1134eb4d9c44aab40b330665d53c6c0d163dc599560ab42beb75d531f
bytes = 901

operational_formal_manifest.json
SHA-256 = 925957ceeabb481ecc3dc5d236a67d42a35377d238a81b1cbb650f400f7ca73a
bytes = 591

operational_pilot_formal_validation.json
SHA-256 = 10d2557e0d653b5a84fc2f9efdbe426d53398917e4f2a67cbe5740f9644d56b7
bytes = 1,276
```

---

# 23. 再現性識別子

```text
Study ID:
NAP-OP-2026-08-15-v1

Formal run ID:
NAP-OP-FORMAL-2026-08-15-v1

Scientific-state snapshot:
f1a1f63ed76e960da1355def19bd823027d29aa6

Base main snapshot:
d3c930293de6fd0126b58233d706075f371c9726

T0:
2026-08-15T11:13:35+09:00

Historical M1 geometry:
46ef4dd475ec7645d78130722e8ddfcae9f51c6244ce95dc22ea9f7dcaa6609d

MLIT administrative archive:
e38cdc6102af404f05c2bac5f5219858c3c1d401fb9ff0acf410edd70ae88bd0

Formal cases:
30

Required operational inputs:
192

Usable required inputs:
0

Operational-workflow-ready cases:
0

Adversarial cases:
25

Formal outcome:
WORKFLOW_VALIDATED_WITHIN_PILOT_SCOPE
```

---

# 24. Repository内の主要資料

## Protocol / prospective design

```text
doc/operational-pilot/OPERATIONAL_PILOT_PROSPECTIVE_PROTOCOL.md
analysis/operational-pilot/operational_pilot_design_registry.json
analysis/operational-pilot/operational_input_taxonomy.json
analysis/operational-pilot/operational_state_vocabulary.json
analysis/operational-pilot/operational_source_provenance_rules.json
analysis/operational-pilot/operational_temporal_validity_rules.json
analysis/operational-pilot/operational_adversarial_expected_cases.csv
```

## Pilot frame / spatial linkage

```text
analysis/operational-pilot/operational_pilot_case_registry.csv
analysis/operational-pilot/operational_pilot_case_geometry_manifest.json
analysis/operational-pilot/operational_pilot_case_geometry_rehydration_verification.json
analysis/operational-pilot/operational_case_jurisdiction_overlay.json
```

## Source / time freeze

```text
analysis/operational-pilot/operational_source_acquisition_plan.csv
analysis/operational-pilot/operational_case_source_freshness_registry.json
analysis/operational-pilot/operational_t0_registry.json
analysis/operational-pilot/operational_preexecution_incidental_exposure_audit.json
```

## Formal acquisition / execution

```text
analysis/operational-pilot/operational_formal_source_acquisition_audit.json
analysis/operational-pilot/run_operational_pilot_formal.py
analysis/operational-pilot/validate_operational_pilot_formal.py
analysis/operational-pilot/operational_formal_summary.json
analysis/operational-pilot/operational_formal_manifest.json
analysis/operational-pilot/operational_pilot_formal_validation.json
analysis/operational-pilot/operational_pilot_formal_result.json
```

## Checkpoints

```text
doc/checkpoints/2026-08-15-operational-pilot-prospective-design-freeze.md
doc/checkpoints/2026-08-15-operational-pilot-geometry-rehydration-closure.md
doc/checkpoints/2026-08-15-operational-pilot-jurisdiction-overlay-freeze.md
doc/checkpoints/2026-08-15-operational-pilot-formal-closure.md
```

---

# 25. 後続研究との境界

本study v1はformal endpointへ到達したため、追加のlocal record、direct observation、human participant research、ecological validationを後から追加してこの結果を上書きしない。

将来の候補は、独立したnew study / extensionとして、たとえば次を扱い得る。

```text
restricted-but-authorized local operational recordsを含むvalidation
protocolled field observationを含むoperational validation
actual pasture-manager / land-authority linkage study
human practitioner usability study
response-transfer validationの新しいindependent context study
```

ただし、いずれも本研究のformal outcomeをretroactively rewriteしない。

NAP-002 Study 2Bは本研究とは独立に、

```text
DEFERRED / NOT STARTED
```

のままである。

---

# 26. 結論

本研究の問いは、local/current operational factsを、provenance、authority、geographic scope、temporal validity、不確実性を保持したまま、Natural Area Planningのdecision-support workflowへ安全に接続できるか、というものであった。

prospectiveに固定した30 casesをformal executionした結果、192 required operational inputsのうちusableだったものは0件であり、30 casesすべてがoperational workflow上blockされた。

しかし、workflowはこの不足を「問題なし」に変換しなかった。156の`UNKNOWN`、36の`UNAVAILABLE`を保持し、114 missing-information blockers、42 verification blockers、36 unavailable-information blockersへ変換した。25 adversarial casesを含むOP1–OP9はすべてPASSし、独立materializationも再現した。

したがって、研究質問へのformal answerは、

```text
WORKFLOW_VALIDATED_WITHIN_PILOT_SCOPE
```

である。

これは、「実行可能な管理行為が見つかった」という意味ではない。

より正確には、

> **case-specific local/current operational evidenceが不足していても、その不足を安全・許可・実行可能・生態学的に好ましいという意味へ変換せず、明示的なblockerとして再現可能にdecision-support workflowへ接続できた。**

という結果である。

Natural Area Planningにおいて、この区別は重要である。planning systemは、答えを必ず出すことよりも、**答えを出してよいevidenceがないときに、どこで止まるべきかを正確に示すこと**を必要とする。本研究は、そのoperational layerについてprospective pilot validationを完了した。

---

# 用語

**operational fact**  
現地で候補行為を検討・実施する際のlocal/currentな実務条件。生態学的response evidenceとは別物。

**T0**  
formal pilot全体で共通に固定したdecision-time reference。本研究では`2026-08-15T11:13:35+09:00`。

**UNKNOWN**  
必要なfactの真偽・状態を決定できない。false、safe、feasible、neutralではない。

**UNAVAILABLE**  
prospective ruleに従った取得・観測を実施できない、またはformal frame内で利用できる情報がない状態。no constraintを意味しない。

**NOT_VERIFIED**  
source authority、scope、identity等をformal ruleで確認できていない状態。

**VALIDITY_UNKNOWN**  
effective/phenomenon timeやexpiryをformalに確定できずCURRENTとして使えない状態。

**blocker**  
required inputがformal workflowで利用できない理由を明示するstate。recommendationやpriority scoreではない。

**WORKFLOW_VALIDATED_WITHIN_PILOT_SCOPE**  
prospectively frozen pilot frameでOP1–OP9を満たし、operational factsのrepresentation / provenance / time / blocker / firewall / reproducibilityがvalidationされた状態。候補行為のoperational feasibilityやecological desirabilityを意味しない。

---

# Selected methodological references / prior art

- World Wide Web Consortium (W3C). *PROV-O: The PROV Ontology*. W3C Recommendation.
- Open Geospatial Consortium (OGC). *OGC Abstract Specification Topic 20: Observations, Measurements and Samples*.
- ISO. *ISO 19157-1:2023 Geographic information — Data quality — Part 1: General requirements*.
- Runge, M.C. & Bean, E. (2020). *Decision Analysis for Natural Resource Management*. U.S. Geological Survey.
- Williams, B.K., Szaro, R.C., & Shapiro, C.D. (2009). *Adaptive Management: The U.S. Department of the Interior Technical Guide*. U.S. Department of the Interior.
- 国土交通省. 国土数値情報「行政区域」2026年版・熊本県.
- 阿蘇市、産山村、熊本県、環境省、気象庁の各official operational / administrative information routes. 個別source identityとformal-use ceilingはrepository内formal source auditを参照。

---

# 引用時の推奨表記

```text
Natural Area Planning / NAP-OP-2026-08-15-v1 (2026).
Public Research Report: local/current operational factsは意思決定支援workflowへ安全に接続できるか
― 30 cases・192 required inputsをprospective ruleで処理し、利用可能情報が0件でも未知・未検証を正しくblockできることを検証した ―.
Version 1.0, 2026-08-15.
Formal outcome: WORKFLOW_VALIDATED_WITHIN_PILOT_SCOPE.
Study snapshot: nkkmd/natural-area-planning @ f1a1f63ed76e960da1355def19bd823027d29aa6.
```