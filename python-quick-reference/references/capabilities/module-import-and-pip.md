<!-- capability_id: cap.python-quick-reference.module-import-and-pip | revision: 1 | status: active -->
<!-- 来源: 单元 v13；归并候选 8 条（f21/f22/p27/ce12/ce13/g15/g16/g17） -->

# 模块导入与包管理（import / from…import、标准库速查、pip 装与卸）

## R — 原文 (Reading)

> 其中，modulename 为要导入模块的名称；[as alias]为给模块起的别名，通过该别名也可以使用模块。
> ……
> Member：用于指定要导入的变量、函数或者类等。可以同时导入多个定
> 义，各个定义之间使用逗号“，”分隔。如果想导入全部定义，也可以使
> 用通配符星号“＊”代替。
>
> — 第 11 章 · 模块「必背必记 1-2」L1637-1659

> Modulename：可选参数，用于指定要安装或者卸载的模块名，当command
> 为 install 或者 uninstall 时不能省略。
>
> — 第 11 章 · 模块「必背必记 4．第三方模块的下载与安装」L1699-1701

## I — 方法论骨架 (Interpretation)

这张卡回答「我要用一个功能，手头这个环境到底有没有」：先分清**标准库**与**第三方模块**，再决定写 `import` 还是走命令行的 `pip`。混着来会得到两种返工——装了半天发现本来就自带，或者 import 了却发现没装。

顺序单一：先查书中的标准模块表（sys、time、calendar、urllib、json、re、math、decimal、shutil、logging、tkinter）。表里有的属标准库，**不需要 pip、直接 import**；表里没有才是第三方，走 pip。

引入方式两种：`import modulename [as alias]` 导入整个模块并可起别名，之后通过该别名使用；`from modulename import member` 只导入指定的变量、函数或类，逗号分隔可多个，想导入全部定义可用通配符星号 `*` 代替。

pip 是**命令行**工具、不是 Python 语句。形式 `pip <command> [modulename]`，command 常用 install / uninstall / list；modulename 字面是「可选参数」，但 command 为 install 或 uninstall 时不能省略。两条硬约束：模块名**区分字母大小写**、要与定义时一致（否则对应 p26 的 ImportError）；install / uninstall 必带模块名。

## A1 — 应用示例 (Application Example)

### 示例 1: 「取当前时间」要不要装包
- **类型**: 合成演练（按原书规则构造，非作者案例）
- **问题**: 需要一个取当前时间的能力，先判断它属于标准库还是第三方
- **怎么用**: 查「必背必记 3」的标准模块表（候选 p27），命中 time（表中写「提供与时间相关的各种函数的标准库」）；属标准库，因此 `import time` 即可，起别名则 `import time as t`。书中只给模块用途、不给函数名，故停在「引入模块」这一层。
- **结果**: 无需 pip 安装；书中未说明具体的时间函数怎么调用。

### 示例 2: 一条 pip 命令白跑一趟
- **类型**: 合成演练（按原书规则构造，非作者案例）
- **问题**: 只敲 `pip install` 却不写模块名
- **怎么用**: 按 ce13 检查——「当command 为 install 或者 uninstall 时不能省略」；改写为 `pip install <模块名>`。
- **结果**: 书中未给出命令输出，只给出上述限制。

## A2 — 触发场景 (Future Trigger) ★

### 用户会在什么情境下需要这张卡?
1. 要用某个库或功能，不确定它是自带的、还是要先装
2. 代码第一行 import 就报错，想知道是名字写错还是没装
3. 要装或卸一个第三方模块，不确定命令怎么写全

### 语言信号
- 「这个库要装吗 / 是不是自带的」
- 「ImportError」「ModuleNotFoundError」「No module named …」
- 「怎么装包 / 怎么卸载」
- 英文: "do I need to install this", "is it in the standard library", "pip install", "from x import y"

### 与相邻能力的区分（阶段 3 定稿）
- 与 `database-access` 的区别: 那条讲连接对象、游标对象与 SQL 执行顺序；连库前的驱动模块从哪来、是不是标准库、要不要 pip 装属于本卡。按 depends-on：先走本卡把模块引入或装上，再进 `database-access`。
- 与 `socket-communication` 的区别: 那条讲 Socket 的方法序列（底本未给 `import socket` 与创建语句，NR-12）；本卡只管「模块叫什么、怎么引入或装上」，不管里面有哪些方法。

## E — 可执行步骤 (Execution)

1. **判定模块来源**
   - 完成标准: 把要用的功能在标准模块表里逐项对照；命中记「标准库，无需安装」，未命中记「第三方，需 pip」，并写出依据的表中哪一行
   - 判停条件: 表中只有模块用途、没有函数名时不要编函数名，本步只到模块粒度

2. **写导入语句**
   - 完成标准: 给出两种形式之一并说明为什么选它（整体使用要短名 / 只要几个成员 / 全部定义用 `*`）；模块名大小写与定义处一致（ce12）

3. **走 pip 装或卸（仅第三方）**
   - 完成标准: 命令形如 `pip install <模块名>`、`pip uninstall <模块名>`、`pip list`，install 与 uninstall 都带模块名（ce13），并说明 pip 是命令行命令
   - 判停条件: 用户只问「装了没」时用 `pip list`，不要顺手给出卸载命令

4. **自检**
   - 完成标准: 每条语句都能回指「标准模块表 + 两种 import 形式 + pip 三个 command」之内

**输入契约**: 必需 = ①要用的功能是什么；②打算怎么用它（整体 / 只要几个成员）；③在写 .py 还是敲命令行。缺 ③ 要先问；缺 ① 无法查表。

**输出契约**: 三段式——①判定（标准库 / 第三方，附表中依据）；②引入语句（附大小写核对）；③若为第三方，给出完整 pip 命令。完成标准：每条语句都能回指表中模块用途或 f21/f22 的语法形式。

## B — 边界 (Boundary) ★

### 不要在以下情况使用这张卡
- 模块内部的函数怎么调、参数怎么传——本书只给模块用途，具体 API 不在本卡范围
- 需要版本约束、依赖解析或隔离环境（`pip install -r`、venv）——本书未覆盖
- 目录怎么组织成包、包内模块怎么互引——本书未覆盖

### 作者在书中警告的失败模式
- 模块名大小写不一致: ce12 明写「区分字母大小写，需要和定义模块时设置的模块名称的大小写保持一致」；对应 p26 的 ImportError
- install / uninstall 少写模块名（ce13）
- 把 pip 当 Python 语句写（g17：pip 是命令行的包管理器，不是 .py 里的语句）

### 作者盲点与底本缺口
- **本书未覆盖**包（目录）、虚拟环境与 requirements 管理（单元 v13 note；BOOK_OVERVIEW 记为时代局限）
- **本书未覆盖**模块与包的区别（g15：本书把模块当作「被导入的单位」，不区分模块与包）
- **本书未覆盖** pip 的其它 command 与参数（书中只给这三个 command 的含义）
- 底本缺陷（NR-15）: 标准模块表被压成单行交错（p27 的引文即为原样），模块名与用途的对应需按语义核对；引用照抄

### 容易混淆的邻近方法论
- `database-access`: 产物是「连接对象 → 游标 → 执行 SQL」的调用序列；本卡是「模块名 + pip 命令」
- `socket-communication`: 讲 Socket 的方法序列；引入模块由本卡负责（NR-12）

---

## 相关能力（阶段 3 定稿）

- depends-on: `database-access`（连库前要先 pip 装对应驱动模块，并判断它是不是标准库）
- depends-on: `socket-communication`（底本未给 import socket，NR-12）

---

## 审计信息

- **验证通过**: V1 ✓ / V2 ✓ / V3 ✓（阶段 1.5，单元 v13）；晋升去向 router（预算排序第 12 顺位）
- **证据**: 候选 8 条；ck-d11eb52b5ee8 ｜ **蒸馏时间**: 2026-09-17
