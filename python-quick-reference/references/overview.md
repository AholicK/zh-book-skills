# 《Python背记手册》整书导读

> 本文件是能力包随产物分发的导读（编译后位于本包 `references` 目录下的 `overview.md`），由阶段 0 的 `BOOK_OVERVIEW.md` 精简而来，
> 并承接阶段 3 的「参考区」落地职责（`references.md` 的 12 条 `reference` + 4 项背景材料）。
> 底本：`/root/books/markdown/Python背记手册.md`（OCR 整理稿，2,840 行 / 32,643 字符 / CRLF）。

## 基本信息

- **标题**：Python背记手册
- **作者**：源文未署名（OCR 稿无版权页、无前言、无作者信息；全文「作者」「出版社」「ISBN」「前言」「版权」出现次数均为 0）
- **出版/发布时间**：源文未标注。正文未出现 Python 版本号（「Python 3」出现 0 次，仅正则标志行出现「仅适用于Python3.x」）；内容形态（`Print(输出内容)` 的大小写混排、以 `%` 格式化为重点、tkinter 归入标准库、同时出现 Pygame / Flask 章节）符合面向 Python 3 初学者的教材配套背记册
- **内容类型**：技术速查手册 / 教材配套背记册（非方法论书、非叙事书）
- **结构**：目录列 21 章（L1-41）。**正文标题形式不统一**：`# 第 1 章 走进 Python`（L43）、`# 列表与元组`（L739，缺章号）、`## 运算符与表达式`（L383，用二级标题）、`## 第 20 章`（L2663，章号在后）

## 一句话主旨

把 Python 入门要背的东西按 21 个主题做成「语法形式 + 参数逐项说明 + （部分）示例结果」的速查表，
让初学者能反复查阅、按星级背记；每章另附英语词汇栏，解决读不懂英文关键字与报错名的次生问题。

## 骨架（七组主题）

1. **语法骨架（第 1–4 章）**：if / if…else / if…elif…else / 嵌套、while、for、break、continue 的写法与判序
2. **数据结构（第 5–6 章）**：列表、元组的切片与增删统计；字典与集合的创建、遍历、增删
3. **文本处理（第 7–8 章）**：字符串 encode/decode、split/join、`%` 与 `format() `格式化；正则的元字符、限定符、标志
4. **代码组织（第 9–11 章）**：函数（def / 默认值 / return / lambda）、类与面向对象（class / 实例方法 / @property / 继承 / 重写）、模块与 pip
5. **运行保障（第 12–13 章）**：异常类型对照、try…except…finally、raise、assert；文件与目录操作（open/close/write、os.remove/rename、stat 属性）
6. **数据与界面（第 14–16 章）**：数据库（连接对象 / 游标对象 + SQLite 增删改查 + SQL 语句）、GUI（wxPython 工具包、控件构造器、sizer）、Pygame（模块表、display 与 Surface 方法）
7. **网络与并发（第 17–21 章）**：爬虫框架简介、进程与线程（Process/Thread/Lock）、Socket 方法表、Web 概念（HTTP/HTML/CSS/JS/CGI/WSGI）、Flask（HTTP 方法、过滤器、WTForms 字段与验证器）

前六章为**递进**（语法 → 数据结构 → 文本 → 组织 → 运行保障），第 14–21 章为**并列的应用专题**。
全书各章之间几乎没有交叉引用，属于「并列的查表条目集合」。

## 核心命题（摘自阶段 0 的整书理解，逐条有原文支持）

1. 每章知识可压成「语法形式 + 参数说明 + 实例」三件套。
2. 条件分支有三种形态，判序不同；`else` 不能单独出现，必须与 `if` 成对（L67）。
3. 循环由「条件为真就执行、执行完回判」（while）或「从对象里逐个取值」（for）驱动（L113-117）。
4. break 跳出循环、continue 跳过本轮（L127-143）。
5. 列表 / 元组 / 字符串共用同一种切片语法与同一套三参数默认值（L745-757 / L1139-1153）。
6. 排序有两条路：`sort()` 改原列表、`sorted()` 不改原列表（L879 / L893）——全书少数明确给出「行为差异」的地方。
7. 推导式把「生成 / 映射 / 筛选」三件事写成一行（L907 / L917 / L927）。
8. 字典的键必须唯一且不可变，值不限（L1023-1029）；集合的元素必须是不可变类型（L1067-1069）。
9. 字符串编解码默认 UTF-8、errors 默认 strict（L1115-1127）。
10. 异常处理三件套：try 放可能出错的代码、except 指定要捕获的异常、finally 放任何情形都必须执行的代码（L1765 / L1791）。
11. 数据库操作固定为三层：connect → 连接对象 → 游标对象（L1999-2053）。
12. 并发用 multiprocessing.Process 与 threading.Thread 两套同构 API，共享数据用 Lock 保护（L2487-2551）。

**作者要解决的核心问题**：初学者「看得懂但写不出来」与「报错信息不认识」两个具体障碍。

## 能力地图（20 张能力卡）

| 能力 | 去向 | 一句话 |
|---|---|---|
| [control-flow-and-loops](capabilities/control-flow-and-loops.md) | 独立 Skill | 条件已知用 while、对象已知用 for；break 中断、continue 跳过本轮 |
| [sequence-slicing](capabilities/sequence-slicing.md) | 独立 Skill | `sname[start : end : step]`，含起点不含终点，切片生成新序列 |
| [list-and-tuple-operations](capabilities/list-and-tuple-operations.md) | 独立 Skill | 取元素 → 统计 → 遍历 → 追加（整表追加用 extend，不是 append） |
| [dict-and-set-operations](capabilities/dict-and-set-operations.md) | 独立 Skill | 键唯一且不可变；集合元素不可变；items() 遍历拿键值对 |
| [function-definition-and-call](capabilities/function-definition-and-call.md) | 独立 Skill | 定义—调用—返回三段式；默认值参数写在参数列表里 |
| [class-and-objects](capabilities/class-and-objects.md) | 独立 Skill | 定义 → 实例化 → 方法调用；self 必在首位、类体不能空 |
| [exception-handling](capabilities/exception-handling.md) | 独立 Skill | try / except / finally 三段；raise 用于本层不处理、assert 用于钉住假设 |
| [sort-vs-sorted](capabilities/sort-vs-sorted.md) | 路由承载 | 原顺序还要不要用？要则 sorted，不要则 sort |
| [list-comprehension](capabilities/list-comprehension.md) | 路由承载 | 生成 / 映射 / 筛选的一行写法 |
| [string-encoding-split-join](capabilities/string-encoding-split-join.md) | 路由承载 | split 默认按空白、join 的调用者才是分隔符、errors 默认 strict |
| [string-formatting](capabilities/string-formatting.md) | 路由承载 | `%` 与 `format()` 两张字符表；进制转换用 b/o/x |
| [operator-precedence](capabilities/operator-precedence.md) | 路由承载 | 「谁先算」与「复合赋值等价于什么」 |
| [input-and-print](capabilities/input-and-print.md) | 路由承载 | `variable = input("提示文字")` 与 print 的三种输出内容 |
| [module-import-and-pip](capabilities/module-import-and-pip.md) | 路由承载 | 先用标准库表判断要不要装，再用 pip 装/卸 |
| [file-and-directory-operations](capabilities/file-and-directory-operations.md) | 路由承载 | 打开—写入—关闭；删文件必须走 os |
| [database-access](capabilities/database-access.md) | 路由承载 | 连接 → 游标 → 执行 → 取结果 → 提交/关闭 |
| [process-and-thread](capabilities/process-and-thread.md) | 路由承载 | Process / Thread 同构参数 + Lock 加解锁成对 |
| [socket-communication](capabilities/socket-communication.md) | 路由承载 | 服务端 bind → listen → accept；客户端 connect |
| [regex-construction](capabilities/regex-construction.md) | 路由承载 | 匹配什么（元字符）+ 重复几次（限定符）+ 什么行为（标志） |
| [gui-window-construction](capabilities/gui-window-construction.md) | 路由承载 | 窗口 = 构造器 + 参数；sizer 负责布局 |

**去向口径**：`独立 Skill` 表示阶段 1.6 晋级门通过（7 张）；`路由承载` 表示通过知识验证但按预算排序保留为能力卡（13 张），
经来源路由入口 `python-quick-reference-router` 可达——**没有能力被删除或失联**。

## 参考区（不是可执行方法，保留为速查与底本证据）

> 本节落地 `references.md` 登记的 12 条 `reference` 与 4 项背景材料。
> **硬约束**：A 类的损毁表格不得用常识补全；B 类只作名称→用途速查，**不为其编写使用步骤**；D 类不是候选池的一部分。

### A. 表格损毁类（规则可读、表格不可重建）

| 条目 | 出处 | 只保留什么 | 不要做什么 |
|---|---|---|---|
| 转义字符 | 第 2 章 L237-261 | 规则句：「所谓转义字符是指使用反斜杠"\"对一些特殊字符进行转义」（L239）；书中取值名含续行符、换行符、水平制表符、双引号、单引号、反斜杠、换页、八进制数、十六进制数 | **符号列整体丢失**，照它重建符号表即为凭常识编造 |
| 数据类型转换函数表 | 第 2 章 L263-287 | 规则：「要转换就选对应函数」 | 表格列错位：`ord()` 行的函数名被下一行 `hex` 顶掉（引文写作"将一个字符x转换为它对应的整数值 hex(x)"）；`(x)p.10` 是页码残留。**本项目不用常识回填，task-12 因此无 verified 承担（NR-02）** |
| Surface（Pygame） | 第 16 章 L2377-2391 | 作术语保留（见 `glossary.md`） | 书中**没有定义句**，只有「方法作用于其上」的列举，不作能力证据 |

### B. 纯对照表类（名称 → 用途，无可执行流程）

| 表 | 出处 | 内容口径 |
|---|---|---|
| GUI 工具包选型表 | 第 15 章 L2139-2165 | wxPython / Kivy / Flexx / PyQt / Tkinter / Pywin32 / PyGTK / pyui4win，每条一句定位；第 15 章可执行的部分只有 wxPython 控件构造（见 [gui-window-construction](capabilities/gui-window-construction.md)） |
| Pygame 常用模块表 | 第 16 章 L2307-2355 | 21 个模块的名称→功能；**书中没有游戏循环代码** |
| Pygame display 与 Surface 方法表 | 第 16 章 L2357-2391 | 方法清单完整，但无调用示例、无主循环 |
| urllib 子模块 + 爬虫框架简介 | 第 17 章 L2431-2461 | urllib 四个子模块职责；Scrapy / Crawley / PySpider 各一句介绍 |
| HTTP 方法语义表 | 第 21 章 L2703-2731 | GET/HEAD/POST/PUT/DELETE/OPTIONS 的概念对照；**全书未出现 Flask 路由写法**（`@app.route` 零次） |
| Flask 常用过滤器表 | 第 21 章 L2733-2751 | 过滤器名称→用途；书中无模板语法示例（`{{ }}` 未出现） |
| WTForms 字段类型表 | 第 21 章 L2753-2789 | 17 种字段类型对照；无使用代码 |
| WTForms 验证函数表 | 第 21 章 L2791-2815 | 10 种验证函数对照；该段首行混入源文串接缺陷（见「已知限制」） |

### C. 辅助栏目

| 栏目 | 出处 | 说明 |
|---|---|---|
| 英语词汇 / 热度（★） | 全书 **21 处**（每章一处，首见 L143-177） | 英文关键词 + 中文含义 + ★热度评级（含 ★ 的行共 63 行）。**没有方法动作**，作为整体保留（术语表 `glossary.md` 有 g26 条目说明），不逐词建卡 |

### D. 原书存在但未收为候选的背景材料（有出处、无方法动作）

| 材料 | 出处 | 说明 |
|---|---|---|
| 目录（21 章清单） | L1-41 | 导航信息；正文标题形式不统一，见「基本信息」 |
| 第 19 章网络编程前沿知识：IP 协议、TCP 协议、Socket 简介 | L2589-2600 | 三句概念介绍；Socket 的**方法表**由 [socket-communication](capabilities/socket-communication.md) 承担 |
| 第 20 章 Web 概念：HTTP / HTML / CSS / JavaScript / CGI / WSGI 定义 | L2665-2685 | 纯定义句，无可执行内容 |
| 固定标语「背记有法，让英语不再成为编程学习的拦路虎！」 | **21 处**（每章一处，如 L145 / L305） | 栏目口号，非知识 |

## 已知限制（必须与产物一起交付）

1. **底本是 OCR 整理稿**（NR-15）：圆括号 `()` 被识别成字母 `O`（`inputO`/`printO`/`openO`/`splitO`/`listO`/`sortO`/`removeO`/`acquireO`…）、`\S` → `Is`、`<<`/`>>` → `ΛΛ，VV`、花括号 `{}` → 圆括号、`-l` 实为 `-1`；至少 10 张表被压成单列；个别段落混入不相关内容（L2791 的 WTForms 一节里混进 Django 的句子）。
   **引用一律照抄原稿**；本项目**不能声称已穷尽源文的所有拼接/识别缺陷**，能声称的是「已核验的 166 段引文保真度 hard = 0」（`.cangjie/reports/quote-verification.md` 可复算）。
2. **章结构与内容重复**（NR-16）：第 1 章的「必背必记 1-8」与第 4 章 L597-695 是同一段文字原样重出；**切片**在第 5 章与第 7 章各讲一遍且参数说明逐字同构。均**按一处证据计**，不因出现两次而提高权重。
3. **数据类型转换表整条任务无能力承担**（NR-02）：task-12 在覆盖审计中如实记为缺口，未用常识回填。
4. **8 项「调用层缺失」**（NR-07 ~ NR-14）：本书多处只给零件不给装配图——`input()` 返回值类型、`open()` 签名行、`connect()` 调用示例、`Thread/Process` 的 start/join、socket 创建与 import、`re` 调用函数、`wx.App` 与主循环。相关能力卡的 B 段逐条标注「本书未覆盖」，下游不得补造。
5. **本书面向「背」而非「用」**：全书没有任何完整程序或项目，只有语法片段与参数说明；不覆盖虚拟环境、依赖锁定、类型注解、`with`、测试与调试器（阶段 0 批判节记为时代局限）。
