<!-- capability_id: cap.python-quick-reference.exception-handling | revision: 1 | status: active -->
<!-- 来源: 单元 v14；归并候选 7 条（f23/f24/f25/p26/ce16/g18/g19） -->

# 异常处理与调试（try…except…finally / raise / assert / 常见异常对照）

## R — 原文 (Reading)

> 完整的异常处理语句应该包含finally代码块，其语法格式如下：
>
> — 第 12 章 · 异常处理及程序调试「必背必记 3」L1775-1791

> 如果某个函数或方法可能会产生异常，但不想在当前函数或方法中处理这个异常，则可以使用raise语句在函数或方法中抛出异常。Raise语句的语法格式如下:
>
> — 第 12 章 · 异常处理及程序调试「必背必记 4」L1793-1799

## I — 方法论骨架 (Interpretation)

这张卡把「程序出错时会发生什么」从二值事件（崩 / 没崩）变成三段可控行为。判据不是「这里可能出错」，而是**出错之后我要做什么**：能就地收拾的用 try…except，这层收拾不了的用 raise 交给调用方，要验证「自己以为必然成立」的前提用 assert。

三段分工：try 放可能出现错误的代码块；except 捕获指定异常并处理（异常名可选、可起别名以便记录异常内容）；finally 放任何情形都必须执行的代码。书里的判据是硬话一句——完整语句应包含 finally，只写 try…except 不算写完。

raise 的理由是「本层不处理」：函数或方法可能会产生异常但不想在此处理时用 raise 抛出、交给调用方；参数 ExceptionName[(reason)] 可选，**省略时把当前的错误原样抛出**。

assert 是调试工具、不是控制流：为真什么都不做、为假抛出 AssertionError，reason 用于描述判断条件。它暴露的是「假设不成立」，不是「用户输入错了」。

排障入口是常见异常对照表：NameError 未声明变量、IndexError 索引超范围、IndentationError 缩进错误、ValueError 传入的值错误、KeyError 不存在的字典关键字、IOError 输入输出错误（如文件不存在；OCR 作 OError）、ImportError 找不到模块、AttributeError 未知属性、TypeError 类型不合适、MemoryError 内存不足、ZeroDivisionError 除数为 0。

## A1 — 应用示例 (Application Example)

### 示例 1: 读一个不存在的文件
- **类型**: 合成演练（按原书规则构造，非作者案例）
- **问题**: 读取文件这一步可能失败，既要给友好提示，又要保证收尾动作一定执行
- **怎么用**: try 里放读取动作；except 捕获对照表中与「输入输出错误（如要读取的文件不存在）」对应的 IOError（OCR 作 OError）；finally 里放必须执行的收尾代码——三段齐备才符合 f23 的判据（assert 的用法见 E 段第 4 步）。
- **结果**: 书中未说明运行结果；结构与各块职责取自 f23，异常名对应取自 p26。

## A2 — 触发场景 (Future Trigger) ★

### 用户会在什么情境下需要这张卡?
1. 程序一跑就抛异常，看到一长串信息不知从哪看起
2. 想让程序在出错时给友好提示而不是崩掉
3. 某个动作「无论如何都要做」（关闭、清理），不知放哪一段
4. 函数里发现错误但这一层不适合处理，纠结该处理还是上抛

### 语言信号
- 「报错了 / 直接崩了 / 怎么让它别崩」
- 「try except finally 怎么写」「raise / assert 有什么用」
- 「这个异常是什么意思」「NameError」「KeyError」
- 英文: "wrap in try/except", "traceback", "assertion error", "catch exceptions"

### 与相邻能力的区分（阶段 3 定稿）
- 与 `function-definition-and-call` 的区别: 那条讲 def / return / lambda 与参数默认值；本卡只管函数**出错时**的动作（raise 上抛、assert 暴露假设，composes-with）
- 与 `file-and-directory-operations` 的区别: 那条给文件动作的参数口径；文件不存在、输入输出错误**怎么办**是本卡的事（composes-with：先写对动作，再用本卡包起来）。
- 与 `control-flow-and-loops` 的区别: if/elif 判可预期条件，try/except 捕运行期不可预期的错误（contrasts-with：不要用 try 替代 if）。

## E — 可执行步骤 (Execution)

1. **分清「可预期分支」还是「运行期错误」**
   - 完成标准: 说出被处理的情况属于「条件本来就该判」还是「某个动作可能出错」，只有后者留在本卡
   - 判停条件: 若是正常业务分支，转 `control-flow-and-loops`（contrasts-with）

2. **划出三段**
   - 完成标准: 写出 try（可能出现错误的代码块）、except（异常名 + 处理动作）、finally（任何情形都必须执行的代码）三块，finally 不为空（f23 判据）；每个异常名都能在对照表中按语义对上
   - 判停条件: 找不到语义对应的异常名时先说明「本书对照表没有这一项」，不要用表外的名字

3. **决定就地处理还是上抛**
   - 完成标准: 能就地处理的写在 except 内；不能处理的用 `raise [ExceptionName[(reason)]]` 抛出，并说明省略参数时是「把当前的错误原样抛出」（f24）

4. **用 assert 钉住假设、并自检**
   - 完成标准: 写成 `assert expression [,reason]`（expression 是「程序自己以为必然成立」的条件、reason 写明判断条件，且不替代 if——ce16）；交付物没有多分支 except 顺序、异常层次、自定义异常、else 子句等本书未覆盖的写法

**输入契约**: 必需 = ①会出错的操作用一句自然语言描述；②要捕获哪一种异常（答「不知道」则按操作语义给候选）；③是否有「无论如何都要做」的收尾动作。缺 ② 不得静默挑异常名；缺 ③ 时 finally 可省，但结论要写明本卡口径。

**输出契约**: 代码骨架 + 说明（各异常名的依据、finally 里放什么、是否需要 raise 或 assert）。完成标准：每个异常名都能在对照表中回指一行，且不含本书未覆盖的语法元素。

## B — 边界 (Boundary) ★

### 不要在以下情况使用这张卡
- 用户要的是正常业务分支判断（if / elif）——那属于 `control-flow-and-loops`
- 要设计异常类型体系、自定义异常类或异常链——本书未覆盖
- 只想让报错「消失」而把大段代码包进 try——这会掩盖真实的出错位置

### 作者在书中警告的失败模式
- 把 assert 当条件分支（ce16）：「为真时，什么都不做，如果为假时，则抛出AssertionError异常」——它暴露的是假设不成立，不是「用户输入错了」
- 该上抛却就地吞掉: f24 明写 raise 的场景是「可能会产生异常，但不想在当前函数或方法中处理这个异常」
- 收尾动作不写进 finally: f23 的判据是「完整的异常处理语句应该包含finally代码块」

### 作者盲点与底本缺口
- **本书未覆盖** else 子句、异常层次结构与自定义异常（单元 v14 note）
- **本书未覆盖**断言在优化模式（-O）下失效（NR-08）
- **本书未覆盖** except 不写异常名时的行为（g18 注明属外推、本项目不采信）
- 底本缺陷（p26）: 异常名与描述被压成同行交错（引文里的「描述 NameError」「传入的值错误 KeyError」即为原样），对应关系需按语义核对；引用照抄（NR-15）

### 容易混淆的邻近方法论
- `file-and-directory-operations`: 给 open / write / close 的参数口径，本卡给「出错时怎么办」

---

## 相关能力（阶段 3 定稿）

- composes-with: `function-definition-and-call`（函数内部用 raise 与 assert）
- composes-with: `file-and-directory-operations`（读写文件是异常处理的典型场景）
- contrasts-with: `control-flow-and-loops`（if 判可预期条件，try 捕不可预期的错误）

---

## 审计信息

- **验证通过**: V1 ✓ / V2 ✓ / V3 ✓（阶段 1.5，单元 v14）；晋升去向 promoted（五条判据全过）
- **证据**: 候选 7 条；ck-e9b24e941ce4 ｜ **蒸馏时间**: 2026-09-17
