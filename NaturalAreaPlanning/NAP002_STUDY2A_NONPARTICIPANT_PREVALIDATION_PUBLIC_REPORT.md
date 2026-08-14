# Natural Area Planning / NAP-002 Study 2A — Public Research Report

**人間参加者を用いずに意思決定支援表現をどこまで事前検証できるか**  
**― traceability・誤推論耐性・情報等価性・公開行政文書との整合性を検証する ―**

- レポート版: **v1.0**
- 作成日: **2026-08-14 JST**
- 研究プロジェクト: **Natural Area Planning / NAP-002 Study 2A**
- 研究状態: **COMPLETE / FROZEN — non-participant decision-support pre-validation**
- 正式結果: **INDETERMINATE（判定不能）**
- 研究状態スナップショット: `nkkmd/natural-area-planning` / commit `db83cae09e9b84a6b9deab46d87af6c36e9cfa49`
- 本文の性格: **単体公開用研究レポート**
- 編集整備: **2026-08-14 — public-report format alignment; scientific findings unchanged**

> この文書は、元のGitHubリポジトリ、内部protocol、checkpoint、CSV、JSON、監査artifactを参照しなくても、NAP-002 Study 2Aの背景、研究質問、設計、事前凍結、正式評価、主要結果、限界、再現性、Study 2Bとの境界、今後の研究課題を理解できるように構成している。GitHub外へこのファイル単体をコピーして公開しても、研究レポートとして成立することを意図している。

---

## 要旨

NAP-002 Study 1は、熊本県阿蘇地域の半自然草原を対象として、公開証拠の限界を維持したまま、管理判断を `WHERE / WHY / WHAT NEXT` へ分解する意思決定支援表現を構築した。Study 1では、193 planning unitsのうち188がmanagement-review scopeとして `PASS_PUBLIC` となった一方、source-localなmanagement responseをplanning-unit responseへ転移するG4、local operational feasibilityを扱うG6、resource/capacityを扱うG7、current operational conditionsを扱うG8の `PASS_PUBLIC` はすべて0であった。したがってStudy 1は、管理行為の推奨や最適配分ではなく、「どこをレビューし、何が判断を止め、次に何を確認すべきか」を示す構造として完了した。

しかし、科学的に保守的な表現を構築できたことと、その表現が人間にとって理解しやすく、誤読されにくく、実務workflowへ適合することは別問題である。実在する行政担当者・草原管理者・意思決定者を対象に検証するには、human-participant studyが必要になる。一方、本研究時点では実在人物への十分なcontact / recruitmentを前提としない研究経路を明確に分離する必要があった。

そこでNAP-002 Study 2を、非参加者型の事前検証である**Study 2A**と、将来の実在人物による検証である**Study 2B**へprospectively分離した。Study 2Aの研究対象は人間ではなく、Study 1で凍結された意思決定支援表現そのものである。

Study 2Aでは、formal scoring前に、16のatomic facts、うち15のcritical facts、12のunsupported-inference boundary families、36のadversarial cases、情報等価baseline、10本のpublic-document search query、analysis/reproducibility schemaをmaterializeして凍結した。正式評価は5つのnon-compensatory gateから構成した。

```text
A1 traceability                    PASS
A2 evidence-boundary cues         PASS
A3 baseline equivalence           PASS
A4 adversarial consistency        PASS
A5 documentary compatibility      INDETERMINATE

OVERALL                            INDETERMINATE
```

A1では15/15のcritical factsがStudy 1の凍結済みstate / rule / required-input definitionへ追跡可能で、invented assumptionは0だった。A2では12/12のcritical boundary familiesについて禁止解釈を明示的に棄却できた。A3ではStudy-1 conditionとconventional baselineが同じ16 atomic factsを保持し、critical omission / addition / state changeはすべて0だった。A4では12 boundary families × 3 surface variants = 36 adversarial casesについて、36/36のcritical expected interpretationsが内部整合し、contradictionは0、invented ecological truthを必要とするcaseは0だった。

A5では、阿蘇・熊本・環境省等の公開行政・運用文書との用語・workflow compatibilityを検証するため、10 query × 各上位10件、最大100 ranked positionsという有限のretrieval frameを事前凍結した。しかしformal executionではQ06とQ07が各9件しか返さず、98/100 positionsにとどまった。取得できた適格文書の範囲では、management/review planning、permission/authorization、safety/current-condition checks、resource/capacity、monitoring/validation、data/source provenanceの各domainが確認され、Study 1の境界を反転するcritical semantic conflictは観察されなかった。それでも、検索挙動を確認した後に不足2順位を別検索で補完すれば、prospectively frozen stopping ruleを変更することになるため補完しなかった。

したがってA5は `INDETERMINATE`、overallも `INDETERMINATE` とした。これはStudy 1の表現が実務と不適合であるという結果ではなく、**事前に凍結したA5 retrieval frameを完全に実行できなかったため、formal compatibility decisionを確定しなかった結果**である。

Study 2Aは、人間参加者を一切使用しておらず、practitioner comprehension、usability、workflow fit、real-world decision quality、ecological effectiveness、G4 response-transfer validity、management recommendation、safety、permission、priority、optimalityを実証しない。実在人物によるhuman validationはStudy 2Bとして `DEFERRED / NOT STARTED` のまま残る。

本研究の中心的成果は、人間validationの代替を作ったことではない。**human validationへ進む前に、representation自体について何を非参加者型に検証でき、どこから先は人間参加者なしには答えられないかをprospectively分離し、その境界をformal auditとして固定したこと**である。

---

# 1. 研究の背景

## 1.1 NAP-001が示したpublic-evidence ceiling

Natural Area Planning / NAP-001は、阿蘇半自然草原について、公開情報だけからmanagement-specific systematic conservation planningをどこまで科学的に構築できるかを検討した。

NAP-001では、193 planning units、13 candidate management regimes、feature-specific conservation architecture、公開GIS・植生base state、source-localなmanagement-response evidence、planning softwareへのstructural mappingまでを構築した。

しかし、formal spatial allocationに必要なplanning-unit response coefficientやvalidated response transferは構築できなかった。

最終的なpublic-only boundaryは次である。

```text
Q3 planning-response features         = 0
Q4 local management × context models = 0
coefficient-ready regimes             = 0
zone contribution rows                = 0
formal optimizer authorized           = false
```

NAP-001の中心的判断は、solverが数値を必要とすることを理由に、証拠が正当化しない係数を作らないことだった。

## 1.2 NAP-002 Study 1が作ったdecision-support representation

NAP-002 Study 1は、このnon-ready resultを救済してplanning coefficientを補う研究ではない。研究質問を一段上流へ移し、証拠限界そのものを意思決定支援へ変換した。

Study 1のformal answerは `YES` であり、最終的な実務表現は次の三部構造となった。

```text
WHERE
  Management Review Scope Map

WHY
  Candidate Action × Decision-Gate Blocker Matrix

WHAT NEXT
  Candidate Action × Required-Input Checklist
```

Study 1は、management-review scope、decision blocker、information needを機械追跡可能に表現できることを示した。一方で、management recommendation、action ranking、action authorization、planning-unit ecological response、G4 validation、optimizationは支持しなかった。

## 1.3 「科学的に正しい表現」と「人間に有用な表現」は同じではない

Study 1の表現が科学的境界を保持していることは、そのままhuman usabilityを意味しない。

たとえば、次の可能性はStudy 1だけでは評価できない。

```text
情報量が多すぎて理解しにくい
G1 PASSをaction approvalと誤読する
G5 NOT_APPLICABLEをsafeと誤読する
required-input数をpriorityと誤読する
WHERE / WHY / WHAT NEXTの分離がworkflowに合わない
terminologyが現場用語と一致しない
```

この問題を正式に評価するには、実在するpractitioner / administrative decision-makerを対象としたhuman-participant studyが必要である。

しかし、人間参加者の確保が難しい状況で、それをLLMやsynthetic userで置き換えて「human validation済み」とすることは科学的に不適切である。

そこで、human evidenceを必要としないrepresentation-level pre-validationと、human evidenceを必要とするpractitioner validationを別研究へ分離した。

---

# 2. Study 2A / 2Bの分離

2026-08-14、NAP-002 Study 2は次の2研究へprospectively分離された。

```text
NAP-002 Study 2A
  Non-Participant Decision-Support Pre-Validation
  人間参加者なしでrepresentation-level prerequisiteを評価

NAP-002 Study 2B
  Practitioner / Administrative Decision-Support Validation
  実在人物を対象にhuman comprehension / usability / workflow fitを評価
```

この分離はhuman dataを見た後のresult-driven redesignではない。Study 2Bの人間参加者データは0であり、contact、recruitment、consent、survey、interview、participant task、participant-level analysisは開始していない。

既存のhuman-participant protocol lineageはStudy 2Bとして将来へ保持し、Study 2Aは別のprospective studyとして開始した。

重要な関係は次である。

```text
Study 2A COMPLETE
!=
Study 2B COMPLETE

non-participant evidence
!=
human-participant evidence
```

---

# 3. 研究対象

Study 2Aの主たる研究対象は、阿蘇の土地そのものでも、practitioner集団でもない。

研究対象は、NAP-002 Study 1で凍結された**decision-support representation**である。

対象representationは、少なくとも以下から構成される。

```text
G1 management-review scope
G2 target relevance
G3 management-response evidence
G4 response transferability
G5 ecological safeguard / conflict
G6 local operational feasibility
G7 resource / capacity
G8 current operational conditions

WHERE / WHY / WHAT NEXT presentation
required-input architecture
evidence-boundary interpretation rules
```

阿蘇・熊本の公開行政・運用文書はA5のdocumentary compatibility auditに使用したが、それらの文書に記載された個人を研究参加者として扱っていない。

---

# 4. 研究目的と中心的研究質問

Study 2Aの中心的研究質問は次である。

> **NAP-002 Study 1で凍結された意思決定支援表現は、将来のhuman validationへ進む前に必要となる非参加者型の条件、すなわちtraceability、evidence-boundary integrity、information-equivalence、adversarial logical consistency、public-document terminology/workflow compatibilityを満たすか。**

ここで重要なのは、これらの条件がhuman validationの**必要条件になり得るが十分条件ではない**ことである。

Study 2Aがpositiveでも、

```text
practitionerが理解できる
practitionerが使いやすい
workflowに適合する
real-world decision qualityが改善する
```

とは結論しない。

---

# 5. 先行研究との関係とnovelty boundary

## 5.1 decision support evaluation自体は新しくない

自然資源管理・保全では、Structured Decision Making、adaptive management、Value of Information、robust decision making、uncertainty visualization、map-based decision support、participatory spatial decision support等に広い先行研究がある。

Study 2Aは、次の一般概念を新規発明として主張しない。

```text
decision representationを評価すること
uncertainty / evidence boundaryを明示すること
baseline comparisonを設けること
adversarial caseで誤解釈可能性を点検すること
public workflow terminologyとの対応を見ること
human-facing interfaceを別途評価する必要性
```

## 5.2 Study 2Aが扱う固有の問題

Study 2Aの固有性は、一般的なusability methodの発明ではなく、**NAP-002 Study 1の凍結されたboundary-rich representationについて、人間参加者を使わずに検証可能な範囲をprospectively分離し、その結果をhuman validationへ昇格させないformal firewallを実装したこと**にある。

## 5.3 LLMをhuman surrogateにしない

言語モデルは、synthetic interpreterやstress-test instrumentとして探索的に使用する余地を残したが、formal Study-2A decisionには使用しなかった。

```text
LLM behavior
!=
practitioner behavior

AI agreement
!=
human usability
```

この区別をhard boundaryとして保持した。

---

# 6. 研究ガバナンスとprospective firewall

Study 2Aのdesign registryは次の研究identityで凍結した。

```text
studyId = NAP002-STUDY2A-NONPARTICIPANT-PREVALIDATION-2026-08-14-v1
humanParticipantStudy = false
study2BStatus = DEFERRED_NOT_STARTED
llmFormalInferenceAuthorized = false
```

formal outcome vocabularyは次の4つに限定した。

```text
READY_FOR_HUMAN_VALIDATION
NOT_READY_FOR_HUMAN_VALIDATION
MIXED
INDETERMINATE
```

formal evaluationは、test materials、expected keys、public-document search/stopping rule、analysis schema、reproducibility manifestがmaterializeされるまでblockした。

---

# 7. Formal evaluation前のM1–M9 materialization

Study 2Aでは、結果を見ながらtest setを作ることを避けるため、正式評価前にM1–M9をmaterializeして凍結した。

| Material | 内容 |
|---|---|
| M1 | atomic fact registry |
| M2 | Study-1 structured test condition |
| M3 | information-equated conventional baseline |
| M4 | baseline-equivalence audit specification |
| M5 | evidence-boundary cue registry |
| M6 | adversarial case registry |
| M7 | expected interpretation key |
| M8 | public-document search register |
| M9 | analysis schema / reproducibility manifest |

pre-evaluation materialization auditの結果は次である。

```text
facts                              = 16
critical facts                     = 15
Study-1 / baseline fact sets equal = true
boundary families                  = 12
adversarial cases                  = 36
variants per boundary              = 3
public-document queries frozen     = 10
results to screen per query        = 10
maximum discovery frame            = 100
M1 through M9 materialized         = true
pre-evaluation audit               = PASS
```

この時点ではformal gate scoringもpublic-document searchも実行していなかった。

---

# 8. Formal Decision Gates

Study 2Aは5つのnon-compensatory gateで構成した。

| Gate | 評価対象 | hard requirement |
|---|---|---|
| A1 | traceability | critical factの100%がStudy 1へtrace可能 |
| A2 | boundary cues | 12 critical unsupported-inference familiesを明示的に棄却可能 |
| A3 | baseline equivalence | critical omission/addition/state change = 0 |
| A4 | adversarial consistency | 全critical expected interpretationが内部整合し、invented ecological truth不要 |
| A5 | documentary compatibility | prospectively frozen public-document auditを実行し、critical conflictを評価 |

overall decision ruleは次のとおりである。

```text
READY_FOR_HUMAN_VALIDATION
  A1-A4 PASS
  + A5にcritical conflictなし
  + reproducibility/materialization valid

NOT_READY_FOR_HUMAN_VALIDATION
  critical A1-A4 failure
  または A5 CRITICAL_CONFLICT

MIXED
  A1-A4 PASS後のnon-critical mismatch
  critical failureを弱める用途には使わない

INDETERMINATE
  frozen material不足/破損
  irreproducible analysis
  prospectively required documentary auditを実行不能
```

---

# 9. A1 — Structural Traceability

## 9.1 問い

Study 2Aで検証するcritical claimが、Study 1の凍結済みstate、rule、required-input definitionへ明示的に追跡できるか。

## 9.2 failure condition

以下はfailureである。

```text
critical factがStudy 1に存在しない
新しいecological assumptionが必要
legal / operational assumptionを追加する
priority assumptionを追加する
```

## 9.3 結果

```text
critical facts                 = 15
critical facts traceable       = 15
untraceable critical facts     = 0
invented assumptions required  = 0
```

したがって、

```text
A1 = PASS
```

とした。

---

# 10. A2 — Evidence-Boundary Cue Integrity

## 10.1 問い

Study 1が禁止しているunsupported inferenceを、representationから明示的に拒否できるか。

事前に12のcritical boundary familiesを固定した。代表例は次である。

```text
candidate action != recommendation
G1 scope pass != action pass
source-supported != planning-unit supported
unresolved G4 cannot be assumed passed
G5 NOT_APPLICABLE != ecological safety
missing G6/G7 != feasible
missing/current G8 != permitted
missing information != approval
fewer blockers != higher priority
public no-record != ecological absence
unknown != zero / neutral / safe
representation success != ecological validation
```

## 10.2 結果

```text
critical boundary families          = 12
families with derivable rejection   = 12
missing critical boundary cues      = 0
```

したがって、

```text
A2 = PASS
```

とした。

---

# 11. A3 — Information-Equated Baseline

## 11.1 なぜbaselineを情報等価にしたのか

Study 1のstructured representationとconventional baselineを比較する際、structured conditionだけに有利な事実を追加すると、「構造の違い」ではなく「情報量の違い」を比較することになる。

そのため、両条件が保持するdecision-relevant atomic factsを同一に固定した。

許可された差は、organization / presentation structureのみである。

## 11.2 結果

```text
atomic facts                         = 16
Study-1 / baseline fact sets equal   = true
critical fact omissions              = 0
critical fact additions              = 0
critical state changes               = 0
Study-1 ranking signal               = false
baseline ranking signal              = false
Study-1 recommendation signal        = false
baseline recommendation signal       = false
```

したがって、

```text
A3 = PASS
```

とした。

重要なのは、Study 2Aでは人間participantへこの比較条件を提示してperformanceを測定していないことである。A3は**比較条件の情報等価性を検証したgate**であり、structured conditionが人間にとって優れていることを示す結果ではない。

---

# 12. A4 — Adversarial Logical Consistency

## 12.1 設計

A2の12 boundary familiesそれぞれについて、3種類の異なるsurface variantを作成し、合計36 adversarial casesを凍結した。

```text
12 boundary families
×
3 surface variants
=
36 adversarial cases
```

G1–G8 coverageは次である。

```text
G1 = 5 cases
G2 = 5 cases
G3 = 5 cases
G4 = 5 cases
G5 = 4 cases
G6 = 4 cases
G7 = 4 cases
G8 = 4 cases
```

## 12.2 pre-scoring correction

formal scoring前のQAで、初期materializationにはvariant IDが異なるにもかかわらず、一部boundary内でsurface wordingが実質同一という問題が見つかった。

この問題は、

```text
formal scoring前
public-document search前
formal outcome assignment前
```

に修正した。

変更したのはsurface wordingだけであり、fact、expected answer、criticality、gate coverageは変更していない。修正履歴はcheckpointとして保存した。

## 12.3 結果

```text
adversarial cases                          = 36
critical expected interpretations          = 36
consistent critical interpretations        = 36
contradictory critical interpretations     = 0
cases requiring invented ecological truth  = 0
```

したがって、

```text
A4 = PASS
```

とした。

A4はsynthetic caseに対する内部論理の検証であり、人間が実際にどの程度誤読するかというerror rateを測定したものではない。

---

# 13. A5 — Public-Document Terminology / Workflow Compatibility

## 13.1 目的

A5では、Study 1が分離しているconceptやworkflowが、阿蘇・熊本・国の公開行政・運用文書において重大に逆転していないかを検査した。

対象domainは次である。

```text
management / review planning
permission / authorization
safety / current-condition checks
resource / capacity constraints
monitoring / validation
data / source provenance
```

A5は、行政文書がStudy 1を「承認した」かを調べるものではない。

また、

```text
public-document compatibility
!=
practitioner acceptance
```

である。

## 13.2 prospectively frozen search queries

formal search前に以下10 queryを凍結した。

| ID | Query | Scope |
|---|---|---|
| Q01 | 阿蘇 草原 野焼き 管理 許可 | Aso burning workflow |
| Q02 | 阿蘇 草原 野焼き 安全 管理 | Aso burning safety |
| Q03 | 阿蘇 草原 放牧 管理 計画 | Aso grazing management |
| Q04 | 阿蘇 草原 刈取り 管理 計画 | Aso mowing management |
| Q05 | 熊本県 野焼き 許可 草原 | Kumamoto permission workflow |
| Q06 | 熊本県 草原 保全 管理 計画 | Kumamoto conservation planning |
| Q07 | 環境省 二次草原 管理 指針 | Japan official grassland guidance |
| Q08 | 阿蘇 草原 再生 モニタリング 管理 | Aso monitoring and management |
| Q09 | Aso grassland burning management official | English discovery cross-check |
| Q10 | Kumamoto grassland conservation management official | English discovery cross-check |

## 13.3 stopping rule

停止規則は次のように固定した。

```text
各queryについて最初の10 unique retrievable resultsをscreenする
canonical document / URLでdeduplicateする
最初のquery/rank occurrenceを保持する
最大discovery frame = 100 query-result positions
eligible documentはすべて含める
favorable resultが見つかっても途中停止しない
```

このfinite frameにより、結果に都合のよい文書だけを追加検索し続けることを防いだ。

---

# 14. A5で確認した公開文書

formal retrieval frame内で適格と判断された文書の記述的contextには、以下のような公式・運用資料が含まれた。

| ID | 発行主体 | 文書・ページ | 主なworkflow domain |
|---|---|---|---|
| D01 | 環境省 | 阿蘇くじゅう国立公園 風景地保護協定 | management scope / permission / implementation |
| D02 | 農林水産省 | 世界農業遺産 熊本県阿蘇地域 | management plan / assessment / monitoring / revision |
| D03 | 阿蘇市 | 令和8年一斉野焼きのお知らせ | current weather / safety / operational timing |
| D04 | 南阿蘇村 | 野外焼却・例外規定 | legal exception / safety / fire precaution |
| D05 | 熊本県 | 県立自然公園の許可・届出 | permission / notification / authorization |
| D06 | 環境省 | 阿蘇草原保全計画関連業務 | current state / past management / ecological survey / plan formulation |
| D07 | 環境省 | 阿蘇管理道路調査・設計関連業務 | resource capacity / implementation / safety / planning |
| D08 | 熊本県 | 阿蘇草原維持再生基礎調査 | local management state / actor capacity / problem identification |
| D09 | 阿蘇グリーンストック | 草原保全ボランティア活動 | training / labor capacity / safety |
| D10 | 阿蘇草原再生協議会 | 情報プラットフォーム | data provenance / usage conditions / attribution |
| D11 | 環境省 | 阿蘇くじゅう国立公園の取組 | management difficulty / actor capacity / coordinated action |
| D12 | 環境省 | 自然再生基本方針 | regional context / monitoring / stakeholder process / adaptive management |
| D13 | 環境省 | 阿蘇草原再生全体構想 | restoration planning / stakeholder coordination / monitoring |
| D14 | 環境省 | 阿蘇地域管理運営計画関連資料 | regional planning / deliberation / public comment |
| D15 | 農研機構 | 阿蘇草原植生に対する刈取り時期の影響 | management-response context / timing specificity / cautions |
| D16 | 熊本県 | 阿蘇草原再生全体構想 第3期 | restoration plan / local coordination / management |
| D17 | 環境省 | 阿蘇くじゅう国立公園 保護・規制計画改定関連資料 | protection regulation / land-use context |
| D18 | 熊本県 | 自然環境保全基本方針 | regulation / assessment / monitoring / conservation planning |

これらはA5の利用可能corpusを記述するためのものであり、18文書だけを都合よく選んでformal corpusと定義したものではない。formal ruleはあくまでprospectively frozen 10-query retrieval frameである。

---

# 15. A5のformal executionと結果

formal single-query executionで得られたranked positionsは次であった。

```text
Q01 = 10
Q02 = 10
Q03 = 10
Q04 = 10
Q05 = 10
Q06 =  9
Q07 =  9
Q08 = 10
Q09 = 10
Q10 = 10

TOTAL = 98 / intended 100
```

Q06とQ07はそれぞれ9件しか返却されなかった。

利用可能な適格文書の記述的reviewでは、次がすべて確認された。

```text
reviewScopePlanningRepresented       = true
permissionAuthorizationRepresented  = true
safetyCurrentConditionRepresented   = true
resourceCapacityRepresented         = true
monitoringValidationRepresented     = true
sourceLocalProvenanceRepresented    = true
criticalSemanticConflictsObserved   = 0
```

しかし、formal ruleは「documentary gapをcompatibilityへ変換しない」と事前に規定していた。

検索挙動を見た後で、

```text
別queryを追加する
不足rankだけ再検索する
検索条件を緩める
manual substitutionで10件へ埋める
```

ことは、prospectively frozen retrieval frameの変更になる。

そのため不足2 positionsは補完しなかった。

```text
A5 = INDETERMINATE
```

とした。

これは、

```text
critical conflictが見つかった
```

という結果ではない。

正確には、

> **利用可能な適格文書ではcritical semantic conflictを観察しなかったが、凍結済みformal retrieval frameを完全に実行できなかったため、compatibilityのformal stateを確定しなかった。**

という結果である。

---

# 16. Study 2Aのformal outcome

formal resultは次のとおりである。

```text
A1 traceability                    PASS
A2 evidence-boundary cues         PASS
A3 baseline equivalence           PASS
A4 adversarial consistency        PASS
A5 documentary compatibility      INDETERMINATE

OVERALL                            INDETERMINATE
```

overall ruleでは、A5をformalに解釈できない状態で `READY_FOR_HUMAN_VALIDATION` を付与しない。

同時に、A1–A4にcritical failureはなく、A5で `CRITICAL_CONFLICT` を観察したわけでもないため、`NOT_READY_FOR_HUMAN_VALIDATION`にも分類しない。

したがってformal outcomeは、

> **INDETERMINATE（判定不能）**

である。

`INDETERMINATE`はStudy 2Aが未完了という意味ではない。事前に定めたdecision ruleを適用した結果、「READY / NOT_READY / MIXEDのformal assignmentを正当化できない」と判定し、その状態で研究を完了・凍結したという意味である。

---

# 17. 本研究が支持すること

Study 2Aは、少なくとも以下を支持する。

1. Study 1のcritical factsを、15/15について凍結済みsource state / rule / required-input definitionへ追跡できる。
2. 12のcritical unsupported-inference familiesすべてについて、禁止解釈をrepresentationから明示的に棄却できる。
3. Study-1 conditionとconventional baselineを同一16-fact setとしてmaterializeできる。
4. critical omission、addition、state changeを0のままbaseline comparison structureを作れる。
5. 12 boundary families × 3 distinct surface variants = 36 adversarial casesをprospectively凍結できる。
6. 36/36 critical expected interpretationsが内部整合し、invented ecological truthを必要としない。
7. public-document workflow auditをfinite search frameとしてprospectively規定できる。
8. retrieval shortfallが生じても、それをpost hoc searchで都合よく修復しないdecision ruleを実行できる。
9. available official/operational documentary materialにおいて、Study 1 boundaryのcritical semantic inversionは観察されなかった。
10. non-participant evidenceとhuman-participant evidenceをformal governance上分離できる。
11. `INDETERMINATE`をnegative/positive方向へsoftenせず、valid final resultとして凍結できる。

---

# 18. 本研究が支持しないこと

Study 2Aは以下を主張しない。

- practitionerがStudy 1 representationを正しく理解できる
- practitionerのunsupported-inference errorが減る
- practitionerがこのrepresentationを好む
- real-world decision qualityが改善する
- decision speedが改善する
- administrative workflowへ実際に適合する
- organizational adoptionが進む
- cognitive burdenが低い
- human confidence calibrationが改善する
- management interventionがecologically effectiveである
- source-local responseをplanning unitへ転移できる
- G4がvalidatedである
- candidate actionがrecommendedである
- actionがsafeである
- permission / authorizationがある
- required-input数がaction priorityを示す
- optimal management allocationが得られた
- LLM behaviorがpractitioner behaviorを代理する

特に、

```text
human participant evidence = 0
```

である。

---

# 19. 実務的含意

Study 2Aの実務的含意は、「人間に使わせる前に機械的・構造的に確認できること」と「人間に使わせなければ分からないこと」を分離した点にある。

human-facing decision supportを評価する場合、いきなりparticipant studyへ進む前に、少なくとも次を確認できる。

```text
根拠へtraceできるか
禁止推論をrepresentation自体が棄却できるか
比較条件の情報量が等しいか
adversarial caseで内部矛盾しないか
public workflow vocabularyと重大に反転していないか
```

しかし、これらを通過しても、

```text
実際に分かりやすいか
誤読率が低いか
現場で使いやすいか
```

は残る。

Study 2Aは、この残差を「AIなら分かる」「文書上は似ている」といったproxyで閉じなかった。

---

# 20. NAP-002 Study 2Bとの境界

Study 2Bは、実在するpractitioner / administrative decision-makerを対象とするhuman validationである。

現在の状態は、

```text
NAP-002 Study 2B
DEFERRED / NOT STARTED
```

である。

Study 2Bが扱う予定の主要domainは次である。

```text
comprehension
evidence-boundary interpretation
critical unsupported-inference error
required-input identification
rationale traceability
usability
cognitive burden
workflow fit
confidence calibration
```

Study 2Aの結果を用いて、これらのhuman endpointを「実質的に検証済み」と扱ってはならない。

また、Study 2Aが `INDETERMINATE` であるため、`READY_FOR_HUMAN_VALIDATION`も`NOT_READY_FOR_HUMAN_VALIDATION`も正式にはassignされていない。

将来Study 2Bを実施する場合は、participant access、ethics/privacy、eligibility、instrument、randomization、sample/statistical plan等を独立にprospectively管理する必要がある。

---

# 21. G4 Response-Transfer Validationとの関係

Study 2A / 2Bはhuman-facing decision supportの検証系列である。

一方、Study 1で最も重要な科学的barrierの一つは、

```text
source-local response
!=
planning-unit response
```

というG4 transferabilityである。

G4はhuman usabilityとは別の科学的問題であるため、Study 2Bをdeferredのまま残し、独立したprospective G4 Response-Transfer Validation Studyへ進むことは可能である。

ただし、G4 studyはStudy 2Bのhuman validationが完了したと仮定してはならない。

逆に、将来Study 2Bがpositiveであっても、それによってG4がvalidatedになるわけではない。

```text
human usability
!=
ecological response transferability
```

である。

---

# 22. 研究上の限界

## 22.1 人間参加者を使用していない

Study 2Aの最大の限界であり、同時に研究identityそのものである。

comprehension、preference、usability、workflow fit、cognitive burden等のhuman endpointは測定していない。

## 22.2 A4はsynthetic adversarial consistencyである

36 casesはrepresentationのlogical consistencyを検証するためのsynthetic testであり、実在practitionerのerror distributionを推定するものではない。

## 22.3 A3はbaseline superiorityを示さない

A3はfact-set equalityを確認しただけであり、structured representationがflat baselineより人間にとって優れていることを示さない。

superiorityを検証するにはhuman task performanceが必要である。

## 22.4 A5はdocumentary compatibilityでありhuman acceptanceではない

行政・運用文書で用語やworkflow conceptが共存していても、実際の担当者がStudy 1 representationを自然に理解できるとは限らない。

## 22.5 A5 retrieval frameが完全実行できなかった

凍結済みtarget 100 positionsに対し98 positionsしか取得できなかったため、A5はformalに判定不能となった。

このlimitationを後から検索条件変更で消していない。

## 22.6 search-engine return behaviorは研究対象そのものではない

A5の `INDETERMINATE` は、行政文書が2件不足していることを意味しない。formal retrieval interfaceが凍結されたquery executionに対して10 resultsを返さなかったqueryが2本あった、というprotocol-execution limitationである。

## 22.7 Study 1の科学的endpointを再検証していない

Study 2AはG1–G8のecological / operational truthを新規に測定した研究ではない。Study 1のfrozen representationをread-only predecessorとして扱った。

---

# 23. 第三者資料・データの取扱い

本public reportは単体公開を想定し、第三者著作物や個人情報の再配布を必要最小限にしている。

本レポートには、

- 原著論文PDFそのもの
- 行政文書PDFそのもの
- 行政文書の図表画像
- 大量の逐語引用
- restricted/private raw data
- sensitive rare-species coordinates
- practitioner個人情報
- participant data
- organization-identifiable confidential data

を収録していない。

A5で用いた公開文書については、issuer、title、URL、workflow domain等のprovenanceをrepository内のmachine-readable registerへ保持している。

Study 2Aではpublic institutional documentsを主対象とし、個人のpublic commentや個人SNS投稿をhuman-behavior evidenceとして扱っていない。

---

# 24. 再現性とclosure audit

## 24.1 formal resultの再構成

凍結済みartifactから再計算したgate statesは次である。

```text
A1 = PASS
A2 = PASS
A3 = PASS
A4 = PASS
A5 = INDETERMINATE

recomputed formal outcome = INDETERMINATE
stored formal outcome     = INDETERMINATE
```

reproducibility auditでは、

```text
passed                          = true
formalResultReproducible        = true
inputsFrozenBeforeRelevantScoring = true
decisionRuleConsistent          = true
postHocA5GapFill                = false
llmFormalInferenceUsed          = false
humanParticipantDataUsed        = false
study1EndpointsModified         = false
study2BStarted                  = false
interpretationCeilingPreserved  = true
```

を確認した。

## 24.2 adversarial closure audit

closure時には、結果の解釈が都合よく拡張されていないかを別途監査した。

以下はすべて `false` である。

```text
A5 missing ranksをcompatibilityへ変換
no-conflict observationをREADYへ昇格
INDETERMINATEをsecondary evidenceでsoften
Study 1 formal endpointsを書換え
Study 2Aをhuman validationとして報告
practitioner usability claim
G4/ecological validation claim
management recommendation / authorization
LLMをhuman surrogateとしてformal利用
Study 2Bを暗黙に完了扱い
unknownをzero/safeへ変換
public no-recordをecological absenceへ変換
```

adversarial closure auditは `PASS` であり、closureはauthorizedとなった。

---

# 25. 再現性識別子

Study 2Aの主要なfrozen identityを以下にまとめる。

## Study identity

```text
NAP002-STUDY2A-NONPARTICIPANT-PREVALIDATION-2026-08-14-v1
```

## scientific-state snapshot

```text
repository = nkkmd/natural-area-planning
study-state snapshot commit = db83cae09e9b84a6b9deab46d87af6c36e9cfa49
study date = 2026-08-14 JST
```

## pre-evaluation materialization

```text
facts = 16
critical facts = 15
boundary families = 12
adversarial cases = 36
variants per boundary = 3
public-document queries = 10
maximum discovery frame = 100
pre-evaluation audit = PASS
```

## formal A1-A4 artifact identity

```text
2445d07d93f47a7f9fba817df1e6eb74b17579f3
```

## formal A5 artifact identity

```text
95289455433ff3f0fa0cf1bb9fe00558b1b6c56e
```

## formal result artifact identity

```text
18bf2e864c1fad7c4dfbeb46aacdf2c025e0ac5d
```

## final outcome

```text
A1 PASS
A2 PASS
A3 PASS
A4 PASS
A5 INDETERMINATE
OVERALL INDETERMINATE
```

---

# 26. Repository内の主要な再現性資料

このレポート単体で主要結論を理解できるが、完全なprovenanceとmachine auditはrepositoryで追跡できる。

## prospective design

```text
doc/nap002/study2/study2a/NAP002_STUDY2A_PROSPECTIVE_RESEARCH_PROTOCOL.md
doc/nap002/study2/study2a/FORMAL_DECISION_GATES.md
doc/nap002/study2/study2a/PUBLIC_DOCUMENT_AUDIT_PROTOCOL.md
doc/nap002/study2/study2a/MATERIALIZATION_AND_REPRODUCIBILITY_PLAN.md
doc/nap002/study2/study2a/DECISION_REGISTER.md
```

## pre-evaluation materialization

```text
analysis/nap002/study2a/study2a_design_registry.json
analysis/nap002/study2a/materialization/atomic_fact_registry.csv
analysis/nap002/study2a/materialization/study1_test_condition.json
analysis/nap002/study2a/materialization/baseline_condition.json
analysis/nap002/study2a/materialization/boundary_cue_registry.csv
analysis/nap002/study2a/materialization/adversarial_case_registry.csv
analysis/nap002/study2a/materialization/adversarial_expected_key.csv
analysis/nap002/study2a/materialization/public_document_search_register.csv
analysis/nap002/study2a/materialization/pre_evaluation_materialization_audit.json
```

## formal results

```text
analysis/nap002/study2a/results/a1_a4_formal_audit.json
analysis/nap002/study2a/results/a5_retrieval_execution_audit.json
analysis/nap002/study2a/results/a5_observed_documentary_context.csv
analysis/nap002/study2a/results/a5_public_document_compatibility_audit.json
analysis/nap002/study2a/results/study2a_formal_result.json
analysis/nap002/study2a/results/study2a_reproducibility_audit.json
analysis/nap002/study2a/results/study2a_adversarial_closure_audit.json
```

## checkpoints

```text
doc/nap002/study2/checkpoints/2026-08-14-study2-split-into-2a-2b.md
doc/nap002/study2/checkpoints/2026-08-14-study2a-prospective-design-freeze.md
doc/nap002/study2/checkpoints/2026-08-14-study2a-pre-evaluation-materialization-freeze.md
doc/nap002/study2/checkpoints/2026-08-14-study2a-pre-scoring-materialization-correction.md
doc/nap002/study2/checkpoints/2026-08-14-study2a-a1-a4-formal-audit.md
doc/nap002/study2/checkpoints/2026-08-14-study2a-a5-formal-audit.md
doc/nap002/study2/checkpoints/2026-08-14-study2a-formal-closure.md
```

---

# 27. 今後の研究

## 27.1 Study 2B — human validation

実在practitioner / administrative decision-makerを対象に、comprehension、critical unsupported-inference error、required-input identification、rationale traceability、usability、workflow fit等を検証する。

現在は `DEFERRED / NOT STARTED` である。

## 27.2 G4 Response-Transfer Validation

Study 1の主要科学的barrierであるresponse transferabilityを扱う独立prospective studyである。

少なくとも、

```text
source / target comparability domain
management-definition compatibility
target / outcome definition
context modifiers
transfer criteria
explicit non-transfer state
analysis plan
external / temporal validation
```

等を結果を見る前に固定する必要がある。

## 27.3 operational pilot

将来、real decision-time local/current inputsを扱う場合は、Study 1/2Aとは別のprospective operational protocolが必要になる。

permission、safety、weather、site condition等のcurrent factsをhistoric public evidenceで代替してはならない。

## 27.4 Future Stage A / B

restricted/local empirical dataや新規prospective field/monitoringを使う研究は、NAP-001 public-only Stage Cのhistorical endpointを変更せず、別studyとして管理する。

---

# 28. 結論

NAP-002 Study 2Aは、NAP-002 Study 1で構築された`WHERE / WHY / WHAT NEXT`型意思決定支援表現について、実在する人間参加者を用いずに実施可能なpre-validationをprospectively設計・実行した。

Study 2Aでは、formal scoring前にtest materialsとdecision rulesを凍結し、

```text
16 atomic facts
15 critical facts
12 critical boundary families
36 adversarial cases
10 frozen documentary queries
```

を用いて評価した。

その結果、

```text
A1 traceability                    PASS
A2 evidence-boundary cues         PASS
A3 baseline equivalence           PASS
A4 adversarial consistency        PASS
A5 documentary compatibility      INDETERMINATE
OVERALL                            INDETERMINATE
```

となった。

A1–A4ではcritical structural defectを確認しなかった。A5の利用可能文書でもcritical semantic conflictは観察されなかった。しかし、prospectively frozen 100-position retrieval frameのうちformal executionで98 positionsしか得られなかったため、不足2 positionsをpost hocに補完せず、A5とoverallを `INDETERMINATE` のまま閉じた。

この結果は、人間validationの代替ではない。

Study 2Aの最終到達点は、次の一文に要約できる。

> **人間に使わせる前に、根拠追跡性、禁止推論の明示、比較条件の情報等価性、adversarialな論理整合性、公開workflowとの意味的衝突を非参加者型に点検できる。しかし、人間が実際に理解し、誤読せず、使いやすいかという問いは、人間参加者なしには閉じてはならない。**

Study 2Aが `INDETERMINATE` で完了したこと自体も、この原則の一部である。検索結果が2 positions不足したとき、都合よくretrieval ruleを変更せず、formal decisionを確定しないという選択を保持した。

---

# 用語

**non-participant pre-validation**  
実在する研究参加者からperformance / preference / usability dataを取得せず、representationやprotocol自体の必要条件を事前検証すること。本研究ではhuman validationの代替を意味しない。

**traceability**  
評価対象のclaimやstateが、凍結済みStudy 1のrule、state、required-input definitionへ追跡できる性質。

**atomic fact**  
baseline condition間で情報等価性を監査するために分解した、最小単位のdecision-relevant fact。

**boundary cue**  
`candidate action != recommendation`等、evidence ceilingを越える誤推論を明示的に拒否するための表現上の手掛かり。

**adversarial case**  
誤読・境界越え・論理矛盾を誘発するよう設計したsynthetic test case。

**information-equated baseline**  
Study-1 structured conditionと同一のatomic factsを保持し、organization / presentationだけを変えた比較条件。

**documentary compatibility**  
公開行政・運用文書のterminology / workflowと、Study 1のconceptual separationが重大に逆転していないかというdocument-level property。human acceptanceを意味しない。

**INDETERMINATE**  
凍結済みruleに従ってformal decisionを確定するためのmaterial / execution条件が不足した状態。本研究ではA5 retrieval frameの未完遂によりassignされた。研究未完了を意味しない。

**Study 2B**  
実在practitioner / administrative decision-makerを対象とする将来のhuman validation study。Study 2Aとは別研究である。

---

# Selected methodological references / prior art

Study 2Aは、decision support、decision science、uncertainty representation、map-based decision support等に広いprior artがあることを前提とし、一般的方法論のnoveltyを主張しない。

- Martin et al. (2009). Structured decision making / decision-threshold literature. DOI: `10.1890/08-0255.1`
- Hemming et al. (2022). Decision science in conservation. DOI: `10.1111/cobi.13868`
- Regan et al. (2005). Robust conservation decision making under severe uncertainty. DOI: `10.1890/03-5419`
- Korporaal, Ruginski & Fabrikant (2020). Effects of uncertainty visualization on map-based decisions. DOI: `10.3389/fcomp.2020.00032`
- Arciniegas, Janssen & Rietveld (2013). Collaborative map-based spatial decision support evaluation. DOI: `10.1016/j.envsoft.2012.02.021`

Companion predecessor reports:

- Natural Area Planning / NAP-001 — Public Research Report, `doc/publication/NAP001_PUBLIC_RESEARCH_REPORT.md`
- Natural Area Planning / NAP-002 Study 1 — Public Research Report, `doc/publication/NAP002_STUDY1_PUBLIC_RESEARCH_REPORT.md`

A5 documentary contextの完全なmachine-readable provenance:

- `analysis/nap002/study2a/results/a5_observed_documentary_context.csv`
- `analysis/nap002/study2a/materialization/public_document_search_register.csv`

---

## 引用時の推奨表記

本レポートを引用する場合は、少なくとも次の情報を含めることを推奨する。

```text
Natural Area Planning / NAP-002 Study 2A (2026).
Public Research Report:
人間参加者を用いずに意思決定支援表現をどこまで事前検証できるか
— traceability・誤推論耐性・情報等価性・公開行政文書との整合性を検証する —.
Version 1.0, 2026-08-14.
Formal outcome: INDETERMINATE.
Study snapshot: nkkmd/natural-area-planning @ db83cae09e9b84a6b9deab46d87af6c36e9cfa49.
```

この `Study snapshot` はStudy 2Aの凍結された科学的状態を指す。その後のnavigation更新、日本語化、public-report format整備等のeditorial-only commitは、Study 2Aのformal scientific outcomeを変更しない。

著者名、所属、DOI、恒久公開URL等が後日正式に付与された場合は、それらを優先して引用情報へ追加する。