<!-- capability_id: cap.automate-boring-stuff.file-read-write | revision: 1 | status: active -->
<!-- 来源: 单元 v07；归并候选 13 条（f17/f18/f19/f20/p47/p48/p49/g22/g23/c15/c16/ce14/ce15）；13 条候选；底座型单元（v08/v11/v12/v13 的前置） -->

# 文件读写与路径操作（三步协议、'w' 覆写、cwd 锚点、os.path、shelve）

## R — 原文 (Reading)

> 在Python 中，读写文件有3 个步骤：
> 1．调用open()函数，返回一个File 对象。
> 2．调用File 对象的read()或write()方法。
> 3．调用File 对象的close()方法，关闭该文件。
>
> — 第8章 · 读写文件（8.2 文件读写过程 L5528-5544）

> 写模式将覆写原有的文件，从头开始，就像你用一个新值覆写一个变量的值。将'w'作为第二个参数传递给open()，以写模式打开该文件。不同的是，添加模式将在已有文件的末尾添加文本。
> ……
> 如果传递给 open()的文件名不存在，写模式和添加模式都会创建一个新的空文件。
>
> — 第8章 · 读写文件（8.2.1 打开文件 L5546-5562；8.2.3 写入文件 L5586-5602）

## I — 方法论骨架 (Interpretation)

文件是自动化程序的输入输出总线。协议三步：`open()` 得 File 对象 → `read()/readlines()` 或 `write()` → `close()`。`read()` 给大字符串，`readlines()` 给行列表（更好处理）。三条要记：省略模式或 `'r'` 是读模式；`'w'` 是**从头覆写**（不是「打开待写」），追加要显式 `'a'`；`'w'/'a'` 遇不存在的文件名会新建空文件，`write()` 不自动加换行符。写完必须 close()。

路径：绝对路径从根文件夹开始，相对路径相对**当前工作目录（cwd）**——cwd 由启动位置决定（与脚本位置无关）；`os.getcwd()` 取、`os.chdir()` 改（改到不存在目录会报错）。跨平台拼路径用 `os.path.join()`；`os.path` 函数族：dirname/basename/split/getsize（字节数）/exists/isdir/isfile。动手前先判路径存在性。

持久化两条轻路线：`shelve`（按关键字存取变量，像字典，用完 close()）与 `pprint.pformat`（嵌套结构转字符串再 write()）；本单元函数只适用于纯文本文件。

## A1 — 书中的应用 (Past Application)

### 案例 1: randomQuizGenerator.py——35 套乱序测验
- **类型**: 书中项目（第8章）
- **问题**: 要 35 份不重样的州首府测验，防抄袭。
- **方法论的使用**: capitals 字典放框架；循环 35 次，每份建 quizFile 与 answerKeyFile 写表头；`random.shuffle()` 打乱州序；每题取 3 个错答案与正确答案打乱成 A–D 写出，正确字母写进答案文件；用完 close()。
- **结论**: 字典 + 文件写入可批量生成成套文档。
- **结果**: 生成 capitalsquiz1..35.txt 与答案文件（示例开头「Name: Date: Period: State Capitals Quiz (Form 1)」）。

### 案例 2: mcb.pyw——多重剪贴板（shelve 持久化）
- **类型**: 书中项目（第8章）
- **问题**: 剪贴板一次只有一段，要按关键字存取多段。
- **方法论的使用**: `shelve.open('mcb')`；`save <keyword>` 时 `mcbShelf[sys.argv[2]] = pyperclip.paste()`；单参数时 'list' 把关键字列表拷到剪贴板，否则取该键的值；末尾 close()。
- **结论**: shelve 让剪贴板内容跨次运行保留（口令保管箱的升级版）。
- **结果**: 未说明——书中未给运行输出样张。

## A2 — 触发场景 (Future Trigger) ★

### 用户会在什么情境下需要这张卡?

1. 把一个 txt 读成行列表逐行处理，结果写成新文件。
2. 写文件但怕覆盖原文件（'w' 的语义）。
3. 文件明明存在却报「找不到」，或换个目录运行就失败（相对路径锚点）。
4. 要路径拼接、取文件大小/目录/文件名，或判断路径类型。

### 语言信号

- 「读这个 txt 逐行处理」「把结果写到新文件」
- 「写文件会不会覆盖原来的」「文件明明在却说不存在」
- 英文: "read a file line by line"

### 与相邻能力的区分（阶段 3 定稿）

- 与 `strings-and-clipboard`（composes-with）: 本卡管读写出入，清洗走那条卡。
- 与 `regex-patterns`（composes-with）: 对文件内容批量提取/替换时，正则嵌在本卡的读写流程里。
- 与 `organize-files`（depends-on）: 本卡管「一个文件的内容」，那条卡管**文件本身**的复制/移动/改名/删除。
- 与 `csv-and-json`（composes-with）: CSV/JSON 读取与落盘走本卡文件协议，编解码规则走那条卡。
- 与 `pdf-and-word`（depends-on）: 本卡只适用于纯文本；PDF/Word 走各自二进制接口。

## E — 可执行步骤 (Execution)

1. **明确对象与意图**
   - 完成标准: ①路径；②读/写、覆盖还是追加；③粒度（大字符串/行列表）。
   - 判停条件: 输出会覆盖同名文件而用户没说时先确认。

2. **三步协议读写**
   - 完成标准: `open()` → `read()/readlines()` 或 `write()` → `close()`；写完必须 close() 才能再次打开；write 要自己补换行符。

3. **写前做覆盖检查**
   - 完成标准: 对已存在文件说明本次是覆盖（'w'）还是追加（'a'）；输出与原文件不同路径。
   - 判停条件: 无法确认时先写新路径——'w' 打开即清空（ce14）。

4. **路径按 cwd 口径核算**
   - 完成标准: 说明相对路径锚在 cwd（不是脚本目录）；跨平台用 `os.path.join()`；元数据用 getsize/dirname/basename。
   - 判停条件: 无人值守/跨目录场景拼绝对路径；持久化小数据用 shelve/pprint（见 I 段）。

**输入契约**: ①路径；②读/写与覆盖或追加；③交付形态。缺 ① 或覆盖意图先问。

**输出契约**: 调用序列（open → read/readlines 或 write → close）+ 路径口径说明（cwd、join、大小取法）。

## B — 边界 (Boundary) ★

### 不要在以下情况使用这张卡

- 批量复制/移动/改名/删除/压缩文件本身——转 `organize-files`。
- 二进制模式（'rb'/'wb'）、with 语句、字符编码处理——书中未覆盖，如实说明。

### 作者在书中警告的失败模式

- 'w' 覆写（ce14）: 'w' 不是「打开待写」而是「从头开始写」——打开一瞬间原内容即清空；ZIP 同理（`ZipFile(..., 'w')` 丢全部成员，追加要 'a'）。告警：重跑后文件一次次变小。
- 相对路径锚点（ce15）: 相对路径相对 cwd 解析，cwd 由启动位置决定（双击/换目录/调度器各不同）；`os.chdir()` 到不存在目录直接报错。告警：FileNotFoundError 但文件存在。

### 作者盲点与底本缺口

- **unit_note**: c15/c16 是两个完整项目；f19 的 shelve 是「小数据库」路线，为 v14/v15 持久化备选。
- **nr_warnings**: 本单元无表格类警告；小结有 OCR 错字（`read()` 印成 reae()、`open()` 印成 Open()），引用照原样。
- **书中未覆盖**: with 语句、二进制模式（'r+'/'wb'）、shelve 并发冲突；`os.chdir()` 失败示例仅为 Windows 路径。

### 容易混淆的邻近方法论

- `organize-files`: 本卡管「一个文件的内容」，那条卡管「一批文件本身」且带危险动作护栏；相对路径锚在 cwd（≠脚本目录）。

---

## 相关能力（阶段 3 定稿）

- composes-with: `strings-and-clipboard`（清洗与读写是上下游）
- composes-with: `regex-patterns`（批量提取/替换嵌在读写流程里）
- depends-on: `organize-files`（v08 的路径拼接与目录口径来自本卡）
- composes-with: `csv-and-json`（CSV/JSON 读取与落盘走文件协议）
- depends-on: `pdf-and-word`（PDF/Word 走各自的二进制接口）

---

## 审计信息

- **验证通过**: V1 ✓ / V2 ✓ / V3 ✓（阶段 1.5，单元 v07）
- **证据**: 候选 13 条（f17/f18/f19/f20/p47/p48/p49/g22/g23/c15/c16/ce14/ce15）；ck-dee629da19df / ck-122553e0e4f0 / ck-c5cab912519d
- **去向**: 阶段 1.6 router 经来源路由入口可达
- **蒸馏时间**: 2026-09-18（分片 B）
