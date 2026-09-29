---
name: computation
description: |
  当用户要判断一件事「能不能用计算机做、要花多大代价、边界在哪里」时调用。
  覆盖《计算》(吴翰清) 的方法面：计算概念定层与混淆诊断、可计算性→代价→NP 完全三级筛选、计算系统的先天约束清单、编码—规则—初始状态三步建模、自指系统的编码+对角线+归谬诊断、形式系统四性质体检、有穷化改写、哥德尔式编码、图灵完备判定、问题复杂度类定位七步、归约论证、近似算法与启发式设计、并行化加速比、量子计算宣传可信度、复杂系统诊断、组织与智能系统的计算化翻译、问题定义与排障、争议问题资产化、计算主义与机器智能的信念边界。
  不适用于：具体编程语言与框架的教学、算法实现与性能调优细节、哲学立场裁断、需要临床/法律/金融等专业资质的处置。
metadata:
  cangjie.generated-by: cangjie-tools v2.5.0
  cangjie.variant: single
  cangjie.bundle-id: bundle.computation
  cangjie.capability-count: 27
  cangjie.entrypoint-count: 1
---
# 《计算》 — 全书能力入口

## 触发与不触发

**适用**：与本书能力域相关的咨询与任务（见下方路由表的意图列）。
**不适用**：
- 具体编程语言、框架与工程实现的教学（本书不教写代码）
- 算法实现细节与性能调优操作（缓存、JIT、具体库用法等）
- 数学本体论与心身问题的立场裁断（作者自陈这些是「只有猜想」的问题）
- 需要临床、法律、金融等专业资质的处置

## 核心原则（常驻速览，概览类问题读到这里即可回答）

1. 书里的「计算」对应 computing（可由机器执行的符号变换过程），不是计算器意义上的 calculation——凡涉及「能不能算」的讨论，先统一到这个口径（L1007-1009）。
2. 计算的边界是先天继承的：现代计算机基于通用图灵机，因此继承不完备性、停机问题与判定问题的全部约束（L517-535）。
3. 能算 ≠ 算得动：可计算性与计算复杂性是两级筛子，先判「能不能」，再判「要多少代价」（L527-529）。
4. 形式化的前提是剥离符号的意义、只保留变换规则——这也是把推理交给机器的前提（L495）。
5. 自指既是一切悖论的根源，也是边界证明的引擎：编码 → 对角线 → 归谬，是诊断任何自指系统界限的统一三件套（L477-507）。
6. 不可能性结论在工程上的用法是「先停止浪费时间」的判据：NP 完全题要接受近似，并行有加速上限，量子计算置换的是物理基底而非数学边界。
7. 依赖未证明假设的推论（P≠NP、BPP≠BQP、单向函数存在、宇宙是图灵机）只能作条件句使用，不得当作已证前提。

## 能力路由（先读本表，按意图加载 1 张能力卡）

| 用户意图 | 先读 | 补读/备注 |
|---|---|---|
| 定位一个「计算」话题属于原理/技术实现/影响中的哪一层；诊断一场计算争论中的概念混淆；判断某个说法是事实问题还是信念问题；对齐团队里「计算/算力/算法」的口径；给写作或分享找一个讲清「计算」的入口框架 | references/capabilities/concept-positioning.md | references/capabilities/abstraction-history.md、references/capabilities/computationalism-boundary.md、references/capabilities/machine-intelligence-paradigms.md、references/capabilities/problem-definition-troubleshooting.md、references/capabilities/research-paradigm-criteria.md |
| 定位一个记号、算法或教材写法在抽象史中的层次；把一堆特例改写成一般规则（或把实物台账改成符号化编码）；判断某个被沿用的约束是有效约束还是历史包袱；讲解计算/数学史的抽象递进（计数→符号→代数→公理） | references/capabilities/abstraction-history.md | references/capabilities/concept-positioning.md、references/capabilities/foundations-infinity.md、references/capabilities/recursion-iteration.md、references/capabilities/research-paradigm-criteria.md、references/capabilities/symbolic-logic-calculus.md |
| 判断某个问题能不能用程序算出来；判断某个算法/需求是否值得投入算力；区分「算法跑得慢」与「问题本身难」；立项前对一批需求做可行性快筛；为「必须转近似求解」这一结论找依据 | references/capabilities/three-tier-screening.md | references/capabilities/inherent-constraints.md、references/capabilities/problem-definition-troubleshooting.md、references/capabilities/quantum-credibility.md、references/capabilities/reduction-argument.md、references/capabilities/turing-completeness.md |
| 给系统或平台写「先天约束说明」；判断某条限制换硬件是否能解决；区分架构约束与模型约束；核查「图灵机受哥德尔不完备性定理约束」这类说法；评估某任务的物理量级上限（能耗 / 速度 / 信息量） | references/capabilities/inherent-constraints.md | references/capabilities/approximation-heuristics.md、references/capabilities/parallelization-speedup.md、references/capabilities/quantum-credibility.md、references/capabilities/three-tier-screening.md、references/capabilities/turing-completeness.md |
| 把业务流程或规则体系改写为可计算模型；决定对象该怎么编码、状态该怎么表示；判断一套规则体系能否交给机器执行；建模前登记假设与适用边界；决定编码时该剔除哪些细节 | references/capabilities/encoding-rule-initial-state.md | references/capabilities/complexity-class-locating.md、references/capabilities/godel-encoding.md、references/capabilities/reductio-proof.md、references/capabilities/symbolic-logic-calculus.md、references/capabilities/turing-completeness.md |
| 设计递归或迭代的逼近求解流程；决定用递归还是递推、停止条件怎么定；估算递归方案的步数与终止性；给自相似问题（分治/树遍历）写分解步骤 | references/capabilities/recursion-iteration.md | references/capabilities/abstraction-history.md、references/capabilities/approximation-heuristics.md、references/capabilities/complex-system-diagnosis.md、references/capabilities/problem-definition-troubleshooting.md、references/capabilities/self-reference-limits.md |
| 证明某事物不存在或做不到；从对方前提引出矛盾以反驳论证；核对一个不存在性结论的前提是否可靠；判断某条逻辑规则能否用在无穷对象上 | references/capabilities/reductio-proof.md | references/capabilities/encoding-rule-initial-state.md、references/capabilities/paradox-assetization.md、references/capabilities/reduction-argument.md、references/capabilities/relative-consistency.md、references/capabilities/self-reference-limits.md |
| 诊断一套自指规则体系（评级/权限/合规/自评）的界限；判断一个形式系统或程序能否判定自己；逐行复核哥德尔不完备、罗素悖论、实数不可数这类构造；检查「所有」「一切」式总体表述会不会自指塌陷；核对「系统已自证一致」这类断言是否成立 | references/capabilities/self-reference-limits.md | references/capabilities/formal-system-checkup.md、references/capabilities/godel-encoding.md、references/capabilities/paradox-assetization.md、references/capabilities/recursion-iteration.md、references/capabilities/reductio-proof.md |
| 为「某系统一致/无矛盾」构造归约链并标出终点；核对一条「A 的一致性由 B 保证」的结论是相对还是绝对；判断某个基础性断言该不该改用模型化方法处理；评估删改某条公理（如选择公理）的后果；解释为什么「用了很多年没出问题」不等于已证明一致 | references/capabilities/relative-consistency.md | references/capabilities/finitization-rewrite.md、references/capabilities/formal-system-checkup.md、references/capabilities/foundations-infinity.md、references/capabilities/reductio-proof.md、references/capabilities/reduction-argument.md |
| 检查一套公理系统或规范体系是否可靠；找出规则体系里多余或相互冲突的条款；判断某个形式系统能否自证一致性；判断是否存在通用方法自动判定合规性；评估规范是否被要求承诺「覆盖所有情况」 | references/capabilities/formal-system-checkup.md | references/capabilities/finitization-rewrite.md、references/capabilities/godel-encoding.md、references/capabilities/relative-consistency.md、references/capabilities/self-reference-limits.md、references/capabilities/symbolic-logic-calculus.md |
| 把无法验证的全称断言改写成可检查的表述；为规范或合同条款写可验收的条件；把「理论上一定存在」的论证补成可检验形式；区分一句话要遍历无穷多对象还是只需查有限区间 | references/capabilities/finitization-rewrite.md | references/capabilities/formal-system-checkup.md、references/capabilities/foundations-infinity.md、references/capabilities/paradox-assetization.md、references/capabilities/relative-consistency.md |
| 为一套符号系统设计可逆的编码规则；判断一条自指表达式是良性还是恶性；让系统能够谈论它自己；检查一个正整数是否为有效编码并还原表达式 | references/capabilities/godel-encoding.md | references/capabilities/encoding-rule-initial-state.md、references/capabilities/formal-system-checkup.md、references/capabilities/self-reference-limits.md、references/capabilities/symbolic-logic-calculus.md、references/capabilities/turing-completeness.md |
| 判定一门新语言或 DSL 是否图灵完备；用五元组描述一台图灵机；判断某个系统的表达能力上限；区分"有规则"与"能停机"（循环机）；把五元组编码成描述数 | references/capabilities/turing-completeness.md | references/capabilities/encoding-rule-initial-state.md、references/capabilities/godel-encoding.md、references/capabilities/inherent-constraints.md、references/capabilities/parallelization-speedup.md、references/capabilities/three-tier-screening.md |
| 给一个具体问题定复杂度类；判断一个问题是不是 NP 完全；判断问题属于 P 还是 NP；定位未分类问题（因数分解 / 图同构）；判断该找精确算法还是转近似；复核复杂度类结论的依据链与前提 | references/capabilities/complexity-class-locating.md | references/capabilities/encoding-rule-initial-state.md、references/capabilities/parallelization-speedup.md、references/capabilities/problem-definition-troubleshooting.md、references/capabilities/quantum-credibility.md、references/capabilities/reduction-argument.md |
| 判断某问题能否归约到 SAT 或 3SAT；判断某问题是否至少和已知难题一样难；检查一段「把 P 化成 Q」的说明是否够格称归约；用反证法排除某条技术路线的可行性；论证某能力不可能存在高效算法 | references/capabilities/reduction-argument.md | references/capabilities/approximation-heuristics.md、references/capabilities/complexity-class-locating.md、references/capabilities/reductio-proof.md、references/capabilities/relative-consistency.md、references/capabilities/three-tier-screening.md |
| NP 难问题求可行近似解；用线性规划建模近似一个问题并检验模型；评估近似结果的误差与近似因子；设计启发式算法（模拟退火 / 遗传 / 局部搜索）；判断还能不能继续优化近似算法 | references/capabilities/approximation-heuristics.md | references/capabilities/inherent-constraints.md、references/capabilities/parallelization-speedup.md、references/capabilities/problem-definition-troubleshooting.md、references/capabilities/recursion-iteration.md、references/capabilities/reduction-argument.md |
| 判断某任务或算法能否并行化；评估并行改造的加速比与收益上限；解释「加了机器却没快多少」的原因；判断某问题是否固有串行（P 完全）；云／集群架构的容量与数据位置决策 | references/capabilities/parallelization-speedup.md | references/capabilities/approximation-heuristics.md、references/capabilities/complexity-class-locating.md、references/capabilities/inherent-constraints.md、references/capabilities/quantum-credibility.md、references/capabilities/turing-completeness.md |
| 判断某项量子计算/量子霸权的宣传或投资材料是否可信；评估量子计算机对本行业问题（优化/搜索/密码）的实际威胁与时机；核对「量子比特数」「指数级加速」等指标的说法是否有依据；评估现有加密体系是否需要迁移到抗量子方案；分清量子计算材料里「已被验证的能力」与「理论上的归约」 | references/capabilities/quantum-credibility.md | references/capabilities/complexity-class-locating.md、references/capabilities/inherent-constraints.md、references/capabilities/parallelization-speedup.md、references/capabilities/three-tier-screening.md |
| 判断某个系统的不可预测性是系统固有还是建模不足；评估「拆到最小单元即可理解整体」类方案在复杂对象上是否可行；诊断一个组织/网络系统为什么僵化或为什么能持续产生创新；评估控制系统/风控机制的行为多样性是否足以应对环境扰动；为「复杂结构没有捷径」「加算力也不能预测一切」类判断找依据 | references/capabilities/complex-system-diagnosis.md | references/capabilities/machine-intelligence-paradigms.md、references/capabilities/organization-computationalization.md、references/capabilities/recursion-iteration.md |
| 把公司/团队翻译成计算系统并做治理评估；判断这套使命愿景价值观是不是一个反馈闭环；评估一个「智能系统」是真智能还是信息化系统；组织变大后管理失控的诊断与治理方式切换；为团队设计目标函数、反馈回路与落地次序 | references/capabilities/organization-computationalization.md | references/capabilities/complex-system-diagnosis.md、references/capabilities/machine-intelligence-paradigms.md、references/capabilities/research-paradigm-criteria.md |
| 把一团模糊的困惑定义成可解的问题；区分现象和问题，找出可改变的关键点；排障卡住时给思考定次序（怀疑→分解→由易到难→复查）；判断问题该放弃还是改写成近似问题；挖根因并验证候选根因 | references/capabilities/problem-definition-troubleshooting.md | references/capabilities/approximation-heuristics.md、references/capabilities/complexity-class-locating.md、references/capabilities/concept-positioning.md、references/capabilities/recursion-iteration.md、references/capabilities/three-tier-screening.md |
| 判断一个争了几十年的老问题还值不值得投入；判别需求/架构文本里的「无限」「无限精度」是过程还是实体；找出一个悖论式规则的结构问题（自我指涉/否定/潜无穷做实无穷处理）；归档一场长期争论的各方法反驳范式 | references/capabilities/paradox-assetization.md | references/capabilities/finitization-rewrite.md、references/capabilities/foundations-infinity.md、references/capabilities/reductio-proof.md、references/capabilities/self-reference-limits.md |
| 判断某条计算主义式论断能不能写进交付物；区分书中结论、作者信念与作者强推测；回答「机器能思考吗／宇宙是台计算机吗」这类立场问题；给「算力衡量价值」「犯罪源于单向函数」类说法做强度标定；检查引用是否丢掉了条件句前件 | references/capabilities/computationalism-boundary.md | references/capabilities/concept-positioning.md、references/capabilities/foundations-infinity.md、references/capabilities/machine-intelligence-paradigms.md |
| 在符号规则、训模型、改提示、做 Agent 之间选型；排障：专家系统规则膨胀无法维护；解释 GPT 与 BERT 的路线差别、何时该微调；定位提示强度（zero-shot / one-shot / few-shot / fine-tune）；评估大模型业务的门槛：数据、成本、能耗 | references/capabilities/machine-intelligence-paradigms.md | references/capabilities/complex-system-diagnosis.md、references/capabilities/computationalism-boundary.md、references/capabilities/concept-positioning.md、references/capabilities/organization-computationalization.md、references/capabilities/research-paradigm-criteria.md |
| 判断「算力/大模型带来科研范式革命」这类主张落在哪一档；评估评审标准或研究方向是否被上一代成功经验锁定；把附录A/附录C 的史观与产业判断与学科共识分开标注；引用「近代科学的起点」「科学范式演化」这类说法前的口径确认 | references/capabilities/research-paradigm-criteria.md | references/capabilities/abstraction-history.md、references/capabilities/concept-positioning.md、references/capabilities/machine-intelligence-paradigms.md、references/capabilities/organization-computationalization.md |
| 把散落在自然语言里的推理规则机械化；把命题译成符号表达式再做演算；判断现有工具的表达力缺口在哪；把混乱的规则体系整理成公理加演绎；为符号体系补一层取值解释使其可被机器执行 | references/capabilities/symbolic-logic-calculus.md | references/capabilities/abstraction-history.md、references/capabilities/encoding-rule-initial-state.md、references/capabilities/formal-system-checkup.md、references/capabilities/godel-encoding.md |
| 解释数学基础是怎么严格化起来的；判别一个表述里的无穷是潜无穷还是实无穷；核对 ZFC 公理底表与某问题依赖哪条公理；解释无理数的定义与实数不可数；判断某条公理能否删改及其后果 | references/capabilities/foundations-infinity.md | references/capabilities/abstraction-history.md、references/capabilities/computationalism-boundary.md、references/capabilities/finitization-rewrite.md、references/capabilities/paradox-assetization.md、references/capabilities/relative-consistency.md |

**非能力类查询**：
- 书名/作者/章节/整书概览 → references/overview.md
- 术语解释 → references/glossary.md
- 决策规则速查（不需要原文依据时） → references/cheatsheet.md
- 完整意图与关键词索引（本表未覆盖的意图先查这里） → references/capability-index.md

## 加载规则

- 每次任务先读本文件，再按路由表加载 **1** 张能力卡；任务明确跨域时最多加载 2 张。
- 概览/书名类问题不加载能力卡，用「核心原则」与 overview.md 回答。
- 路由表与 capability-index.md 都无法命中的意图，明确告知超出本书范围，不要硬套。

## 边界与判停

- 用户要的是「帮我把这道题算出来」而不是判断其边界时，停止本流程，直接解题。
- 涉及公开未决问题（P vs NP、宇宙是否图灵机、实无穷是否实在）时，停止把猜想当前提，明确标注未决。
- 用户要求裁断某条论断的哲学立场对错时，停止，只输出支撑强度与作者主张标签（L539-543）。
- 遇到 OCR 残缺的公式或指数数值（NR-04 / NR-06）时，停止数值复算，只记结构并标待核查。
