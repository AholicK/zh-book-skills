<!-- capability_id: cap.python-quick-reference.process-and-thread | revision: 2 | status: active -->
<!-- 来源: 单元 v17；归并候选 5 条（f31, f32, ce09, c12, g23） -->

# 进程与线程（Process / Thread 同构参数、Lock 加解锁）

## R — 原文 (Reading)

> Process类的参数说明如下：
group：参数未使用，值始终为None。
表示当前进程启动时执行的可调用对象。
>
> — 第 18 章 · 使用进程和线程「必背必记 1-2」L2483-2523

> acquire([blockingl)：获取锁定，如果有必要，需要阻塞到锁定释放
为止。如果提供blocking参数并将它设置为False，当无法获取锁定时将
立即返回False，如果成功获取锁定则返回True。
releaseO：释放一个锁定。当锁定处于未锁定状态时，或者从与原本
>
> — 第 18 章 · 使用进程和线程「必背必记 3．使用互斥锁」L2525-2551

## I — 方法论骨架 (Interpretation)

Python 把「同时做两件事」交给两套几乎同形的类：`multiprocessing.Process` 与 `threading.Thread`，参数表一模一样——**记住一套就能读懂两处调用**。

- **参数层（怎么建）**：`group` 值为 `None`（未使用）；`target` 是启动时执行的可调用对象（所以要先有函数）；`args` 是参数元组、`kwargs` 是参数字典；`name` 是实例别名。填参数只负责"把活儿挂上去"，不负责让它跑起来。
- **互斥层（怎么不打架）**：共享数据用 `threading.Lock()` 造一把锁，临界区围成 `acquire` → 改 → `release` 三段，判据是**成对且同一线程**。
- **等待语义**：`acquire` 默认阻塞到锁被释放；只有传 `blocking=False` 才是"试一下"——拿不到立即得 `False`，拿到 `True`。

本卡交付的边界是**创建对象 + 正确用锁**：本书没有 `start()`/`join()`，因此不写"怎么真的跑起来"。

## A1 — 应用示例 (Application Example)

### 示例 1: 互斥锁的三行骨架（创建 → 加锁 → 释放）
- **类型**: 原书示例（残片）
- **问题**: 一段"读改写"的代码不许别的执行体插进来，锁怎么写
- **怎么用**: 书中骨架为 `mutex = threading.Lock()`（创建锁）→ `mutex.acquire([blocking])`（锁定）→ `mutex.release()`（释放锁）；OCR 中注释与语句错行、`acquireO`/`releaseO` 括号误识成字母 O，照抄不改字
- **结果**: 书中未说明（原书只给骨架，无运行输出）

### 示例 2: 两个线程 + 一把锁保护共享计数
- **类型**: 合成演练（按原书规则构造，非作者案例）
- **问题**: 两个线程各打印一句话，还要给同一个计数器加一
- **怎么用**: 用 `Thread` 的同构参数形式给出 `target`（打印并加计数的函数）与 `args`；用 `threading.Lock()` 创建**一把**锁；计数自增先 `acquire`、改完立刻 `release`（成对同线程）；若希望拿不到就先去干别的，写成非阻塞形式（按书中约定：拿不到立即得 `False`）
- **结果**: 书中未说明（纸面推演）。演练**未写** `start()`/`join()`——书中未给

## A2 — 触发场景 (Future Trigger) ★

### 用户会在什么情境下需要这张卡?
1. 要同时跑两件事，纠结用 `Process` 还是 `Thread`、参数怎么写
2. 抄来一段 `Thread(...)` 调用，想逐个确认 `target`/`args`/`kwargs` 对应什么
3. 代码里有 `mutex.acquire()` 与 `mutex.release()`，看不出为什么要成对写
4. 看到 `acquire(blocking=False)`，不知道返回什么、该不该等
5. 报错出在释放锁那一行，想确认是不是"没拿到就释放"/"跨线程释放"

### 语言信号
- 「Python 怎么起一个线程／进程」「Process 和 Thread 的参数是一样的吗」
- 「threading.Lock 怎么用」「acquire 返回什么」「锁 release 报错了」
- 英文: "threading.Lock usage", "Process vs Thread arguments", "acquire(blocking=False) return value", "release unlocked lock error"

### 与相邻能力的区分（阶段 3 定稿）
- 与 `function-definition-and-call` 的区别: 本卡只管"把函数挂到线程/进程上"（`target`/`args`/`kwargs` 怎么填）；那个函数本身怎么定义、怎么传参与返回，走那条能力。两者相接处只有一个动作：先有可调用对象，才能填 `target`

## E — 可执行步骤 (Execution)

**输入契约**：①要并发执行的"活儿"是什么 ②有没有被两个执行体同时读写的名字 ③是否需要"拿不到锁就继续走"。缺①②时先问一句"这段代码里哪个变量会被两边同时改"，不要替用户假设没有共享数据。

1. **写清目标与参数** — 完成标准: 一句话说明 `target` 做什么、`args`/`kwargs` 有哪些项，并指出其中哪些值可变
2. **填写同构参数** — 完成标准: 给出 `Process(...)`/`Thread(...)` 调用，五个参数逐一可对应（`group` 默认、`target` 可调用对象、`args` 元组、`kwargs` 字典、`name` 可选）
3. **判定是否要锁** — 完成标准: 列出全部共享名字及"单边写／两边写"；判停条件: 无共享写入 → 跳到第 6 步
4. **用 Lock 围住临界区** — 完成标准: 锁只创建一次且位置早于使用处，临界区写成 `acquire` → 改 → `release`，并注明"同一执行体内配对"
5. **按需使用非阻塞获取** — 完成标准: 写了非阻塞形式，必须补上拿不到时的分支（得 `False` 时等待还是放弃）
6. **声明边界** — 完成标准: 交付物中不出现 `start()`/`join()` 等书中未给的内容；用户要求"跑起来"时说明本书未覆盖（NR-11）

**输出契约**：交付物 = ①目标与参数清单 ②参数填全的构造调用 ③共享数据清单与加解锁序列（无共享写"无"）④边界声明。完成标准：每个共享写入的变量都能找到对应的 `acquire`/`release` 配对，或标注"无锁保护"。

## B — 边界 (Boundary) ★

### 不要在以下情况使用这张卡
- 要"真的把线程/进程跑起来"（启动、等待、回收）：本书没有 `start()`/`join()`（NR-11），**本书未覆盖**
- 问 GIL、进程间通信（`Queue`/`Pipe`）、死锁、可重入锁、条件变量：书中未讲
- 单线程里写循环 → `control-flow-and-loops`；定义被挂上去的那个函数 → `function-definition-and-call`

### 作者在书中警告的失败模式
- release 的两种非法情形（原文照抄）：锁处于未锁定状态时释放，或从与原本调用 acquire 方法的不同线程调用该方法——「将出现错误」。判据落成一句：**成对 + 同线程**；书中只写"将出现错误"，未给异常类型
- 阻塞语义误用：`acquire` 不传参数是"需要时就阻塞到锁释放为止"；只有传 `blocking=False` 才立即返回 `False`/`True`，混用会得到"以为在等其实没等"

### 作者盲点与底本缺口
- `start()`/`join()` 全单元未出现（NR-11）、GIL 与进程间通信（`Queue`/`Pipe` 仅出现在英语词汇表）——**本书未覆盖**
- OCR 现象（NR-15）：`group`/`target` 的说明被折行切开、`target` 的标签行落在说明之后；`acquireO`/`releaseO` 括号误识，照抄未规范化
- 证据 5 条候选（f31/f32/ce09/c12/g23），其中只有 c12 是示例且为残片；全书无作者亲历案例

### 容易混淆的邻近方法论
- `function-definition-and-call`: 本卡的 `target` 指向它产出的函数；先定义、再挂载

---

## 相关能力（阶段 3 定稿）

- depends-on: `function-definition-and-call`（Process/Thread 的 target 必须是一个函数或可调用对象）

---

## 审计信息

- **验证通过**: V1 ✓ / V2 ✓ / V3 ✓（阶段 1.5，单元 v17）
- **证据**: 候选 5 条；ck-0898d629b0ad
- **蒸馏时间**: 2026-09-17
