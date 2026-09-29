<!-- capability_id: cap.automate-boring-stuff.excel-spreadsheets | revision: 1 | status: active -->
<!-- 来源: 单元 v11；归并候选 10 条（f42/f43/f44/p63/p64/g27/c25/c26/c27/ce24）；10 条候选；promoted；含 ce24（覆写原表）与 NR-04 版本风险 -->

# Excel 电子表格处理（openpyxl 读写、单元格/行列、公式与样式、批处理骨架）

## R — 原文 (Reading)

> 当修改Workbook 对象或它的工作表和单元格时，电子表格文件不会保存，除非你调用save()工作簿方法。
> …
> 当你编辑从文件中加载的一个电子表格时，总是应该将新的、编辑过的电子表格保存到不同的文件名中。这样，如果代码中有缺陷，导致新的保存到文件中数据不对或讹误，还有最初的电子表格文件可以处理。
>
> — 第12章 · 处理Excel 电子表格（12.5.1 创建并保存Excel 文档）

> …2．调用openpyxl.load_workbook()函数。
> …4．调用get_active_sheet()或get_sheet_by_name()工作簿方法。
> …8．读取Cell 对象的value 属性。
>
> — 第12章 · 处理Excel 电子表格（12.3.6 工作簿、工作表、单元格）

## I — 方法论骨架 (Interpretation)

对象链先摆正：**工作簿（文件）→ 工作表 → 单元格**；单元格行列**从 1 开始**（与列表下标相反）。

- 读取八步：`import openpyxl` → `load_workbook()` → Workbook → `get_active_sheet()`/`get_sheet_by_name()` → Worksheet → 索引或 `cell(row=, column=)` → `value`；通用骨架：加载文件 → 准备数据结构 → 逐行遍历。
- 遍历用 `get_highest_row()` 一类口径取边界、跳表头；聚合用嵌套字典（`setdefault` 建层）。
- **写入是「内存中改、显式落盘」**：修改不会自动保存，必须 `wb.save(文件名)`。
- **两条保命规则**：①改动必须 `save()`；②编辑既有表**总是另存新文件名**，留原件回退，确认无误后再删。
- 批量改值用字典（如 PRICE_UPDATES）替代硬编码 if 链；列字母↔数字用 `get_column_letter()`/`column_index_from_string()` 互转；`sheet.freeze_panes = 'A2'` 冻结上边行与左边列、不含所在行列。

## A1 — 书中的应用 (Past Application)

### 案例 1: 12.4 普查数据聚合 readCensusExcel.py
- **类型**: 书中项目（第12章）
- **问题**: 遍历普查表几千行、算每县人口与普查区数；「整张电子表格也需要几个小时时间」。
- **方法论的使用**: 按骨架读表（`for row in range(2, get_highest_row()+1)`）`setdefault` 建 `countyData[州][县] = {'tracts': 0, 'pop': 0}` 累加；用 `pprint.pformat()` 写成可 import 的 census2010.py。
- **结论**: 数据结构先定、再写填充代码。
- **结果**: 实测 `>>> census2010.allData['AK']['Anchorage']` 得 `{'pop': 291826, 'tracts': 55}`；「手工计算需要数小时，这个程序只要几秒钟」。

### 案例 2: 12.6 更新价格表 updateProduce.py
- **类型**: 书中项目（第12章）
- **问题**: 价格录错、几千行待更新；「不能简单地对价格查找替换」（同价产品会被误改）。
- **方法论的使用**: `PRICE_UPDATES = {'Garlic': 3.07, …}` 替代硬编码 if 链；第 2 行起读第 1 列产品名、命中写第 2 列价格；`wb.save('updatedProduceSales.xlsx')` 另存新文件。
- **结论**: 「它没有覆写原来的电子表格，以防万一程序有缺陷」。
- **结果**: 未说明。

## A2 — 触发场景 (Future Trigger) ★

### 用户会在什么情境下需要这张卡?

1. 要批量汇总几千行的表（统计、筛选）
2. 要批量更新单元格（改价、填标记），或把结果写回 Excel
3. 要把「几千行手工统计」变成脚本或周期化
4. 涉及列字母与列号换算，或想改公式、样式、冻结窗格

### 语言信号

- 「把这个表格的数据汇总一下」「批量更新价格列」「怎么把结果写回 Excel」「改完怎么没保存」
- 英文: "summarize this spreadsheet", "update prices", "write to Excel", "loop over rows"

### 与相邻能力的区分（阶段 3 定稿）

- 与 `dicts-and-modeling` 的区别（composes-with）: 本卡管表和单元格**怎么读写**；聚合结果的**数据结构设计**走那条卡。
- 与 `csv-and-json` 的区别（composes-with）: .xlsx 与 CSV/JSON 是同一批数据的两种载体；目标是 CSV 时不套 openpyxl。
- 与 `email-and-sms` 的区别（composes-with）: c35 是「Excel 名单 + sendmail」流水线——名单本卡出，发送走那条卡。

## E — 可执行步骤 (Execution)

1. **打开与选表**——完成标准: `load_workbook('x.xlsx')` → `get_sheet_by_name(...)`/`get_active_sheet()`；行列从 1 起。
2. **遍历与聚合**——完成标准: 先定数据结构；`for row in range(2, sheet.get_highest_row()+1)`（跳表头）用 `sheet.cell(row=, column=).value` 取值、setdefault 建层。
3. **写入与保存（两条保命规则）**——完成标准: 改动写进 `Cell.value` 后**必须** `wb.save(新文件名)`；编辑既有表**总是另存**、不覆写原件。
4. **收尾核对**——完成标准: 列字母复核（'AHP'↔900 以原文为准）；公式列随输入自动重算；打开保存后文件核对。

**输入契约**: 必需＝①.xlsx 文件（或新建）②要读/要改的工作表与列 ③输出文件名（编辑既有表时强制新文件名，缺则先问）。CSV 场景转 `csv-and-json`。
**输出契约**: 交付＝①脚本 ②另存的新文件 ③`wb.save()` 与列口径；能回指 f42–f44 / p63。

## B — 边界 (Boundary) ★

### 不要在以下情况使用这张卡

- 数据载体是 CSV/JSON → `csv-and-json`。
- 工作簿含图表且要往返保存 → p63 警告：加载不载入图表，同文件名保存「实际上就会删除其中的图表」。
- 需要现代 openpyxl API 签名 → 按源文（2.1.4）口径记录，执行前按官方文档核对。

### 作者在书中警告的失败模式

- ce24 覆写原表没有退路: 直接保存回原文件名，缺陷（误判行、误改价格）直接写进数据、无对照可回滚；作者写法：新文件（updatedProduceSales.xlsx），确认后才删原件。
- p63 不 save 等于没改；c27 编辑既有表要另存新文件名。

### 作者盲点与底本缺口

- **版本改名（unit_note）**: openpyxl API 后续版本有改名（如 get_sheet_by_name），以源文为准，执行前按官方文档核对。
- **版本风险（NR）**: 本书示例为 2016 年版；同名库可能已改名或调整语义；不得把版本变化导致的失败计为能力缺陷；遍历上限用 get_highest_row() 一类旧口径。
- **OCR 错位（NR）**: 三张表 OCR 串行错位（表10-1/7.10/表12-3）；引用以「正文逐句陈述 + 单点示例」为准——表 12-3 被压平，冻结窗格只用正文口径。
- 多字母列无换算公式（只有互查样例）；'AHP'↔900 以原文为准。

### 容易混淆的邻近方法论

- `dicts-and-modeling`: 聚合结构设计；`csv-and-json`: 换载体口径；`email-and-sms`: 发送动作。

---

## 相关能力（阶段 3 定稿）

- composes-with: `dicts-and-modeling`（读表聚合用字典组织，c25/c26）；`csv-and-json`（Excel 与 CSV/JSON 互转）
- composes-with: `email-and-sms`（Excel 名单 + sendmail 流水线，c35）

---

## 审计信息

- **验证通过**: V1 ✓ / V2 ✓ / V3 ✓（阶段 1.5，单元 v11）
- **证据**: 候选 10 条（f42/f43/f44/p63/p64/g27/c25/c26/c27/ce24）；ck-12acee24326e / ck-1af0692dd439 / ck-2ae3e05de504
- **去向**: 阶段 1.6 promoted 独立入口
- **蒸馏时间**: 2026-09-18（分片 B）
