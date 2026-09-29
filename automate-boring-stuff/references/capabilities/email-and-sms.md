<!-- capability_id: cap.automate-boring-stuff.email-and-sms | revision: 1 | status: active -->
<!-- 来源: 单元 v15；归并候选 10 条（f54/f55/f56/p69/c35/c36/c37/c46/ce32/ce33）；10 条候选；promoted；含 ce32（口令硬编码）与 ce33（IMAP 限制） -->

# 电子邮件与短信通知（SMTP 发信六步、IMAP 收信与搜索、Twilio 短信）

## R — 原文 (Reading)

> 得到SMTP 对象后，调用它的名字古怪的EHLO()方法，向SMTP 电子邮件服务器“打招呼”。
> ……
> 如果要连接到SMTP 服务器的587 端口（即使用TLS 加密），接下来需要调用 starttls()方法。
> ……
> 电子邮件正文字符串必须以'Subject: \n'开头，作为电子邮件的主题行。
>
> — 第16章 · 发送电子邮件和短信「16.2 发送电子邮件」L10442-10566

> 不要在IDLE 中输入这个例子，因为smtp.example.com、bob@example.com、MY_ SECRET_PASSWORD 和alice@example.com 只是占位符。
> ……
> 要记住，永远不要直接在代码中写入密码！应该让程序从input()接受输入的密码。
>
> — 第16章 · 发送电子邮件和短信「16.2 注意事项」L10448-10530

> 如果你的搜索匹配大量的电子邮件，Python 可能抛出异常imaplib.error: got more than 10000 bytes。如果发生这种情况，必须断开并重连IMAP 服务器，然后再试。
>
> — 第16章 · 发送电子邮件和短信「16.4.6 大小限制 / 16.4.10 删除电子邮件」L10780-10856

## I — 方法论骨架 (Interpretation)

这张卡给自动化装上「消息出口」：SMTP 发信、IMAP 收信，外加 Twilio 短信（外部付费依赖）。

发信是顺序固定的流水线：`smtplib.SMTP(域名, 587)` → `ehlo()`（拿到对象后第一件事）→ 587 端口 `starttls()` → `login(地址, 口令)` → `sendmail(发件人, 收件人, 正文)` → `quit()`。两个易错点：正文必须以 `'Subject: \n'` 开头（\n 分隔主题与正文）；sendmail() 的返回值是字典——空字典才是全部成功，失败收件人各占一个键值对。凭据纪律（硬边界）：口令不写进源码，走 input() 或命令行参数。

收信走 IMAP 客户端（imapclient + pyzmail）：连接 → login → `select_folder('INBOX', readonly=True)` → `search(搜索键列表)` 拿 UID → `fetch(uid, ['BODY[]'])` 取内容 → 删除走 `delete_messages()` + `expunge()` → logout()。search() 只给 UID 不给内容；多个搜索键是与关系，要或必须用 OR 键；日期写 05-Jul-2015 格式。

## A1 — 书中的应用 (Past Application)

### 案例 1: 向会员发送会费提醒电子邮件（sendDuesReminders.py）
- **类型**: 书中项目（第16章）
- **问题**: 每月要维护交费表并邮件提醒未交会员
- **方法论的使用**: openpyxl 打开 duesRecords.xlsx 取最近月份列 → 未记 paid 的会员存进 unpaidMembers 字典 → SMTP()/ehlo()/starttls()/login()（口令走 sys.argv[1]）→ 逐个 sendmail() 定制正文 → quit()
- **结论/结果**: 实测输出 Sending email to alice@example.com... 等；收件人收到图16-3 所示提醒邮件

### 案例 2: 「只给我发短信」模块（textMyself.py）
- **类型**: 书中项目（第16章）
- **问题/使用**: 长任务跑完不想回电脑旁检查；把 Twilio 凭据与 textmyself(message) 封装成模块，其他程序 import 加一行调用
- **结论/结果**: 「注册Twilio 和编写短信代码只要做一次」；未给发送结果截图

## A2 — 触发场景 (Future Trigger) ★

### 用户会在什么情境下需要这张卡?

1. 长任务跑完要通知自己（邮件或短信）
2. 按名单批量发定制邮件（催办、提醒）
3. 要读收件箱、按条件搜邮件（关键字/日期）
4. 要把结果通知接进无人值守流水线

### 语言信号

- 「跑完给我发个提醒」「发个邮件通知」「批量给这些人发邮件」「读一下收件箱里某类邮件」
- 英文: "send reminder email", "batch emails", "search inbox", "send an SMS"

### 与相邻能力的区分（阶段 3 定稿）

- 与 `excel-spreadsheets` 的区别: 会费提醒流水线的「名单来源」由那条卡负责（读表筛人）；本卡只负责「消息出口」
- 与 `time-and-scheduling` 的区别: 本卡负责消息出口；「何时触发」由那条卡；两者串成「到点自动提醒」

## E — 可执行步骤 (Execution)

1. **确定出口与凭据来源**
   - 完成标准: 选邮件（SMTP）或短信（Twilio）；凭据齐备（地址、口令/授权码、SMTP/IMAP 域名；Twilio 需 SID/认证标识号/号码）；口令走 input() 或 sys.argv，不写进源码（ce32）
   - 判停条件: 凭据不齐 → 先在脚本外解决，本卡只在「凭据齐备」前提下执行
2. **发信按六步写**
   - 完成标准: SMTP 对象 → ehlo() → starttls()（587）→ login() → sendmail(from, to, 正文) → quit()；正文以 'Subject: \n' 开头；发送后检查返回字典，非空则列出失败收件人
   - 判停条件: 465 端口/SSL 改用 SMTP_SSL 并跳过 starttls
3. **收信（如涉及）**
   - 完成标准: IMAPClient(ssl=True) → login → select_folder(readonly=True) → search(键列表) → fetch(uid, ['BODY[]'])；日期写 05-Jul-2015 格式
   - 判停条件: 宽泛搜索前把 imaplib._MAXLINE 调大，否则可能报 got more than 10000 bytes（ce33）
4. **删除（仅在明确要求时）**
   - 完成标准: delete_messages(uids) + expunge()；select_folder 需 readonly=False；先确认删除范围（不可恢复）

**输入契约**: 必需 = ①出口类型；②收件人清单或搜索条件；③凭据来源；④（收信时）文件夹与搜索键。缺 ③ 先问。

**输出契约**: 可运行脚本（六步序列 + 结果检查 + 发送进度打印）+ 凭据处理说明；调用可回指 f54/f55/p69 等候选。

## B — 边界 (Boundary) ★

### 不要在以下情况使用这张卡

- 凭据不齐或只能硬编码口令——先解决凭据来源
- 需要附件、HTML 邮件等高级特性，或站外推送渠道（均不在书内）
- 短信依赖 Twilio 付费账号（外部依赖），账号未注册时不要写这一步

### 作者在书中警告的失败模式

- 口令硬编码（ce32）：「如果有人复制了你的程序，他们就能访问你的电子邮件账户！」；占位符示例不要直接运行
- IMAP 搜索过大（ce33）：宽泛搜索抛 imaplib.error: got more than 10000 bytes，必须断开重连再试
- 删除两段式（ce33）：delete_messages() 只打 \Deleted 标志，expunge() 才永久删除；漏 readonly=False 也删不了
- sendmail() 返回非空字典却当成功——要检查返回字典

### 作者盲点与底本缺口

- **版本风险（nr_warnings）**：本书示例为 2016 年版；现代版本的同名库可能已改名或调整语义，执行前按当前官方文档核对；以源文写法为准记录，不静默改写为现代 API；阶段 4 不得把版本变化导致的失败计为能力缺陷
- **外部依赖（unit_note）**：Twilio 需注册付费账号；Gmail 等现代提供商常要求应用专用密码，书中未覆盖
- 底本为 OCR 稿：附录 C 引文把 smtpObj 误写作 smptObj [原文如此]

### 容易混淆的邻近方法论

- `time-and-scheduling` / `excel-spreadsheets`: 何时触发 vs 消息出口；名单来源 vs 发送动作

---

## 相关能力（阶段 3 定稿）

- composes-with: `excel-spreadsheets`（会费提醒：Excel 名单 + 逐个 sendmail 的流水线）
- composes-with: `time-and-scheduling`（定时 + 通知 = 到点提醒）

---

## 审计信息

- **验证通过**: V1 ✓ / V2 ✓ / V3 ✓（阶段 1.5，单元 v15）
- **证据**: 候选 10 条（f54/f55/f56/p69/c35/c36/c37/c46/ce32/ce33）；ck-7d492c0133cd / ck-d28b4c63c888 / ck-35bdb36c32a7
- **去向**: 阶段 1.6 promoted 独立入口（排序第 6）
- **蒸馏时间**: 2026-09-18（分片 B）
