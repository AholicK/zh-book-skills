<!-- capability_id: cap.automate-boring-stuff.debugging-toolkit | revision: 1 | status: active -->
<!-- 来源: 单元 v09；归并候选 23 条（f28/f29/f30/f31/f32/f33/f34/f35/f36/p55/p56/p57/p58/p59/g24/g25/g26/c20/c21/ce18/ce19/ce20/ce21）；23 条候选（第二大单元）；含 ce18–ce21 四个缺陷边界；B 段须写现代调试器替代口径 -->

# 调试工具与缺陷排查（raise/断言/日志/调试器四件套及其选择判据）

## R — 原文 (Reading)

> 程序员之间流传着一个老笑话：“编码占了编程工作量的
> …，调试占了另外90%。”
> …好在，有一些工具和技巧可以确定你的代码在做什么，以及哪儿出了问题。首先，你要查看日志和断言。
> …缺陷发现的越早，就越容易修复。
> …断言、异常、日志和调试器，都是在程序中发现和预防缺陷的有用工具。
>
> — 第10章 · 调试（10.6 小结）

> 断言针对的是程序员的错误，而不是用户的错误。对于那些可以恢复的错误（诸如文件没有找到，或用户输入了无效的数据），请抛出异常，而不是用assert 语句检测它。
> …
> 断言所针对的错误，是程序不应该尝试恢复的，而是应该快速失败。否则，你应该抛出异常。
>
> — 第10章 · 调试（10.3；10.6 小结）

## I — 方法论骨架 (Interpretation)

四件套按**错误性质**选：

1. **可恢复的错误**（文件没找到、用户输入无效）→ 抛异常：`raise Exception('有用的出错信息')`；**函数抛、调用方接**，接住后用 `str(err)` 转成用户消息。
2. **程序员的错误 / 不该恢复的内部假设** → `assert 条件, '说明字符串'`：失败就让程序崩溃；**不应用** try/except 接住；发布时 `python -O` 禁用。
3. **要看中间量与次序** → `logging`：五级 DEBUG/INFO/WARNING/ERROR/CRITICAL，`basicConfig(level=, format=)`、可写文件（filename），调试完一次 `logging.disable(logging.CRITICAL)` 全关；**不要**用 print() 调试。
4. **要看运行中的实际值** → 调试器：断点设在分支内部（别设在循环每次都执行的语句上）；Step/Over/Out/Go 管步入、跨过、离开与全速。

## A1 — 书中的应用 (Past Application)

### 案例 1: 交通灯模拟——用断言保住「至少一盏红灯」
- **类型**: 书中示例（第10章）
- **问题**: 逐键切灯出现「两向都不是红灯」的中间态——程序不崩溃，「虚拟的汽车撞车了」。
- **方法论的使用**: 在 switchLights() 末尾断言 `assert 'red' in stoplight.values(), 'Neither light is red! ' + str(stoplight)`。
- **结论**: 立即崩溃（AssertionError + 该消息）；「尽早快速失败，可以省去将来大量的调试工作」。
- **结果**: 未说明（未给修复代码）。

### 案例 2: 调试器下找「The sum is 5342」
- **类型**: 书中演练（第10章）
- **问题**: buggyAddingProgram.py 把三次 input() 直接相加；输入 5、3、42 输出 `The sum is 5342`——没崩溃但求和错。
- **方法论的使用**: 先用 Over 单步（不进 print()），看全局变量区实际值。
- **结论**: 三个变量是**字符串值**而非整型——字符串连接而非相加。
- **结果**: 未说明。

## A2 — 触发场景 (Future Trigger) ★

### 用户会在什么情境下需要这张卡?

1. 脚本报错、反向跟踪一大串，不知看哪一行
2. 程序没崩溃但结果不对，要找实际值与预期不符处
3. 要在长循环里看中间量/次序，或拿不准该写 `assert` 还是 `raise`/`try-except`

### 语言信号

- 「脚本又报错了」「循环里想打印中间值看看」「程序不崩溃但结果不对」「assert 和 try/except 用哪个」
- 英文: "how to debug this", "logging vs print", "assert vs try/except"

### 与相邻能力的区分（阶段 3 定稿）

- 与 `functions-and-exceptions` 的区别（composes-with）: 那条讲异常机制；本卡讲出错时**选哪件**。
- 与 `gui-automation` 的区别（composes-with）: 「尽早失败」同源，对象是 GUI。
- 与 `smart-questions` 的区别（composes-with）: 卡住后带反向跟踪与最小复现按那条的七要素求助。
- 与 `environment-setup` 的区别（composes-with）: `-O` 与运行方式配置在附录 B.4 交汇。

## E — 可执行步骤 (Execution)

1. **收集症状**——完成标准: ①有无报错（读完整反向跟踪：消息+行号+调用栈）②是否崩溃③期望值 vs 实际值。判停: 报错已指出位置时先修。
2. **判定错误性质**——完成标准: 归入①可恢复的错误②程序员的错误/不可能状态③需观察中间量与次序之一。
3. **产出工具与写法**——完成标准: ①→`raise Exception('有用的出错信息')` + 调用方 try/except（`str(err)`）；②→`assert 条件, '说明字符串'`（不接 try/except）；③→`logging.basicConfig(level=logging.DEBUG, format=' %(asctime)s - %(levelname)s - %(message)s')`；看实际值→断点/单步。判停: 不得退回 print() 调试。
4. **验证与收尾**——完成标准: 断言失败确实崩溃且消息可读；日志能指出哪一步没执行/值不对；调试完加 `logging.disable(logging.CRITICAL)`；发布禁断言说明 `-O`。

**输入契约**: 必需＝①程序或最小复现 ②症状 ③错误属「可恢复」还是「不该发生的内部错误」（缺③先问）。
**输出契约**: 交付＝①工具选择及依据 ②可复制写法（raise/assert/日志模板）③收尾开关；完成标准：能回指 f28–f32 / p55–p59。

## B — 边界 (Boundary) ★

### 不要在以下情况使用这张卡

- 报错已直接指出错误（如语法错误）——按信息修复即可。
- 要的是面向用户的输出——用户该看的消息本就该用 print()，塞进日志会在 disable 后消失。
- 某一款 IDE 的调试器操作细则——书中载体是 IDLE（f34/f35/f36/c21）。

### 作者在书中警告的失败模式

- ce18 缺陷不报错: 交通灯「两个方向同时放行」，程序不崩溃，「虚拟的汽车撞车了」。
- ce19 断言误用与 -O: 不应用 try/except 处理 assert；断言只针对程序员的错误；`-O` 时断言整体禁用。
- ce20 不要用 print() 调试: print 要逐条手工清除；日志一次 disable 整体关闭。
- ce21 静默缺陷: factorial() 恒返 0；`guesss` 拼写 + int/str 比较恒 False——靠日志/断点看实际值。

### 作者盲点与底本缺口

- **IDLE 依附（unit_note）**: 调试器（f34/f35/f36/c21）与具体 IDE 强耦合、属现代易失效内容——替代品（VS Code/pdb）沿用同一判据。
- **OCR 错位（NR）**: 三张表 OCR 串行错位（表10-1 / 7.10 / 表12-3）；引用以「正文逐句陈述 + 单点示例」为准，不引用表格全貌。
- **书中未覆盖**: 自定义异常继承体系（f29）；日志轮转（f32）；性能开销（f33）；c21 未给修复代码。

### 容易混淆的邻近方法论

- `functions-and-exceptions`: 异常机制；`gui-automation`: 「点错就停」同源；`smart-questions`: 求助七要素；`environment-setup`: `-O` 与运行方式。

---

## 相关能力（阶段 3 定稿）

- composes-with: `functions-and-exceptions`（共用同一套异常机制）；`gui-automation`（「尽早失败」同源）
- composes-with: `smart-questions`（自查→求助接力）；`environment-setup`（`-O` 与运行方式交汇，附录 B.4）

---

## 审计信息

- **验证通过**: V1 ✓ / V2 ✓ / V3 ✓（阶段 1.5，单元 v09）
- **证据**: 候选 23 条（f28/f29/f30/f31/f32/f33/f34/f35/f36/p55/p56/p57/p58/p59/g24/g25/g26/c20/c21/ce18/ce19/ce20/ce21）；ck-c659d5e5c9ce / ck-4a66e94074d2 / ck-9fc7fb01e533
- **去向**: 阶段 1.6 router 经来源路由入口可达
- **蒸馏时间**: 2026-09-18（分片 B）
