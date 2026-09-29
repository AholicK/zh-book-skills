<!-- capability_id: cap.python-crash-course.git-version-control | revision: 1 | status: active -->
<!-- 来源: books/work/python-crash-course/.cangjie 能力单元 v24 附录D 使用 Git 进行版本控制（13 条候选） -->

# Git 版本控制（附录D）

## R — 原文 (Reading)

> 这是你每次提交项目的可行状态时都希望看到的消息。如果显示的消息不是这样的，请仔细阅读，很可能你在提交前忘记了添加文件。
>
> — 附录D D.7 执行提交（L31226）

## I — 方法论骨架 (Interpretation)

版本控制解决「改坏了怎么办」与「历史可查吗」两个问题，最小可用循环只有三个动作：**add → commit → status**。每次提交都是项目在特定时间点的**快照**，验收判据是状态输出出现「**工作目录干净**」（nothing to commit, working directory clean）——不干净就仔细读提示，十有八九是忘了把文件加入暂存区。

两条概念先立住：**仓库**是「程序中被 Git 主动跟踪的一组文件」，管理数据藏在隐藏目录 .git/ 里，千万不要删除（删掉即丢弃全部历史）；**分支**是「项目的一个版本」，书中的命名分支是 master，git status 会告诉你当前在哪个分支上。

三个日常补充：**忽略文件**——.gitignore 写 __pycache__/（旧版本用 *.pyc），不让自动生成物进仓库；**提交消息**——-m 记录一条消息，-am 只覆盖「已跟踪文件的修改」，两次提交之间新建的文件必须显式 git add .；**查看历史**——git log 与 git log --pretty=oneline。

**回退是分级手段，代价从小到大**：只弃未提交修改用 git checkout .（粒度是整个项目，不只是刚才那一行）；只审核旧版本用 git checkout <引用 ID 前 6 位>（进入分离头指针，看完用 git checkout master 回来，此状态下不要改项目）；彻底放弃较近提交用 git reset --hard <前 6 位>（不可撤销，仅限单人项目）。历史彻底混乱时，先求助，最后才是删掉 .git 重建。

一句话判据：**提交=可行状态快照**——宁可多次小提交，也别把多天的工作攒在未提交状态里赌运气。

## A1 — 书中的应用 (Past Application)

### 案例 1: git_practice 实操——从空仓库到两条提交
- **类型**: 原书例题（分步实操）（附录D D.1–D.9）
- **问题**: 新项目开始迭代前，要建立「改坏能回退、历史可查」的版本控制
- **方法论的使用**: git config 配置身份 → .gitignore 写 __pycache__/ → git init → git status 看未跟踪清单 → git add . → git commit -m "Started project." → 再 status（显示 working directory clean）→ git log 查看历史；随后加一行修改，用 git commit -am "Extended greeting." 再次提交
- **结论**: 提交是项目在特定时间点的快照；「工作目录干净」是每次提交可行状态时希望看到的消息
- **结果**: 书中给出的结果：依次得到空仓库、未跟踪/已暂存文件列表与两条提交历史（c03d2a3 与 08d4d5e）（L31096-L31324）

### 案例 2: 改坏之后——checkout、检出旧提交与 reset --hard
- **类型**: 原书例题（撤销操作示范）（附录D D.10–D.12）
- **问题**: 故意加入会破坏项目的代码后，把工作区恢复到「项目能够正常地运行」的最后一次提交；并演示旧版本审核与永久回退
- **方法论的使用**: git checkout . 放弃全部未提交修改 → status 回到 clean；git checkout be017b 审核旧提交（输出提示进入分离头指针）→ git checkout master 回来；确认只需自己开发后，用 git reset --hard be017b 永久恢复到该提交
- **结论**: 回退让「任意数量的修改」变得可承受——不可行就撤销，无需记住改了什么；分离头指针状态下不宜改动，reset --hard 只适合单人项目
- **结果**: 书中给出的结果：checkout 后坏代码消失、状态 clean；检出旧提交进入 detached HEAD；reset --hard 后 log 只剩 Started project.（L31390、L31472、L31500-L31510）

## A2 — 触发场景 (Future Trigger) ★

### 用户会在什么情境下需要这个 skill?

1. 新写完的小工具（还带一个刚新建的配置文件）要开始迭代，希望改坏能安全回退、历史可查
2. 提交过了，但部署或复制时发现某个新文件压根没进仓库
3. 想回到某个旧版本，但不知道 checkout / reset / 删库重建该用哪一级
4. 看到 detached HEAD、working directory clean 这类提示，拿不准意味着什么

### 语言信号

- 「怎么撤销刚才的修改」「怎么回到之前的版本」
- 「明明提交了，怎么少了文件」
- 「reset --hard 是不是万能的」「detached HEAD 是什么」
- 英文: "how do I undo my changes", "git reset --hard", "working directory clean", "not a git repository"

### 与相邻能力的区分

- 与 `django-styling-and-deploy` 的区别: 那张卡是部署链路（含第 20 章用 Git 跟踪项目文件的口径）；本卡只负责本地「建仓→提交→分级回退」循环
- 与 `debugging-and-help-seeking` 的区别: 历史被搞乱且无法恢复时，先从那张卡的求助流程走一遍，再决定是否删掉 .git 重建

## E — 可执行步骤 (Execution)

1. **初始化四步**
   - 完成标准: git config --global 的 user.name 与 user.email 已设置（用户名必填、邮箱可虚构）；.gitignore 写入 __pycache__/（旧版本写 *.pyc）；git init 后 git status 能看到 On branch master 与未跟踪文件清单
   - 输入契约: 一个项目目录（含要跟踪的文件）
2. **首次提交与验收**
   - 完成标准: git add . → git commit -m "…" → git status 出现「nothing to commit, working directory clean」；不干净时按提示排查（多半漏了 add）
   - 输出契约: git log 中可见一条带引用 ID、作者、日期与消息的提交
3. **日常提交循环**
   - 完成标准: 每达到一个「可行状态」立即提交；用 git commit -am "…" 时检查两次提交之间是否新建过文件——有新文件必须显式 git add . 再提交（ce195）
   - 判停条件: status 未到 clean 不算完成
4. **分级回退（按代价从小到大）**
   - 完成标准: 每次回退前先写明适用级别与前提——①只弃未提交修改：git checkout .（会撤销自上次提交后全部文件的修改）；②只审核旧版本：git checkout <引用 ID 前 6 位>，看完用 git checkout master 回来，分离头指针状态下不做修改；③永久回退：确认单人项目后 git reset --hard <前 6 位>（不可撤销）
   - 输出契约: 一份「回退级别 → 命令 → 前提 → 代价」对照记录
5. **历史混乱时的兜底**
   - 完成标准: 先按附录C 方法求助；仍无法恢复且确认单人项目时，确认文件当前状态后删除 .git 目录（rm -rf .git；Windows 用 rmdir /s .git），用 git init + git add . + git commit 重建；执行前明确「只删历史与提交，不影响文件当前状态」（ce196）
   - 判停条件: 多人项目一律停用本条

## B — 边界 (Boundary) ★

### 不要在以下情况使用

- 多人协作仓库中不要使用本卡的重置类操作（reset --hard、删 .git）——书中明示这些只适合「参与项目开发的人只有你自己」，协作者的历史会被搅乱
- 托管平台、远程仓库与部署流程不在本卡范围——本地循环之外的环节另行处理
- 不要把它当备份工具用：每次无关改动都提交会让历史难读；本卡的口径是「提交=可行状态快照」

### 作者在书中警告的失败模式

- ce195：-am 提交不含新建文件——两次提交之间创建的文件没有被跟踪，「以为提交了」直到部署时才发现缺文件；新文件必须显式 git add .
- ce196：.git 目录不可删除——历史全部存放在里面，删除后将无法检出项目的其他任何状态；它是「无法恢复时的最后手段」且仅限单人
- ce198：reset --hard 是不可撤销的丢弃——较近提交被永久抹掉、没有撤销路径，执行前应确认单人并记录提交号
- ce199：checkout . 的粒度是整个项目——自最后一次提交后所有文件的所有修改（含尚未提交的有价值工作）一并被撤销，操作前先确认将影响哪些文件

### 作者的盲点 / 时代局限

- **2016 年快照，按现环境替换**：默认分支名 master、全部命令输出与提示语都是当年 Git 的口径——现行 Git 的默认分支名与提示可能不同（原书未讨论，差异须自行核对）
- 原书把「删除目录 .git」作为恢复失败后的出路，但未展开「先备份工作区再做不可逆操作」的步骤；不可逆这一点只能靠操作者自觉
- 全书未讲分支协作、远程仓库与合并冲突——附录D 的口径是单人本地仓库
- 转档说明：命令行提示符与输出被压平、句子被页内孤立数字行切断（如 L31372-L31388）；L31632 出现与本书体系不符的异常页眉，属转档错误行、不可当正文；封底乱码区 L31654-L31694 不可引用

### 容易混淆的邻近方法论

- `django-styling-and-deploy`: 部署衔接。第 20 章用 Git 跟踪项目文件的判据在那条链路；本卡是其本地基础循环
- `debugging-and-help-seeking`: 历史被搞乱无法恢复时先求助、后删库——两卡在此衔接

---

## 相关 skills（阶段 3 定稿）

- composes-with: `django-styling-and-deploy`（部署衔接的提交流程）、`debugging-and-help-seeking`（删除 .git 等高风险操作前先求助）

---

## 审计信息

- **验证通过**: V1 ✓ / V2 ✓ / V3 ✓（见 `verified.md` 的 v24 单元）
- **蒸馏时间**: 2026-09-18
