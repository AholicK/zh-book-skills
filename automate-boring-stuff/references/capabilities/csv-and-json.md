<!-- capability_id: cap.automate-boring-stuff.csv-and-json | revision: 1 | status: active -->
<!-- 来源: 单元 v13；归并候选 7 条（f47/f48/f49/p66/c29/c30/ce29）；7 条候选；含 ce29（大文件内存）与 NR-05（含混表述）登记 -->

# CSV 与 JSON 数据处理（csv 模块读写、json 的 loads/dumps、从 API 取数）

## R — 原文 (Reading)

> 但并非 CSV 文件中的每个逗号，都表示两个单元格之间的分界。……因为这些潜在的缺陷，所以总是应该使用csv 模块来读写CSV 文件。
>
> — 第14章 · 处理CSV 文件和JSON 数据「14.1 csv 模块 / 14.4 json 模块」L9122-9389

> 要将包含JSON 数据的字符串转换为Python 的值，就将它传递给json.loads()函数（这个名字的意思是“load string”，而不是“loads”）。
> ……
> json.dumps()函数（它表示“dump string”，而不是“dumps”）将一个Python 值转换成JSON 格式的数据字符串。
>
> — 第14章 · 处理CSV 文件和JSON 数据「14.1 csv 模块 / 14.4 json 模块」L9122-9389

## I — 方法论骨架 (Interpretation)

这张卡处理两类「程序与程序之间」的标准数据格式：CSV（表格文本）与 JSON（Web API 应答）。核心判据是**不许自己解析**：CSV 值内部可能含逗号与转义字符，`split(',')` 分不出「分隔符」与「值的一部分」——读用 `csv.reader(文件对象)`、写用 `csv.writer(文件对象)`，传打开后的文件对象而非文件名。

`list(reader)` 得「列表的列表」（`data[row][col]` 取值）；Reader 只能遍历一次，重读要重建；大文件必须 for 逐行（ce29）。写出时 Windows 上 `open(..., 'w', newline='')` 否则行距翻倍；`writerow(列表)` 一次一行。

JSON 只差一字：`json.loads(字符串)`（load string）→ Python 值；`json.dumps(Python值)`（dump string）→ JSON 字符串；可表示类型仅字符串、整型、浮点型、布尔型、列表、字典、NoneType。

取 API 链条：`requests` 拿回的 `response.text` 只是「大字符串」→ `json.loads()` → 按字段路径逐层取。

## A1 — 书中的应用 (Past Application)

### 案例 1: 从 CSV 文件中删除表头（removeCsvHeader.py）
- **类型**: 书中项目（第14章）
- **问题**: 几百个 CSV 要删掉每列顶部表头行，手工处理要几小时
- **方法论的使用**: 循环 os.listdir('.') 跳过非 .csv；csv.reader 逐行读、用 reader.line_num 判断并跳过第一行；csv.writer 写出其余行到先建好的 headerRemoved 文件夹
- **结论**: 作者警告「当你写程序修改文件时，一定要先备份这些文件」
- **结果**: 逐行打印 Removing header from NAICS_data_1048.csv... 等；无表头副本写入 headerRemoved

### 案例 2: 取得当前的天气数据（quickWeather.py）
- **类型**: 书中项目（第14章）
- **问题**: 要脚本打印今明后三天的天气，不经过浏览器
- **方法论的使用**: sys.argv 拼地点 → requests.get() + raise_for_status() → json.loads(response.text) → 按字段路径打印 main 与 description；作者提醒先 pprint 看结构、再查在线文档（如温度是开尔文）
- **结论**: 未说明
- **结果**: 实测输出 Current weather in San Francisco, CA: Clear - sky is clear 等

## A2 — 触发场景 (Future Trigger) ★

### 用户会在什么情境下需要这张卡?

1. 读写 CSV 时用 split(',') 自己解析，遇引号内逗号就错乱
2. 从 API 拿到应答字符串就直接当下标用而报错（漏了 `json.loads()`）
3. 要批量清洗一批 CSV（删表头、改列、合并）
4. 要把 Python 数据存成 JSON 落盘、或转 JSON 传给别的程序

### 语言信号

- 「CSV 怎么读 / 怎么读不出来」「JSON 解析不出来 / 拿到的怎么是字符串」
- 「调个接口拿数据」「requests 返回的字段怎么取」
- 英文: "parse CSV", "JSON decode", "call an API"

### 与相邻能力的区分（阶段 3 定稿）

- 与 `dicts-and-modeling` 的区别: 本卡管字符串↔结构化数据的转换；解析结果的建模与字段路径口径在那条卡
- 与 `file-read-write` 的区别: 本卡聚焦格式层（csv/json 模块）；open 模式、路径与落盘协议走那条卡
- 与 `web-scraping` 的区别: 同一段 requests，应答是 JSON 时用本卡口径；应答是 HTML 要解析标签时走那条卡
- 与 `excel-spreadsheets` 的区别: 同一批表格数据的两种载体——CSV 按本卡 csv 口径，.xlsx 工作簿操作走那条卡

## E — 可执行步骤 (Execution)

1. **定格式与方向**
   - 完成标准: 明确「读还是写」「CSV 还是 JSON」「来源是本地文件还是 Web API」
   - 判停条件: .xlsx 转 `excel-spreadsheets`；抓 HTML 转 `web-scraping`
2. **CSV 读：csv.reader 逐行迭代**
   - 完成标准: 用 `csv.reader(文件对象)`；小文件可 `list(reader)`、大文件 for 逐行（ce29）；行号用 reader.line_num；重读要重建 Reader
3. **CSV 写：open('w', newline='') + writerow**
   - 完成标准: Windows 上 open() 带 `newline=''`；`writerow(列表)` 一行；需要时传 delimiter / lineterminator
4. **JSON：分清 loads / dumps**
   - 完成标准: 字符串→Python 值 `json.loads()`；Python 值→字符串 `json.dumps()`；只用七类可表示类型
5. **API 取数与批量自检**
   - 完成标准: requests.get() → raise_for_status() → json.loads(response.text) → 按字段路径取；遍历文件跳过非目标扩展名、改前先备份；交付注明口径来源

**输入契约**: 必需 = ①格式 ②方向（读/写）③来源（本地路径或 API 端点）④单文件或批量。缺 ③ 先问；批量改文件先确认备份方式。

**输出契约**: 代码 + 口径说明（Reader/Writer、newline=''、loads/dumps、大文件逐行）；完成标准：调用与参数可回指 p66/f47/f49；批量场景「原文件不动、结果另存」。

## B — 边界 (Boundary) ★

### 不要在以下情况使用这张卡

- 数据在 .xlsx 工作簿里（转 `excel-spreadsheets`）；目标是抓网页内容（转 `web-scraping`）
- 二进制格式、数据库等书中未覆盖的载体；大文件要并行（书里只到逐行迭代）

### 作者在书中警告的失败模式

- 用 split(',') 自解析（p66）：「split()方法不能处理这些转义字符」
- 大文件一次性读入内存（ce29）：「你需要在一个for 循环中使用Reader 对象」；「不检查无效数据/格式错误」也被列为缺陷
- 批量改文件不备份（c29/f48）：「一定要先备份这些文件」

### 作者盲点与底本缺口

- **NR-05**：ch14「副本覆盖原文件」段落语义含混，不得当安全警告发布；「原文件是否被替换」按代码语义核对
- **外部不稳定源**：c30 的 OpenWeatherMap 端点为 2015 年形态，字段/接口可能已变——以服务商当前文档为准
- **书中未覆盖**：编码、二进制模式、`with` 语句、JSON 模式校验、CSV dialect 细节；底本为 OCR 稿，引文照抄

### 容易混淆的邻近方法论

- `file-read-write`：底层读写协议 vs 本卡的格式层
- `dicts-and-modeling`：解析结果的数据建模
- `web-scraping`：requests 取回后的另一条解读路径
- `excel-spreadsheets`：同一批数据的 Excel 载体

---

## 相关能力（阶段 3 定稿）

- composes-with: `dicts-and-modeling`（JSON 解析结果即 dict/list 嵌套）
- composes-with: `file-read-write`（CSV/JSON 读写走文件协议）
- composes-with: `web-scraping`（requests 取回 + json.loads 的完整调用法）
- composes-with: `excel-spreadsheets`（表格数据在 Excel 与 CSV/JSON 间流转）

---

## 审计信息

- **验证通过**: V1 ✓ / V2 ✓ / V3 ✓（阶段 1.5，单元 v13）
- **证据**: 候选 7 条（f47/f48/f49/p66/c29/c30/ce29）；ck-34f33a140c17 / ck-d750a1f9b369 / ck-bbf68bf193ac
- **去向**: 阶段 1.6 router 经来源路由入口可达（预算降级·排序第 11）
- **蒸馏时间**: 2026-09-18（分片 B）
