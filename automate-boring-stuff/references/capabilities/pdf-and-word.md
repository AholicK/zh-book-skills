<!-- capability_id: cap.automate-boring-stuff.pdf-and-word | revision: 1 | status: active -->
<!-- 来源: 单元 v12；归并候选 8 条（f45/f46/p65/c28/ce25/ce26/ce27/ce28）；8 条候选；promoted；含 ce25–ce28 四个边界（提取不可靠/加密/样式名/弱口令） -->

# PDF 与 Word 文档处理（PyPDF2 读写合并加解密、python-docx 读样式与写文档）

## R — 原文 (Reading)

> PyPDF2 在取得页面时使用从0 开始的下标：第一页是0 页，第二页是1 页，以此类推。事情总是这样，即使文档中页面的页码不同。
> …
> PyPDF2 不能在PdfFileWriter 对象中间插入页面，addPage()方法只能够在末尾添加页面。
>
> — 第13章 · 处理PDF 和Word 文档（13.1.1、13.1.4）

> 虽然PDF 文件对文本布局非常好，让人们很容易打印并阅读，但软件要将它们解析为纯文本却并不容易。因此，PyPDF2 从PDF 提取文本时可能会出错，甚至根本不能打开某些PDF。遗憾的是，你对此没有什么办法，PyPDF2 可能就是不能处理某些PDF 文件。
>
> — 第13章 · 处理PDF 和Word 文档（13.1.1）

> 要创建自己的.docx 文件，就调用docx.Document()，返回一个新的、空白的Word Document 对象。Document 对象的add_paragraph()方法将一段新文本添加到文档中，并返回添加的Paragraph 对象的引用。
>
> — 第13章 · 处理PDF 和Word 文档（13.3 Word 文档）

## I — 方法论骨架 (Interpretation)

PDF 侧（PyPDF2）对象是内存中的 Reader/Writer：页面下标从 0 起（与印刷页码无关）、`numPages` 取总页数、`addPage()` 只能末尾追加；读用 'rb'、写用 'wb'。合并五步：找 PDF（排序）→ 逐个 Reader → 逐页 getPage/addPage（可跳过封面）→ write → close。

加密前置：先查 `isEncrypted`，未解密就读取必失败；`decrypt('口令')` 返回 0/1 报告成败，且只解密内存对象、硬盘文件仍加密——每次运行都要重新解密；`encrypt(用户口令, 拥有者口令)` 须在 write() 前调用。

文本提取是**不可靠能力**（R 段原文）——必须人工抽查，不能当精确数据来源。

Word 侧（python-docx）：Document → Paragraph → Run。读——`docx.Document('demo.docx')`、`doc.paragraphs`（len 得段数）、`text`、`runs`；写——`docx.Document()` → `add_paragraph()` → `add_run()` → `add_heading('Header 0', 0)` → `doc.save()`；样式名去空格。

## A1 — 书中的应用 (Past Application)

### 案例 1: 13.2 合并 PDF combinePdfs.py
- **类型**: 书中项目（第13章）
- **问题**: 几十个 PDF 合并成一个、各文件第一页是封面不希望重复；「即使有许多免费的程序可以合并PDF，很多也只是简单的将文件合并在一起」。
- **方法论的使用**: `os.listdir('.')` 收集 PDF 并按 `key=str.lower` 排序 → PdfFileWriter → 逐文件 open/PdfFileReader，循环从 1 到 numPages（跳过第 0 页封面）getPage/addPage → 'wb' 写出 allminutes.pdf。
- **结论**: 「循环应该从1 开始」；本程序能定制要合并的页面。
- **结果**: 未说明（未给运行输出样本）。

## A2 — 触发场景 (Future Trigger) ★

### 用户会在什么情境下需要这张卡?

1. 要把几十个 PDF 合并成一个（去封面），或拆分/加水印/旋转
2. 要读取 PDF/Word 内容，且接受「提取文本先抽查」
3. 要批量生成 Word 文档（邀请函）
4. 处理加密 PDF：脚本需要检查口令、解密后才能读
5. 提取文本缺字/错序，想知道是不是方法错了

### 语言信号

- 「把几个 PDF 合并成一个 / 拆分、加水印」「读一下这个 Word / PDF」「生成一批邀请函」
- 「PDF 打不开 / 文本提取不对」
- 英文: "merge PDFs", "read docx", "generate Word files", "PDF is encrypted"

### 与相邻能力的区分（阶段 3 定稿）

- 与 `file-read-write` 的区别（depends-on）: 本卡依赖文件读写协议（'rb'/'wb' 句柄）；open/close/路径走那条卡。
- 与 `organize-files` 的区别（composes-with）: combinePdfs 是「按目录批量处理文件」的文档版（遍历 + 排序 + 输出命名 + 跳过首页）；目录遍历走那条卡。

## E — 可执行步骤 (Execution)

1. **确认目标与前置检查**——完成标准: 明确①输入文件清单②要做合并、读文本还是生成③是否加密——先查 `isEncrypted`。
2. **PDF 读取**——完成标准: `open('x.pdf','rb')` → `PdfFileReader(...)` → `numPages`、`getPage(n)` 从 0 起；加密先 `decrypt('口令')` 并检查返回值（0/1）。
3. **PDF 写出**——完成标准: `PdfFileWriter` → 逐页 `addPage()`（只能末尾追加）→ `mergePage(水印页)`/`rotateClockwise()`（90/180/270 整数倍）→ `open(输出,'wb')` 后 `write()`、`close()`；`encrypt(...)` 在 write 前。
4. **Word 读取与生成**——完成标准: 读——`docx.Document('demo.docx')` → `doc.paragraphs`（len 得段数）→ `text`/`runs`；写——`docx.Document()` → `add_paragraph()` → `add_run()` → `add_heading('Header 0', 0)` → `doc.save()`；样式名去空格。
5. **文本提取自检**——完成标准: `extractText()` 结果人工抽查，确认不可靠时如实告知。

**输入契约**: 必需＝①输入文件（PDF/Word 路径）②目标产物 ③加密文件的处理方式（口令从哪来）——口令不要猜；缺③先问。口令破解不在本卡（见 B 段）。
**输出契约**: 交付＝①脚本（Reader→Writer→write 或 Document→add_paragraph→save）②产物文件与命名 ③已知限制的明示（提取不可靠、样式名不带空格、加密需每次解密）；能回指 f45/f46/p65。

## B — 边界 (Boundary) ★

### 不要在以下情况使用这张卡

- 下游要求「精确文本」：先人工抽查，要求高时改换数据源。
- 口令破解/绕过加密：13.6 暴力破解只保留为 ce28 边界案例，不升级为能力。
- Word 表格/图片写入的完整口径：书中未展开。

### 作者在书中警告的失败模式

- ce25 提取不可靠: 「PyPDF2 从PDF 提取文本时可能会出错，甚至根本不能打开某些PDF。遗憾的是，你对此没有什么办法」——下游当准确文本用会得到静默错误。
- ce26 加密未解密: 未 decrypt() 就读会报错；口令错误时 decrypt() 返回 0、getPage() 继续失败；decrypt() 只解密内存对象，「硬盘上的文件仍然是加密的」，下次运行仍需再解密。
- ce27 样式名带空格: 「包含空格将导致Word 误读样式名称，并且应用失败」——样式静默不生效。
- ce28 弱口令: 字典里的 88000 个大小写单词「只要几分钟时间」即可穷举完。

### 作者盲点与底本缺口

- **项目边界（unit_note）**: 13.6「暴力 PDF 口令破解」只保留为 ce28 边界案例，不升级为能力。
- **版本风险（NR）**: 本书示例为 2016 年版；同名库可能已改名或调整语义（PdfFileReader/PdfFileWriter 新版已改名），以源文为准记录，执行前按当前官方文档核对；不得把版本变化导致的失败计为能力缺陷。
- **书中未覆盖**: Word 表格/图片写入（f46）；提取图像/图表（书中自陈提取不出）。

### 容易混淆的邻近方法论

- `file-read-write`: 打开/关闭与 'rb'/'wb' 协议在其处；本卡依赖它。
- `organize-files`: 目录遍历/批量命名在其处；合并项目是其文档版应用。

---

## 相关能力（阶段 3 定稿）

- depends-on: `file-read-write`（'rb'/'wb' 协议与句柄管理）
- composes-with: `organize-files`（合并 PDF 是目录批处理的文档版）

---

## 审计信息

- **验证通过**: V1 ✓ / V2 ✓ / V3 ✓（阶段 1.5，单元 v12）
- **证据**: 候选 8 条（f45/f46/p65/c28/ce25/ce26/ce27/ce28）；ck-cb35325450c6 / ck-077137e0ee45 / ck-3df92fff5a27
- **去向**: 阶段 1.6 promoted 独立入口
- **蒸馏时间**: 2026-09-18（分片 B）
