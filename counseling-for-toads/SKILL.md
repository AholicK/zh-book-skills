---
name: counseling-for-toads
description: |
  当用户想理解自己的情绪反应与行为模式、处理自我关系，或想帮助情绪低落的人时调用：判断自我状态、识别讨好与顺从、把责怪转为负责、处理愤怒与自我批判、识别人生坐标与心理游戏、准备心理咨询、判断抑郁信号与求助时机、在暴力与人身安全威胁下先判停并转介。
  覆盖《蛤蟆先生去看心理医生》的全部方法论：自我状态三位一体、童年适应模式、愤怒释放、法官与囚徒、人生坐标与自证预言、心理游戏与共谋、情绪温度计、情绪联结、人生故事整合、成长路径与情商。
  ⚠️ **涉及自杀话题的求助，本 skill 适用，不要判为不适用**：「他总说活着没意思」「怀疑自己或家人抑郁了」「怎么开口问对方自杀的事」→ 走 `depression-warning-signs` 卡（识别警示信号 → 直问自杀念头 → 转介专业帮助）。
  急性情形**同样适用**：已出现**当前**念头、具体计划或手段、无法保证安全、亲友失联 → **仍选本 skill**，但第一产出是**紧急求助**（**全国统一心理援助热线 12356**／专业机构／紧急服务／身边可信的人陪同），不要停在分析。
  ⚠️ **涉及暴力、虐待或人身安全威胁的求助，本 skill 同样适用，不要判为不适用或 none**：「我老公昨天动手打了我」「他拿刀威胁我」「我现在不敢回家」「我被家里人打了」→ **仍选本 skill**，走 `depression-warning-signs` 卡的安全判停路径；**第一产出是确保人身安全与转介**（离开危险现场／联系可信的人陪同／联系警方与紧急服务／专业机构），**不是**书里的情绪分析——先安全、后理解，顺序不能颠倒。
  不适用于：临床诊断与用药；**替代**危机干预、急诊与专业处置的**实际执行**（这些由现实机构完成）；本地求助资源与**地方**热线信息（书中没有，不代补、不编造）——**但全国统一心理援助热线 12356 除外**：它是可核查的公开事实，转介时应当告知并标注来源；本书不构成医疗建议。
metadata:
  cangjie.generated-by: cangjie-tools v2.5.0
  cangjie.variant: single
  cangjie.bundle-id: bundle.counseling-for-toads
  cangjie.capability-count: 20
  cangjie.entrypoint-count: 1
---
# 《蛤蟆先生去看心理医生》 — 全书能力入口

## 触发与不触发

**适用**：与本书能力域相关的咨询与任务（见下方路由表的意图列）。
**不适用**：
- 临床心理诊断、药物治疗与精神科处置（书中完全没有；本书不构成医疗建议）
- **替代**危机干预、急诊与专业治疗的实际执行——本 skill 有安全筛查与转介卡（`depression-warning-signs`），急性情形的执行由现实机构完成；但**出现急性情形时本 skill 仍然适用**（见下方 ⚠️），不要判为不适用
- 本地求助资源、**地方**热线与医疗机构信息（书中没有，本流水线不代补，避免编造）
  ——**唯一例外：全国统一心理援助热线 12356**。它是口径明确、可核查的公开事实（国家卫生健康委协调工业和信息化部设置，
  2025-05-01 起全国统一启用），**不属代补**：转介时**应当**告知并标注来源，但**不得**由此推断出任何地方热线或具体机构

> ⚠️ **不要把「话题涉及自杀」列为不适用。** 本书有专门的安全单元：
> 「他总说活着没意思」「怀疑家人抑郁了」「怎么开口问对方自杀的事」→ **适用**，走 `depression-warning-signs`
> （识别警示信号 → 直问自杀念头 → 转介专业帮助）。
> 出现当前念头、具体计划/手段、无法保证安全等**急性**情形时，**本 skill 仍然适用**（不要判为不适用）——
> 只是第一产出换成紧急求助，不停在分析。
>
> ⚠️ **也不要把「正在发生的暴力／虐待／人身安全威胁」列为不适用。**
> 「我老公昨天动手打了我」「他拿刀威胁我」「我现在不敢回家」→ **适用**，走 `depression-warning-signs` 的安全判停路径；
> **第一产出是确保人身安全与转介**（离开危险现场／联系可信的人陪同／联系警方与紧急服务／专业机构），
> **不是**书里的情绪分析——先安全、后理解，顺序不能颠倒。

## 核心原则（常驻速览，概览类问题读到这里即可回答）

1. 三种自我状态人人具备、没有好坏——但只有成人状态能学到关于自我的新知识（儿童状态只是重演，父母状态只是在证明自己）。
2. 童年学到的应对方式（同意/取悦/道歉/依赖）是生存策略而不是性格缺陷；看见它，是它开始松动的条件。
3. 责怪是把主动权留在过去；负责（包括对自己情绪负责）才是成人状态的动作——但负责不是自责。
4. 愤怒不可能被完全压抑，只会以某种方式释放（撒泼、怄气、冷战……）；方向是「不带攻击性地发火」。
5. 最严厉的审判来自内部：自我批判是「挑剔型父母」转向自己；抑郁常与愤怒转向自己有关。
6. 人生坐标与心理游戏靠着自证预言和共谋自我维持——识别它们，是把「总遇到同类的事」从宿命翻译成模式。
7. 帮助他人靠倾听、共情与不给答案，而不是训诫；本书是虚构的咨询示范，其模型是自我反思的启发式工具，不构成医疗建议。

## 能力路由（先读本表，按意图加载 1 张能力卡）

| 用户意图 | 先读 | 补读/备注 |
|---|---|---|
| 判断自己此刻处于哪种自我状态；弄清「我为什么会这样反应」；理解某人反复出现的行为模式；「我这么大个人了怎么还像个小孩」式的退行反应 | references/capabilities/ego-states.md | references/capabilities/adapted-child-patterns.md、references/capabilities/blame-to-responsibility.md、references/capabilities/self-criticism.md、references/capabilities/life-positions.md |
| 弄懂为什么讲道理/劝说没有用；想让某人改变但对方油盐不进；评估「逼他改变」是否可行 | references/capabilities/learning-conditions.md | references/capabilities/ego-states.md、references/capabilities/helping-others.md |
| 总在怪父母/怪别人，走不出来；想停止抱怨循环；面对「凭什么要我改变」的僵局 | references/capabilities/blame-to-responsibility.md | references/capabilities/ego-states.md、references/capabilities/self-criticism.md、references/capabilities/parental-forgiveness.md、references/capabilities/adapted-child-patterns.md |
| 说不出自己是什么感觉；想辨认某个情绪是什么；帮孩子/家人说出情绪 | references/capabilities/basic-feelings.md | references/capabilities/ego-states.md、references/capabilities/adapted-child-patterns.md、references/capabilities/connecting-with-emotions.md、references/capabilities/emotion-thermometer.md |
| 我为什么总是讨好/道歉/不敢拒绝；在强势的人面前自动顺从；识别触发点与童年模式；「某个词/某个场合一出现就本能反应」的触发点与童年来源 | references/capabilities/adapted-child-patterns.md | references/capabilities/ego-states.md、references/capabilities/basic-feelings.md、references/capabilities/blame-to-responsibility.md、references/capabilities/life-story-and-script.md |
| 我从来不发火但会冷战/憋着；生气怎么办；识别愤怒的释放方式与代价 | references/capabilities/anger-release.md | references/capabilities/ego-states.md、references/capabilities/adapted-child-patterns.md、references/capabilities/connecting-with-emotions.md、references/capabilities/self-criticism.md |
| 被父母/上司反复挑毛病；识别某人是否处于挑剔型父母状态；被「我是为你好」困住 | references/capabilities/critical-parent.md | references/capabilities/ego-states.md、references/capabilities/self-criticism.md、references/capabilities/helping-others.md |
| 我总是在心里骂自己；觉得自己没用/不配；自我攻击停不下来；生气之后转向骂自己/自我贬低 | references/capabilities/self-criticism.md | references/capabilities/ego-states.md、references/capabilities/critical-parent.md、references/capabilities/blame-to-responsibility.md、references/capabilities/depression-warning-signs.md |
| 学不进去/看了就烦；觉得自己不适合学这些；学习中突然抵触想放弃 | references/capabilities/learning-resistance.md | references/capabilities/learning-conditions.md、references/capabilities/adapted-child-patterns.md、references/capabilities/ego-states.md |
| 总觉得自己不如别人；为什么我总遇到同样的事；找「我不好」感受的底层定位；为什么总朝同一个方向反应、总是以同样的方式收场 | references/capabilities/life-positions.md | references/capabilities/ego-states.md、references/capabilities/games-and-collusion.md、references/capabilities/game-identification.md |
| 总觉得被针对/被挖坑；重复的负面互动想弄清结构；「他到底想干嘛」 | references/capabilities/game-identification.md | references/capabilities/games-and-collusion.md、references/capabilities/life-positions.md |
| 为什么总遇到同类人/同样的剧情；核对我在玩哪个心理游戏；理解「共谋」 | references/capabilities/games-and-collusion.md | references/capabilities/life-positions.md、references/capabilities/game-identification.md、references/capabilities/learning-resistance.md、references/capabilities/self-criticism.md |
| 想记录情绪变化；说不出自己有多难受；给咨询设一个前后基线 | references/capabilities/emotion-thermometer.md | references/capabilities/connecting-with-emotions.md、references/capabilities/depression-warning-signs.md、references/capabilities/basic-feelings.md |
| 总在压抑/回避情绪；哭是不是软弱；道理都懂但没有变化 | references/capabilities/connecting-with-emotions.md | references/capabilities/basic-feelings.md、references/capabilities/emotion-thermometer.md、references/capabilities/anger-release.md、references/capabilities/self-criticism.md |
| 我是不是太依赖了；评估自己的成长阶段；想自查情商/理解共情；对帮你的人产生强烈情绪（依赖、发火、想推开） | references/capabilities/growth-path-and-eq.md | references/capabilities/counseling-setup.md、references/capabilities/adapted-child-patterns.md、references/capabilities/helping-others.md |
| 朋友状态差，我该怎么帮他；说了很多次他都听不进去；我怕帮倒忙 | references/capabilities/helping-others.md | references/capabilities/learning-conditions.md、references/capabilities/depression-warning-signs.md、references/capabilities/ego-states.md、references/capabilities/growth-path-and-eq.md |
| 考虑去看心理咨询该了解什么；弄清咨询是什么/不是什么；我被人劝去咨询 | references/capabilities/counseling-setup.md | references/capabilities/growth-path-and-eq.md、references/capabilities/depression-warning-signs.md、references/capabilities/learning-resistance.md |
| 想理清自己为什么变成这样；想完整讲一遍我的经历；感觉在重复演同一个剧本 | references/capabilities/life-story-and-script.md | references/capabilities/adapted-child-patterns.md、references/capabilities/life-positions.md、references/capabilities/self-criticism.md、references/capabilities/ego-states.md |
| 我恨我已故的父亲/母亲；该不该原谅他们；对父母的愤怒让我内疚 | references/capabilities/parental-forgiveness.md | references/capabilities/blame-to-responsibility.md、references/capabilities/self-criticism.md |
| 怀疑自己/家人抑郁了；他总说活着没意思；怎么开口问自杀 | references/capabilities/depression-warning-signs.md | references/capabilities/helping-others.md、references/capabilities/emotion-thermometer.md、references/capabilities/self-criticism.md |
| 正在被人动手打／被威胁／被虐待／不敢回家；人身安全正受威胁 | references/capabilities/depression-warning-signs.md | **走安全判停，不套用书中分析流程**：第一产出是确保人身安全与转介（离开危险现场、可信的人陪同、警方与紧急服务、专业机构）；references/capabilities/helping-others.md |

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

- 出现自伤/自杀念头、计划或无法保证安全时：**本 skill 仍然适用**（不要判为不适用），停止自助流程，立即建议寻求精神科/专业帮助与紧急支持（**全国统一心理援助热线 12356**）。
- 用户描述正在发生的虐待、暴力或人身安全威胁时：**本 skill 仍然适用**（不要判为不适用或 none）——**第一产出是确保人身安全与转介**（离开危险现场、联系可信的人陪同、联系警方与紧急服务、专业机构），**不要**先做情绪分析或套用书中框架；先安全、后理解。
- 用户要求用本书框架给他人「诊断/定性」（贴状态、坐标、游戏标签）时：不替对方定性，说明这些是自我反思工具（不是诊断或指控工具），并把方向转回用户自己的观察。
