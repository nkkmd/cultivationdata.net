# Natural Area Planning / NAP-001 — Public Research Report

**公開情報だけで阿蘇半自然草原の管理計画はどこまで構築できるか**  
**― 最適化の前に、証拠が許す境界を明示する ―**

- レポート版: **v1.0**
- 作成日: **2026-08-12 JST**
- 研究プロジェクト: **Natural Area Planning / NAP-001**
- 研究状態: **Stage C public-only study complete**
- 研究正本スナップショット: `nkkmd/natural-area-planning` / commit `a6e6a806bfdec127e573df4b9af59b6f5df626f0`
- 本文の性格: **単体公開用研究レポート**

> この文書は、元のGitHubリポジトリ、内部の研究ログ、checkpoint、CSV、コードを参照しなくても、研究の背景、問い、方法、主要結果、限界、再現性、今後の研究課題を理解できるように構成している。GitHub外へこのファイル単体をコピーして公開しても、研究レポートとして成立することを意図している。

---

## 要旨

半自然草原の保全では、土地を単に「保護する／利用する」に二分するだけでは不十分な場合がある。野焼き、採草、放牧などの反復的な人為管理によって成立・維持されてきた草原では、管理の停止そのものが植生遷移や木本化を引き起こし得る一方、管理の種類、強度、時期によって植物、希少種、昆虫、植生構造などの応答は異なる。

Natural Area Planning / NAP-001 は、熊本県阿蘇地域の半自然草原を対象として、**誰でも追跡・再取得できる公開情報だけを用いた場合に、management-specific systematic conservation planning（管理方法を明示した体系的保全計画）をどこまで科学的に構築できるか**を検討した研究である。

研究では、公開行政資料、査読論文、公開GIS、公開植生データ、Landsat・Sentinel-2等の公開衛星データ、公開統計・政策情報を用い、193のplanning unitsを固定した。管理を単なる「農業／保全」ではなく、野焼き、放牧強度、採草・刈取り、管理停止・再開等を区別したoperational management regimesとして表現し、保全対象も草原性専門植物、群集組成・遷移、希少種、開放草原構造、草原依存性動物等に分離した。さらに13の管理regimeと11のfeatureからなるregime–feature evidence architectureを構築し、既存のmulti-zone conservation planning、特にMarxan with Zones型の構造へ対応付けた。

その結果、**計画問題の構造表現自体は成立した**。公開植生データからplanning-unit単位の植生組成base stateおよびopen/semi-natural grasslandのpresence/areaを構築でき、source-localな管理応答も複数確認できた。しかし、公開証拠だけでは、mandatory featureに対する数値目標、planning-unitごとのtarget-specific amount、source siteからplanning contextへの定量的response transfer、hard feasibility、牧野単位の生産・費用・労働・capacity、hard safeguard thresholdを同時に満たすことができなかった。

最終的なplanning-readinessは、直接利用可能なplanning response（Q3）= 0、local management × context model（Q4）= 0、最高readiness class（G4）= 0、zone-contribution coefficient = 0、formal optimizer = **NOT AUTHORIZED** となった。

本研究の中心的成果は、最適なゾーニング図ではない。**公開証拠を管理計画へ変換できるところと、数値を作ればpseudo-precision（擬似的な精密さ）になるため変換を止めるべきところを、再現可能な形で区別したこと**である。

---

# 1. 研究の背景

## 1.1 生産と自然保全は常に対立するのか

農業や林業などの生産活動は、土地改変や集約化を通じて自然環境に負荷を与えることがある。一方で、世界には、継続的な人間利用そのものが生物多様性や半自然生態系の維持に関わるworking landscapesも存在する。

Ecoagriculture、working lands conservation、landscape approach、High Nature Value farming、multifunctional agriculture、GIAHS、OECM、IUCN Category Vなどは、生産・生活・保全を完全に分離しない土地利用を扱ってきた。したがって本研究は、「生産しながら保全する」という発想自体を新規理論として提案するものではない。

本研究が扱うのは、より具体的な実証・計画上の問題である。

> **どの保全対象に対して、どの管理を、どの強度・時期・条件で行うことが必要・有益・中立・競合的・有害なのか。さらに、その管理を景観内にどのように配置すべきなのか。**

この問いでは、「農業は自然に良い」「管理しない方が自然である」といった一般命題を前提にしない。

## 1.2 Productive ConservationからNatural Area Planningへ

本研究は当初、**Productive Conservation（生産的保全）**という作業概念から出発した。生産活動が収益を生みながら自然環境の維持管理費用の一部を内部化できるのではないか、という問題意識である。

しかし先行研究を検討すると、生産景観・多機能農業・working lands・保全と生産のPareto/frontier分析・systematic conservation planningなどに強いprior artが存在した。そのため、Productive Conservationを新しい保全制度や新理論として押し出すのではなく、研究の焦点を、**管理行為の実証と、その空間計画への接続**へ移した。

ここでいうNatural Area Planningは、新しいoptimizerの名称ではない。既存の体系的保全計画手法を再発明するのではなく、**現実の管理証拠を、どこまで既存planning structureへ科学的に入力できるかを検証するempirical problem-definition / decision-support layer**である。

---

# 2. 研究対象 — 阿蘇半自然草原

熊本県阿蘇地域には、野焼き、放牧、採草などの人間活動と長期的に関係しながら維持されてきた広大な半自然草原が存在する。環境省等の公開情報でも、阿蘇草原は自然・文化・生産の複合的価値を持つ景観として位置づけられている。

阿蘇を研究対象とする理由は、管理をゼロにした状態を単純な「自然状態」と見なせないことにある。火入れ停止後の植生変化を扱ったYamamoto et al. (2002)など、阿蘇の公開研究は管理停止に伴う草原構造・植生組成・木本化の変化を示している。一方、放牧や刈取りについても、強度や時期によって保全対象の応答が異なることが報告されている。

そのため、阿蘇の管理問題は次のように表現する必要がある。

```text
保全対象
  ×
管理の種類
  ×
管理強度
  ×
管理時期
  ×
背景管理
  ×
立地・生態学的context
```

「管理あり／なし」だけでは、この構造を表現できない。

---

# 3. 研究目的と中心的研究質問

NAP-001 Stage Cの中心的研究質問は次のとおりである。

> **阿蘇半自然草原について、誰でも独立に入手・追跡できる公開証拠だけを用いて、管理方法を明示したsystematic conservation planningをどこまで構築できるか。そして、どのinterfaceから先は、定量的配分に必要な精度を公開証拠が正当化できなくなるのか。**

ここで「成功」を最適化結果が出ることとは定義しなかった。

本研究では、

```text
solverが数値を必要とする
!=
その数値について科学的証拠が存在する
```

という区別を研究設計上の原則とした。

---

# 4. public-only研究設計

## 4.1 使用した証拠

現在のNAP-001 Stage Cで使用した経験的証拠は、公開かつ再取得可能なものに限定した。

主な証拠群は以下である。

- 公開行政資料・政策資料
- 公開された査読論文・研究成果
- 環境省等の公開GIS
- 公開植生図・植生属性
- Landsat
- Sentinel-2
- 公開農業統計・人口統計・労働関連統計
- 公開された支援・管理体制情報

## 4.2 使用しなかった証拠

以下はcurrent NAP-001 Stage Cの経験的証拠に含まれない。

- 申請制の2011・2016・2021年阿蘇草原維持再生基礎調査原データ
- 許可制の牧野カルテ等
- 非公開・限定公開の希少種詳細位置
- 個別生産者の非公開経営データ
- private correspondence
- 新規field survey
- 新規prospective monitoring

将来のrestricted/local-data研究を想定したprotocolや申請準備文書は作成したが、**申請は一度も提出していない**。restricted/local empirical dataは、受領、アクセス、閲覧、変換、解析、利用のいずれも行っていない。

このため、Stage Cはpublic-only studyとして完結している。

---

# 5. 方法

## 5.1 二層構造: P-001 と NAP-001

研究は二つの層から構成した。

```text
P-001  Evidence layer
       公開情報から阿蘇の管理・生態・空間・経済contextを整理する

NAP-001 Planning layer
        P-001の証拠をsystematic conservation planningへ接続する
```

P-001は「公開証拠から何が言えるか」を扱い、NAP-001は「その証拠をplanning inputへどこまで変換できるか」を扱う。

## 5.2 P-001の6モジュール

P-001は以下の6モジュールで構成した。

| モジュール | 内容 | planning上の役割 |
|---|---|---|
| P1 | 保全上の重要性 | study-system justification |
| P2 | 管理 × 保全対象の証拠 | management-response evidence |
| P3 | 公開GISによる空間状態 | planning-unit / spatial context |
| P4 | 公開衛星による時間的context | state / temporal uncertainty |
| P5 | 生産・労働・管理継続性 | burden / continuity context |
| P6 | leakage | displacement scenario |

これらを一つの「持続可能性スコア」に集約しなかった。

## 5.3 planning units

公開GISから、planning対象となる空間単位を構築し、**193 planning units**を固定した。

planning-unit identityは研究正本でhash固定されている。

```text
planning units = 193
SHA-256 = 46ef4dd475ec7645d78130722e8ddfcae9f51c6244ce95dc22ea9f7dcaa6609d
```

P3で得られる土地利用、地被、野焼き状態、地形等はcontext/stateとして扱い、それだけから生物多様性や管理効果を推定しない。

## 5.4 保全feature

「生物多様性」を一つの総合値にせず、少なくとも次の保全次元を分離した。

```text
T1  草原性専門植物群集
T2  群集組成・植生遷移
T3  希少種・保全優先種
T4  植生構造・開放草原状態
T5  草原依存性動物
T6  一般的な種数・多様性指標（補助）
```

この分離により、例えば総種数の増加が、希少種の減少や草原構造の喪失を自動的に相殺することを防いだ。

## 5.5 management regimes

planning variableは、土地を抽象的な「保全」「農業」に分けるのではなく、**operational management regime**をplanning zoneとして割り当てる形で定義した。

概念的には、

```text
x_i,k = planning unit i を management regime k に割り当てるか
```

である。

候補regimeには、例えば次が含まれる。

- burn-only
- burning + low grazing
- burning + customary/moderate grazing
- burning + high grazing
- burning + mowing/hay
- mixed management
- restoration / restart
- active conservation-only management
- management cessation / succession trajectory

現在のregime-feature evidence architectureは**13 management regimes × 11 conservation features**で構成される。

## 5.6 management-response evidenceのreadiness

source studyで観測された差を、そのままplanning coefficientへ変換しないため、response evidenceを以下のように分類した。

```text
Q0  usable evidenceなし
Q1  qualitative directionのみ
Q2  source-relativeな定量差
Q3  planningで直接利用できるresponse
Q4  local management × context response model
```

さらに、planning-readinessを次の4 interfaceに分けた。

```text
Q-A  target semantics
Q-B  planning-unit base amount / occurrence
Q-C  source-to-planning transfer domain
Q-D  constraint / safeguard semantics
```

重要な判定原則は以下である。

```text
unknown != zero
public no-record != ecological absence
Q2 source contrast != Q3 planning coefficient
source-local response != planning-unit response
semantic crosswalk != response transfer
mapped vegetation state != management effect
spectral trajectory != biodiversity outcome
optimizer input need != evidence of numeric precision
```

## 5.7 公開植生base state

公開植生データを193 planning unitsへ対応させ、二つの主要base-state artifactを構築した。

```text
planning_unit_vegetation_composition
rows = 1426
SHA-256 = ac01c133cf8d366dc02d0da2b8e1ff1334e5b997f31b74553cd60f0afc4b5413

planning_unit_open_grassland_state
rows = 193
SHA-256 = c6aee6d304e223189289e8b2f8bcbf5f42042b037304741365091eea77a4a93c
```

前者はT2に対するplanning-unit vegetation-composition vector、後者はT4に対するopen/semi-natural grassland presence/areaを与える。

ただし、公開植生源は時間的にheterogeneousであり、同一日に行われた2024年field censusではない。また、植生図のclassは管理応答やtarget attainmentそのものではない。

## 5.8 remote sensing

P4ではLandsatをprimary、Sentinel-2をrecent-period secondary validationとして使用し、NDVI、NDMI、NBRをcontext/state指標として扱った。

これらを直接のbiodiversity indicatorやmanagement coefficientとは解釈しなかった。

## 5.9 planning softwareへの構造対応

management-specific planning structureを、既存のsystematic conservation planning、とくにMarxan with Zones型のmulti-zone structureへ対応付けた。

structural pilotでは、少なくとも以下の表現が可能であることを確認した。

```text
planning units
management zones / regimes
conservation features
spatial relationships / adjacency
resource burdens
scenario families
```

このstructural pilotはPASSした。

したがって、後述するformal optimization未実施の理由は、softwareが問題を表現できないからではない。

---

# 6. 主要結果

## 6.1 管理効果を単一の「良い／悪い」にできなかった

公開証拠から最も一貫して得られた知見は、管理効果がconditionalであることだった。

概念的には、

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

と表現する必要がある。

Yamamoto et al. (2002)の火入れ停止・植生変化、Murata et al. (2008)の放牧強度とオオルリシジミ生息環境、Murata & Matsuura (2011)の放牧強度と蝶類群集、農研機構の刈取り時期に関する公開研究成果などは、管理がtarget-specificであり、単純な「強いほど良い」「使わないほど良い」という普遍順位を支持しない。

これは、management mosaicを将来検討する理由にはなるが、「mosaicが必ず最適」という結論ではない。

## 6.2 193 planning unitsの空間architectureは構築できた

public GISだけでも、193 planning unitsの固定、空間context、adjacency、公開植生base stateを構築できた。

したがって、公開データだけでも「どこをplanning unitとして扱うか」「現在どのようなmapped vegetation stateがあるか」というplanningの空間骨格は相当程度まで構築できる。

## 6.3 remote sensingは有用だが、uncertaintyも示した

2017–2025年について、景観全体の方向は以下のように保持された。

| 指標 | Landsat | Sentinel-2 |
|---|---|---|
| NDVI | negative | positive |
| NDMI | negative | positive |
| NBR | negative | positive |

三指標すべてでrecent directionがdiscordantだった。

本研究では、

- 二つのsensorを平均して一つのtrendにしない
- 望ましい物語を与えるsensorだけを選ばない
- sensor差をbiodiversity差と読み替えない

という判断を採用した。

P4の最終用途は**context_state_only**である。

## 6.4 T2 — vegetation composition / succession

T2では、公開植生データによってplanning-unit単位のmapped vegetation-composition vectorを構築できた。

最終判定は次のとおりである。

```text
Q-A = TARGET_ORDINAL_STATE_CANDIDATE
Q-B = BASE_AMOUNT_DIRECT_CANDIDATE
Q-C = TRANSFER_DOMAIN_NARROW_SOURCE_ONLY
Q-D = CONSTRAINT_SIGNAL_ONLY
workflow class = G3
```

重要なのは、mapped compositionが**現在のmapped state**であって、時間的なsuccession rateや特定管理に対するresponseではないことである。

## 6.5 T4 — open-grassland structure

T4では、公開mappingからopen/semi-natural grasslandのpresence/areaをplanning-unit単位で表現できた。

最終判定は次のとおりである。

```text
Q-A = TARGET_ORDINAL_STATE_CANDIDATE
Q-B = BASE_AMOUNT_PRESENCE_ONLY_CANDIDATE
Q-C = TRANSFER_DOMAIN_NARROW_SOURCE_ONLY
Q-D = CONSTRAINT_SIGNAL_ONLY
workflow class = G3
```

しかし、mapped open-grassland areaは、植生高、被度、stem density、litter、woody encroachment等の直接的structural metricではない。

## 6.6 希少種・草原依存性動物

公開研究には、特定希少植物やオオルリシジミ等についてmanagement-specificなsource evidenceが存在する。しかし、planning-unitごとの現在量・occurrenceと、source siteから193 planning unitsへのtransferable response functionを同時に確立できなかった。

特に希少種について、公開情報で記録が見つからないことを「そのplanning unitには存在しない」というゼロに変換していない。

## 6.7 workflow-readiness

public auditで評価した主要featureのreadinessは以下となった。

```text
G4 = 0
G3 = 2
G2 = 0
G1 = 4
G0 = 2
```

ここでG0–G4は**研究workflow / transformation readiness**であり、生物多様性の良し悪しを示すscoreではない。

最高のG4へ到達したfeatureはゼロだった。

---

# 7. formal optimizationを実行しなかった理由

## 7.1 最終hard readiness

Stage C終了時のhard readinessは次のとおりである。

```text
static_allocation_eligible_regimes   = 0
coefficient_ready_regimes            = 0
target_attainment_eligible_features  = 0
numeric_target_ready_features        = 0
constraint_ready_features            = 0
Q3 planning-response features        = 0
Q4 local response models             = 0
zone contribution data rows          = 0
formal_optimizer_authorized          = false
```

すべてのformal conservation target valueは未設定のままであり、regime–feature relationshipはformal zone contributionへ移行していない。

## 7.2 software failureではない

structural software pilotはPASSしている。

つまり、

```text
optimizerを実行できない
```

のではなく、

```text
optimizerへ科学的に正当化された数値を渡せない
```

のである。

この違いは重要である。

## 7.3 なぜ仮の係数を置かなかったのか

source studyで、management Aの方がBより良い結果だったとしても、それだけから、

```text
A = 1.0
B = 0.5
```

のようなzone contributionを作ることはできない。

そのような正規化は、

- source site固有の差
- background management
- targetの違い
- measurement unitの違い
- planning-unit context
- transfer uncertainty

を隠す。

solverは入力された数値から必ず何らかの解を返し得る。しかし、**解が計算できることは、入力値が科学的に妥当であることを意味しない。**

NAP-001はこの段階で停止することを正式な研究結果として採用した。

---

# 8. 本研究が支持すること

public-only Stage Cは、少なくとも以下を支持する。

1. 阿蘇半自然草原をmanagement-specific planning problemとして定義できる。
2. planning unitsを公開GISから再現可能に構築できる。
3. managementをoperational regimeとして分離できる。
4. 保全対象を非代替的なfeatureとして分離できる。
5. source-relativeなmanagement × target evidenceを体系化できる。
6. 公開植生情報からT2/T4のbase-state候補を構築できる。
7. remote sensingをcontext / uncertainty layerとして利用できる。
8. management-specific structureを既存multi-zone planning softwareへ構造的に対応付けられる。
9. どのplanning inputが不足しているかをinterface別に特定できる。
10. unknownをzeroへ変換せずにplanning-readinessを閉じられる。

これらはformal optimizerがなくてもdecision supportとして利用可能である。

---

# 9. 本研究が支持しないこと

本研究は以下を主張しない。

- 「農業は一般に生物多様性に良い」
- 「管理しない土地は一般に悪い」
- 「放牧は強いほど良い」
- 「customary grazingがすべての保全対象で最良」
- 「総種数が高ければ保全状態も必ず良い」
- 「衛星指標のtrendがbiodiversity trendを直接表す」
- 「公開記録がない希少種は不在である」
- 「mapped vegetation classがmanagement effectを表す」
- 「source-local responseを193 planning unitsへ直接移植できる」
- 「multi-regime mosaicがuniform managementより必ず優れる」
- 「193 planning unitsの最適管理配置を本研究が提示した」

また、本研究は新しいspatial optimizerを発明したというnovelty claimも行わない。

---

# 10. public-only integrity statement

current NAP-001 Stage Cについて、以下を明示する。

```text
Nos.013–015 request submitted               = NO
restricted/local files received            = NO
restricted/local files opened              = NO
restricted/local values inspected          = NO
restricted/local values analyzed           = NO
pasture-card request submitted              = NO
restricted rare-species request submitted  = NO
confidential producer-data request          = NO
```

restricted/local data routeは将来研究として検討できるが、current Stage Cを成立させるために必要なものではない。

したがって、

```text
Stage C = current NAP-001 / COMPLETE public-only study
Stage A = future / deferred / optional restricted/local-data successor
Stage B = future / deferred / optional field/monitoring successor
```

という研究境界を採用する。

---

# 11. 再現性とprovenance

## 11.1 frozen identities

本研究の主要な再現性識別子は以下である。

### planning units

```text
count = 193
SHA-256 = 46ef4dd475ec7645d78130722e8ddfcae9f51c6244ce95dc22ea9f7dcaa6609d
```

### vegetation composition

```text
rows = 1426
SHA-256 = ac01c133cf8d366dc02d0da2b8e1ff1334e5b997f31b74553cd60f0afc4b5413
```

### open-grassland state

```text
rows = 193
SHA-256 = c6aee6d304e223189289e8b2f8bcbf5f42042b037304741365091eea77a4a93c
```

### public report provenance

```text
repository = nkkmd/natural-area-planning
study snapshot commit = a6e6a806bfdec127e573df4b9af59b6f5df626f0
study date = 2026-08-12 JST
```

## 11.2 再現性の考え方

再現性は、「同じoptimizer resultが出ること」だけを意味しない。本研究では、次のdecision chainが追跡可能であることを重視した。

```text
public source
 -> source provenance
 -> extraction / interpretation boundary
 -> management / feature crosswalk
 -> evidence-readiness classification
 -> target/base/transfer/constraint audit
 -> optimizer authorization decision
```

negative decisionやnon-readinessもprovenanceを持つ研究成果として保存した。

---

# 12. 第三者資料・データの取扱い

このpublic reportは、単体での外部公開を想定し、第三者著作物の再配布を必要最小限に抑えている。

本レポートには、

- 原著論文PDFそのもの
- 原著論文の表の大量な逐語・逐セル転記
- 原著figure画像
- 原著figureからdigitizeした時系列点群の全データ
- restricted/local raw data
- 個人・生産者識別情報
- sensitive rare-species coordinates

を収録していない。

本文では、研究上必要なsource-level conclusionを要約し、主要論文には著者・年・DOIを付している。

研究内部では、再現性・監査のため、source-level extractionやdigitizationの詳細記録を保持している場合があるが、それらはこの単体公開レポートの配布物には含めない。

この区別により、**研究結果の公開**と**第三者原資料・詳細抽出物の再配布**を分離する。

---

# 13. 限界

## 13.1 public-only ceilingは「世界にデータが存在しない」という意味ではない

本研究が示したのは、定義されたpublic corpusのevidence ceilingである。申請制・許可制・現地保有・非公開データに、より強い管理履歴や生物情報が存在する可能性は否定しない。

## 13.2 vegetation base stateの時間的一貫性

公開植生データはtemporally heterogeneousであり、同一時点のfield censusとして扱えない。

## 13.3 management responseのtransfer

阿蘇内のsource studyであっても、site history、背景管理、処理定義、measurement supportが異なるため、そのresponseをplanning unitへそのまま転用できない。

## 13.4 economic / labor data

公開情報は地域レベルの管理継続性・支援構造を理解するには有用だが、各牧野のprofit、cost、labor hours、capacityをformal allocationへ使用できる粒度ではない。

## 13.5 remote sensing

Landsat / Sentinel-2のrecent direction discordanceは未解決であり、biodiversity coefficientには変換していない。

## 13.6 optimized mapがない

本研究は最適配置図を出していない。これは未完了の計算ではなく、evidence-readiness ruleに従った研究上の停止判断である。

---

# 14. 今後の研究

Stage Cを変更せず、独立したsuccessor studyとして次を検討できる。

## 14.1 Future Stage A — restricted/local data

候補となる情報は、例えば以下である。

- application/permission-based management history
- planning-unit / pasture-level operational feasibility
- local production / cost / labor / capacity
- lawfulなtarget occurrence data

ただし、restrictedであること自体はhigh qualityやcausal identificationを保証しない。受領後もeligibility auditが必要である。

## 14.2 Future Stage B — prospective field / monitoring

formal quantitative allocationへ進むには、むしろprospective designが重要になる可能性がある。

必要となり得るのは、

- operational management vectorの事前定義
- target / threshold semanticsの事前定義
- target-relevant outcomeの直接測定
- management intensity / timing / backgroundの記録
- planning context modifier
- repeated observation
- causal / counterfactual design
- management burden / labor / economic measurement

である。

このfuture researchは、Stage Cのnegative resultを「救済」するための再解析ではなく、新しい研究である。

---

# 15. 実務的含意

NAP-001の結果は、阿蘇だけに限定されない。

working landscapesでspatial optimizationを行う際、GISやsolverを用意するより前に、少なくとも以下を区別する必要がある。

```text
どこに何があるか                 spatial/base state
何を守りたいか                   target semantics
どの管理が何に作用するか         management response
そのresponseを別地点へ移せるか    transfer domain
その管理を実行できるか            feasibility
誰が費用・労働を負担するか         capacity / burden
守るべきhard conditionは何か       safeguards
```

これらのうち一つが不明だからといって、ゼロや仮の正規化係数を入れると、optimizer outputは精密に見えても、evidence contentを超えてしまう。

したがって、保全計画の品質はsolverの高度さだけでは決まらない。**入力値を「作らない」規律も計画手法の一部である。**

---

# 16. 結論

Natural Area Planning / NAP-001は、公開かつ再現可能な情報だけを用いて、阿蘇半自然草原についてmanagement-specific conservation planningの相当部分を構築できることを示した。

構築できたものは、

- 193 planning units
- operational management-regime architecture
- feature-specific conservation architecture
- 13 × 11 regime–feature evidence architecture
- public spatial/context state
- public vegetation base state
- remote-sensing context / uncertainty
- source-relative management-response evidence
- explicit evidence/readiness classes
- established multi-zone planning softwareへのstructural mapping

である。

一方で、公開証拠だけでは、

- formal numeric target
- planning-unit target-specific zone contribution
- Q3/Q4 management response
- hard feasibility
- pasture-level production/cost/labor/capacity
- hard safeguard threshold

を正当化できなかった。

そのためformal optimizerは実行しなかった。

本研究の最終的な到達点は、次の一文に要約できる。

> **再現可能な保全計画研究は、証拠が止まるところで止まるべきである。solverが係数を要求することは、その係数の存在を証明しない。**

この停止点を明示することは「分析できなかった」という失敗ではない。どの情報がdecision supportに使え、どの情報から先がpseudo-precisionになるかを区別し、次に必要な観測・データ・研究設計を特定すること自体が、management-dependent working landscapeにおける重要な研究成果である。

---

# 用語

**planning unit**  
空間計画で割当・評価の単位となる土地の区画。本研究では193 unitsを固定した。

**management regime**  
野焼き、放牧、採草、停止、再開などを、その強度・時期・背景を含めて定義した運用上の管理状態。

**conservation feature**  
計画上、個別に扱う保全対象。種、群集、植生構造など。

**zone contribution**  
あるplanning unitをあるmanagement regimeへ割り当てた場合、そのconservation featureへどれだけ寄与するかを表すplanning input。本研究ではformal numeric valueを作成していない。

**systematic conservation planning**  
明示された保全目標、空間単位、費用・制約等に基づき、保全行動を体系的に空間配置する計画アプローチ。

**pseudo-precision**  
証拠が支える精度以上に細かな数値を与えることで、科学的根拠以上の確実性があるように見せてしまうこと。

**public-only**  
本研究では、申請・特権的アクセスを要さず、第三者が独立に追跡・再取得可能な証拠のみをcurrent empirical studyに採用したことを指す。

---

# 参考文献・主要公開情報源

## 体系的保全計画・working landscapes

1. Watts, M. E., Ball, I. R., Stewart, R. S., et al. (2009). *Marxan with Zones: Software for optimal conservation based land- and sea-use zoning*. Environmental Modelling & Software, 24, 1513–1521. DOI: https://doi.org/10.1016/j.envsoft.2009.06.005
2. Scherr, S. J. & McNeely, J. A. (2008). *Biodiversity conservation and agricultural sustainability: towards a new paradigm of “ecoagriculture” landscapes*. Philosophical Transactions of the Royal Society B. DOI: https://doi.org/10.1098/rstb.2007.2165
3. Kremen, C. & Merenlender, A. M. (2018). *Landscapes that work for biodiversity and people*. Science. DOI: https://doi.org/10.1126/science.aau6020
4. Sayer, J., Sunderland, T., Ghazoul, J., et al. (2013). *Ten principles for a landscape approach to reconciling agriculture, conservation, and other competing land uses*. Proceedings of the National Academy of Sciences. DOI: https://doi.org/10.1073/pnas.1210595110
5. Queiroz, C., Beilin, R., Folke, C. & Lindborg, R. (2014). *Farmland abandonment: threat or opportunity for biodiversity conservation? A global review*. Frontiers in Ecology and the Environment / related review corpus used in the project. DOI: https://doi.org/10.1890/120348

## 阿蘇の管理・生態学的証拠

6. Yamamoto et al. (2002). 「阿蘇地域の半自然草地における火入れ中止にともなう植生の変化」. DOI: https://doi.org/10.14941/grass.48.416
7. Murata et al. (2008). *Effect of grazing intensity on the habitat of Shijimiaeoides divinus asonis (Matsumura) (Lepidoptera, Lycaenidae)*. DOI: https://doi.org/10.18984/lepid.59.3_251
8. Murata & Matsuura (2011). *Effect of grazing intensity on species diversity of butterfly communities in the habitat of Shijimiaeoides divinus asonis (Matsumura)*. DOI: https://doi.org/10.18984/lepid.62.1_41
9. Yasunaka et al. (2015). *Assessing the effect of controlled burning and grazing on vegetation change in the grasslands of Aso region using satellite image analyses*. DOI: https://doi.org/10.14962/jass.31.4_117
10. 農研機構（NARO）2010年度研究成果情報（阿蘇草地における火入れ・刈取り時期と植生・希少植物に関する公開研究成果）: https://www.naro.go.jp/project/results/laboratory/nilgs/2010/nilgs10-28.html

## 主要公開データ・公的情報

11. 環境省 阿蘇くじゅう国立公園・阿蘇草原保全関連情報: https://www.env.go.jp/park/aso/
12. 環境省 阿蘇草原自然再生関連情報: https://www.env.go.jp/nature/saisei/kyougi/aso/
13. 環境省 生物多様性センター 生物多様性情報システム／植生図関連公開情報: https://www.biodic.go.jp/ikimonomap.html
14. 阿蘇草原再生情報プラットフォーム: https://www.asogreenstock.com/sougensaisei/learn/platform/
15. USGS Landsat Missions: https://www.usgs.gov/landsat-missions
16. Copernicus Sentinel-2 Mission: https://sentinels.copernicus.eu/web/sentinel/missions/sentinel-2

---

## 引用時の推奨表記

本レポートを引用する場合は、少なくとも次の情報を含めることを推奨する。

```text
Natural Area Planning / NAP-001 (2026).
Public Research Report: 公開情報だけで阿蘇半自然草原の管理計画はどこまで構築できるか
— 最適化の前に、証拠が許す境界を明示する —.
Version 1.0, 2026-08-12.
Study snapshot: nkkmd/natural-area-planning @ a6e6a806bfdec127e573df4b9af59b6f5df626f0.
```

著者名、所属、DOI、恒久公開URL等が後日正式に付与された場合は、それらを優先して引用情報へ追加する。