<!-- capability_id: cap.automate-boring-stuff.environment-setup | revision: 1 | status: active -->
<!-- 来源: 单元 v18；归并候选 3 条（f63/f64/p73）；3 条候选（规模最小）；p73 模块清单为全书前置条件检查表 -->

# 环境搭建与脚本运行（pip 安装第三方模块、#! 行、批处理与 PATH、-O 选项）

## R — 原文 (Reading)

> pip 工具需要在命令行中运行：向它传入install 命令，跟上想要安装的模块名称。
>
> ……
>
> 安装模块后，可以在交互式环境中运行import ModuleName，测试安装是否成功。如果未显示错误信息，就可以认为该模块已经成功安装。
>
> — 附录A · 安装第三方模块 L12556-12606（A.1 pip 工具 L12572、A.2 安装第三方模块 L12578）

> 所有Python 程序的第一行应该是#!行，它告诉计算机你想让Python 来执行这个程序。
>
> ……
>
> 没有#!行，你也能从IDLE 运行Python 脚本，但从命令行运行它们就需要这一行。
>
> — 附录B · 运行程序 L12608-12660（B.1 第一行 L12620-12628）

## I — 方法论骨架 (Interpretation)

这张卡把「书上的代码」变成「机器上能跑的命令」：装依赖、把脚本命令行化，外加一份前置条件检查表。

安装链路「pip install → 交互式 import 验证」：判据直接——import 未显示错误信息即安装成功；升级 `pip install –U ModuleName`；OS X/Linux 用 pip3（书中口径加 sudo 前缀），Windows 用 pip。易错点是导入名≠安装名：beautifulsoup4 的导入名是 bs4、python-docx 要装 python-docx 而不是 docx（附录 A 模块清单）。

运行链路「#! 行 → 平台方式」：#! 行告诉计算机用 Python 执行——IDLE 运行不需要，命令行运行需要；Windows 建 .bat（`@py.exe ... %*` 转发参数）并把脚本目录加进 PATH，OS X/Linux 用 `chmod +x`。`-O` 放在 python/python3 之后、.py 之前，跳过 assert 提速。

口径：先验证再写码——装好模块先在交互式环境 import 一次，确认无报错。

## A1 — 书中的应用 (Past Application)

原书未提供独立案例（本单元 3 条候选均为流程/清单，无 c*/ce* 证据）。

### 合成演练（明确标记，非书中案例）: 让 `fetch <url>` 在新机器上直接可用
- **类型**: 合成演练（明确标记；按 f63/f64/p73 规则复算，无书中实测输出）
- **问题**: 在新机器上让 browser 抓取脚本可用，并能从命令行以 `fetch <url>` 运行
- **方法论的使用**: ① `pip install requests beautifulsoup4`（OS X/Linux 用 pip3，必要时 sudo）→ 交互式 `import requests` 无报错即成功；② 首行写 #! 行；Windows 建 .bat（`@py.exe ... %*`）并把脚本目录加进 PATH，OS X/Linux `chmod +x`；③ 需要时 `python -O` 跳过断言
- **结论**: 按书判据，「未显示错误信息，就可以认为该模块已经成功安装」；平台界面为 2015/2016 形态，现代路径按当前系统文档执行
- **结果**: 未说明（合成演练）

## A2 — 触发场景 (Future Trigger) ★

### 用户会在什么情境下需要这张卡?

1. `pip install` 之后 import 仍报 No module（装错包名/导入名对不上）
2. 要核对安装名与导入名（bs4 装哪个包、python-docx 是不是 docx）
3. 要让脚本从命令行直接运行（#! 行、chmod、批处理、PATH）
4. 要跳过断言（-O）或查全书模块清单做前置条件检查

### 语言信号

- 「pip install 完了 import 还是 No module」「怎么让脚本双击就能跑」「bs4 装哪个包」
- 英文: "pip install fails", "No module named", "run from command line", "what package installs bs4"

### 与相邻能力的区分（阶段 3 定稿）

- 与 `debugging-toolkit` 的区别: `-O` 禁用断言、运行方式与调试工具在附录 B.4 交汇（composes-with）；本卡管「装与跑」的前置链路，代码逻辑排障走那条卡

## E — 可执行步骤 (Execution)

1. **装模块**
   - 完成标准: `pip install ModuleName`（OS X/Linux 用 pip3、书中口径加 sudo；升级 `pip install –U ModuleName`）；先核对易错项（bs4、python-docx）
2. **import 验证**
   - 完成标准: 交互式 `import ModuleName` 无报错，才认为安装成功
   - 判停条件: 报 No module → 回步骤 1 核对导入名
3. **写 #! 行**
   - 完成标准: 首行按平台写（Windows `#! python3`；OS X `#! /usr/bin/env python3`；Linux `#! /usr/bin/python3`）
4. **命令行化**
   - 完成标准: Windows 建 .bat（`@py.exe C:\path\to\your\pythonScript.py %*`）并把脚本目录加进 PATH；OS X/Linux `chmod +x 脚本.py` 后运行
5. **（可选）-O**
   - 完成标准: `python -O 脚本.py` 跳过 assert（书中口径：稍稍提高性能）

**输入契约**: 必需 = ①模块名（导入名+安装名两张写法）；②目标平台；③脚本路径。缺平台或模块名先问，不静默猜包名。
**输出契约**: 安装命令 + import 验证语句 + 运行命令（.bat / chmod 形态）。完成标准：每条命令可回指 f63 / f64 / p73；平台步骤注明 2015/2016 形态，按当前系统文档核对。

## B — 边界 (Boundary) ★

### 不要在以下情况使用这张卡

- 需要虚拟环境 / requirements / 离线安装 / 现代 `python -m pip`——本书未覆盖
- 需要现代系统界面的精确步骤（环境变量窗口等）——书里是 2015/2016 形态
- 问题由代码逻辑引起——走 `debugging-toolkit`

### 作者在书中警告的失败模式

- 装错包名：书里明写 python-docx「安装python-docx，而不是docx」；bs4 的安装名是 beautifulsoup4
- 跳过验证：作者判据「如果未显示错误信息，就可以认为该模块已经成功安装」——不验证直接写码，失败点被推迟到脚本里
- 少了 #! 行：「没有#!行，你也能从IDLE 运行Python 脚本，但从命令行运行它们就需要这一行」

### 作者盲点与底本缺口

- 本单元规模最小（3 条候选，unit_note）：「安装流程 + 运行方式 + 模块清单」的操作参考合集，非单一可交付流程；平台步骤 2015 形态、跨场景复用弱，判据降级为 router
- 本书未讲虚拟环境 / requirements / 离线安装；现代 pip 亦可用 `python -m pip`（f63 缺失条件）
- 平台路径与界面随系统版本变化，属现代易失效内容（f64）；原书无失败机制叙述（无反面评测面），本卡失败模式取自作者反向表述

### 容易混淆的邻近方法论

- `debugging-toolkit`: `-O` 禁用断言与运行方式在附录 B.4 交汇；本卡管「装与跑」，排障走那条卡

---

## 相关能力（阶段 3 定稿）

- composes-with: `debugging-toolkit`（`-O` 禁用断言、运行方式与调试工具在附录 B.4 交汇）

---

## 审计信息

- **验证通过**: V1 ✓ / V2 ✓ / V3 ✓（阶段 1.5，单元 v18）
- **证据**: 候选 3 条（f63/f64/p73）；ck-96ef4fdcf32d / ck-7753e82928e2 / ck-3a913637f667
- **去向**: 阶段 1.6 router 经来源路由入口可达（判据降级：独立契约不成立、复用/评测面不足）
- **蒸馏时间**: 2026-09-18（分片 B）
