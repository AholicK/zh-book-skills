<!-- capability_id: cap.automate-boring-stuff.web-scraping | revision: 1 | status: active -->
<!-- 来源: 单元 v10；归并候选 14 条（f37/f38/f39/f40/f41/f66/p60/p61/p62/c22/c23/c24/ce22/ce23）；14 条候选；promoted；B 段须写「选择器随站点变化，需现场复核」 -->

# 从 Web 抓取信息（requests 下载、BeautifulSoup 解析、selenium 控制浏览器）

## R — 原文 (Reading)

> 检查成功有一种简单的方法，就是在Response 对象上调用raise_for_status()方法。如果下载文件出错，这将抛出异常。如果下载成功，就什么也不做。
> …
> 总是在调用requests.get()之后再调用raise_for_status()。你希望确保下载确实成功，然后再让程序继续。
>
> — 第11章 · 从Web 抓取信息（11.2 用requests 模块从Web 下载文件）

> 在一个字符串中定位特定的一段HTML，这似乎很适合使用正则表达式。但是，我建议你不要这么做。
> …
> HTML 的格式可以有许多不同的方式，并且仍然被认为是有效的HTML，但尝试用正则表达式来捕捉所有这些可能的变化，将非常繁琐，并且容易出错。
>
> — 第11章 · 从Web 抓取信息（11.4.4，L7156-7158）

## I — 方法论骨架 (Interpretation)

流程固定：**先侦察、再下载、后解析、够不着才上浏览器**。

- 侦察（11.4.5）：开发者工具对目标元素右键 Inspect Element，读出 HTML 与 class/id，构造 CSS 选择器。
- 下载（11.2）：`requests.get(url)` 后**总是紧跟** `res.raise_for_status()`；失败即抛 HTTPError，不想崩溃就 try/except 包住该行。
- 保存（11.3）：`open(文件名,'wb')` + `for chunk in res.iter_content(100000):` 写入 + `close()`；纯文本也 'wb'。
- 解析（11.5）：`bs4.BeautifulSoup(res.text)` → `soup.select(选择器)` 返回 **Tag 列表**；`getText()`/`get('href')`/`attrs` 取值；空列表须分支处理。
- 升级（11.8）：需点击、填表、登录、等 JS 渲染时换 selenium（`webdriver.Firefox()` → `browser.get(url)` → `find_element_*` → `click()`/`send_keys()`）；能弄清 URL 就留在 requests+BS4。

明令：**不要用正则表达式解析 HTML**，改用 BeautifulSoup（p61/ce23）。

## A1 — 书中的应用 (Past Application)

### 案例 1: lucky.py——先看清结果页再自动化
- **类型**: 作者亲历＋书中项目（第11章）
- **问题**: 作者自述：习惯「通过鼠标中键点击搜索结果链接，或在点击时按住CTRL 键」开前几个结果，流程「变得很乏味」。
- **方法论的使用**: 先定结果页 URL（形如 https://www.google.com/search?q=…）；开发者工具找链接共有选择器（'.r a'）；get + raise_for_status 取页、select 选链接、webbrowser.open 打开前几个。
- **结论**: 把多步操作压成一条命令。
- **结果**: 未说明。

### 案例 2: mapIt.py——命令行/剪贴板双通道
- **类型**: 书中项目（第11章）
- **问题**: 「将一条街道的地址拷贝到剪贴板，并在Google 地图上打开它的地图，这是很繁琐的事」。
- **方法论的使用**: 地址来自命令行参数（`' '.join(sys.argv[1:])`）或剪贴板；`webbrowser.open('https://www.google.com/maps/place/' + address)`。
- **结论**: 表11-1 对比步骤数。
- **结果**: 未说明。

## A2 — 触发场景 (Future Trigger) ★

### 用户会在什么情境下需要这张卡?

1. 要把网页（或文件）下载到本地且失败不静默
2. 要从页面 HTML 提取标题/链接/价格等，需要写出选择器
3. 要写自动查询/下载脚本（打开前几个结果、定期抓某页）
4. 抓回来像 404 页面/解析返回空；或页面需要点击、填表、登录、等 JS 渲染

### 语言信号

- 「把这个网页的数据抓下来」「选择器怎么写 / 元素选不中」「下载总是失败 / 抓回来的是错误页面」
- 英文: "scrape this page", "which selector", "download fails", "save this page"

### 与相邻能力的区分（阶段 3 定稿）

- 与 `regex-patterns` 的区别（contrasts-with）: 正则只用于**解析后的纯文本字段**；HTML 解析必须用 BeautifulSoup（作者明令）。
- 与 `csv-and-json` 的区别（composes-with）: 数据源是 API/JSON 时走那条卡；本卡处理 HTML。
- 与 `time-and-scheduling` 的区别（composes-with）: 定时/多线程下载（15.7）走那条卡。
- 与 `gui-automation` 的区别（contrasts-with）: 能走接口/选择器就走受控路径；pyautogui 是最后手段。

## E — 可执行步骤 (Execution)

1. **侦察**——完成标准: 拿到 URL；用开发者工具读出目标元素 HTML 并构造能选中它的选择器。
2. **下载（硬规则一）**——完成标准: `requests.get(url)` 后**紧跟** `res.raise_for_status()`；需容错时 try/except 包住该行。
3. **保存（可选，硬规则二）**——完成标准: `open(文件名,'wb')` → `iter_content(100000)` 分块写入 → `close()`；纯文本也用 'wb'。
4. **解析与提取**——完成标准: `bs4.BeautifulSoup(res.text)` → `soup.select(选择器)` → `getText()`/`get()`/`attrs`；空列表须分支处理。
5. **需要交互/渲染时上 selenium**——完成标准: `webdriver.Firefox()` → `browser.get(url)` → `find_element_*` → `click()`/`send_keys()`；找不到元素抛 NoSuchElement。

**输入契约**: 必需＝①目标 URL（或从哪个页面取什么）②要提取的元素形态（定选择器）③是否需要登录/点击/JS（决定 BS4 还是 selenium）；缺③先问，缺②先侦察。抓取前先确认站点条款。
**输出契约**: 交付＝①脚本（下载→检查→（保存）→解析→提取）②选择器及来源 ③失败处理（状态检查/try-except/空列表）；完成标准：能回指 f37–f41 / f66 / p60–p62。

## B — 边界 (Boundary) ★

### 不要在以下情况使用这张卡

- 数据源是 API/JSON → 走 `csv-and-json`。
- 页面需 JS 渲染/复杂登录而只想靠 BS4——先评估 selenium；现代反爬书中未覆盖。
- 只解析纯文本字段（非 HTML 结构）→ 可以用 `regex-patterns`。

### 作者在书中警告的失败模式

- ce22 不检查错误的两面: 不调 raise_for_status() 时 404/500 页面被当正常内容，错误推迟到「结果是垃圾」才暴露；selenium 侧相反——无匹配元素即抛 NoSuchElement。
- ce23 用正则解析 HTML: 「我建议你不要这么做」——等价合法写法抓不住，症状是「昨天能跑，今天网页改版就崩/抓空」。

### 作者盲点与底本缺口

- **选择器随站点变化（unit_note）**: 现代网页（动态、反爬）与 2015 年示例差异大——选择器/接口须现场用开发者工具复核，示例站点不能照抄。
- **未讲显式等待**: selenium（f66/p62）未讲 WebDriverWait；超时/重试与 robots/合规（f37）、Google 地图变化（f41）均未覆盖。
- **版本风险（NR）**: 本书示例为 2016 年版；同名库可能已改名或调整语义，执行前按当前官方文档核对；不得把版本变化导致的失败计为能力缺陷。

### 容易混淆的邻近方法论

- `regex-patterns`: 只用于解析后的纯文本字段；`csv-and-json`: JSON/API 走它；`time-and-scheduling`: 定时/批量在其处；`gui-automation`: 受控路径优先。

---

## 相关能力（阶段 3 定稿）

- contrasts-with: `regex-patterns`（不要用正则解析 HTML）、`gui-automation`（受控路径优先）
- composes-with: `csv-and-json`（requests + json = Web API 链路）、`time-and-scheduling`（多线程下载，15.7）

---

## 审计信息

- **验证通过**: V1 ✓ / V2 ✓ / V3 ✓（阶段 1.5，单元 v10）
- **证据**: 候选 14 条（f37/f38/f39/f40/f41/f66/p60/p61/p62/c22/c23/c24/ce22/ce23）；ck-92e20a1cc5e7 / ck-3b7851cc76f5 / ck-cb6c95e93e0c
- **去向**: 阶段 1.6 promoted 独立入口
- **蒸馏时间**: 2026-09-18（分片 B）
