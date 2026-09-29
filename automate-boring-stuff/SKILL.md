---
name: automate-boring-stuff
description: |
  当用户要把重复性的电脑操作（整理文件、批量处理表格、抓取网页数据、发邮件通知、填表单）交给 Python 程序自动完成时调用。覆盖《Python编程快速上手——让繁琐工作自动化》(Al Sweigart) 全书 18 章 + 附录：基础语法（表达式/控制流/函数/列表/字典/字符串）、正则、文件读写与批量组织、调试四件套、Web 抓取、Excel、PDF/Word、CSV/JSON、时间与调度、邮件短信、图像批处理、GUI 自动化，以及提问与项目迁移习惯。
  
  不适用于：需要工程化的项目（自动化测试/版本控制/团队协作）、对他人系统或网站的越权自动化、涉及专业资质的领域（法律/医疗/金融合规）。本书示例基于 2016 年版，第三方模块 API 可能已变化，执行前按当前官方文档核对。
metadata:
  cangjie.generated-by: cangjie-tools v2.5.0
  cangjie.variant: single
  cangjie.bundle-id: bundle.automate-boring-stuff
  cangjie.capability-count: 20
  cangjie.entrypoint-count: 1
---
# 《Python编程快速上手——让繁琐工作自动化》 — 全书能力入口

## 触发与不触发

**适用**：与本书能力域相关的咨询与任务（见下方路由表的意图列）。
**不适用**：
- 需要工程化的项目（自动化测试、版本控制、依赖锁定、团队协作）——本书明说写的是「用完即弃」的一次性脚本
- 对他人系统、公司系统或网站的越权自动化；抓取前先确认站点条款与 robots 约束
- 涉及专业资质判断的领域（法律、医疗、金融合规）

## 核心原则（常驻速览，概览类问题读到这里即可回答）

1. 每个自动化任务都是一条管道：输入来源（文件/网页/剪贴板/命令行）→ 处理（模块函数）→ 输出目的地；先定位管道两端，再选模块。
2. 专用模块（os/shutil/requests/openpyxl/Pillow/pyautogui…）把最难解析的环节压缩成少数函数调用——先学查文档、拼装现成函数，而不是从零写算法。
3. 危险性随能力上升：能删文件、发邮件、控制键鼠的程序必须先预演（注释掉真动作 + print 清单）、留 failsafe、让程序尽早失败。
4. 书里的关键 API 都先在交互式环境（>>>）验证再用；不确定的调用先做最小验证，再组装进项目。
5. 调试按错误性质选工具：外部输入错误用异常处理、内部假设用断言、观察中间量用日志——不要用 print() 硬扛。
6. 程序可以脱离「手动运行」：定时交给操作系统调度器、并发用线程、启动别的程序用 subprocess。
7. 卡住时的正确动作是「先搜索错误原文、再带齐七要素提问」，不是硬耗。

## 能力路由（先读本表，按意图加载 1 张能力卡）

| 用户意图 | 先读 | 补读/备注 |
|---|---|---|
| 写第一个可运行的小脚本（输入/计算/输出）；条件与循环写不对（elif 次序、无限循环、break/continue 位置）；搞不清变量名规则与类型转换（str+int 不能相加） | references/capabilities/language-basics.md | references/capabilities/functions-and-exceptions.md、references/capabilities/lists-and-references.md |
| 把重复代码收进函数、想复用；不理解局部/全局作用域，遇到 NameError/UnboundLocalError；想让用户输入出错时程序优雅报错而不是崩溃 | references/capabilities/functions-and-exceptions.md | references/capabilities/language-basics.md、references/capabilities/debugging-toolkit.md |
| 传列表进函数，结果原数据被改了；想复制列表却改到了原列表；下标/切片口径混淆（起含止不含、负下标） | references/capabilities/lists-and-references.md | references/capabilities/language-basics.md、references/capabilities/dicts-and-modeling.md |
| 用字典统计计数/分组（词频、按类别累计）；KeyError 崩溃、想安全取值；把现实结构（棋盘、成绩表）建成嵌套字典 | references/capabilities/dicts-and-modeling.md | references/capabilities/lists-and-references.md、references/capabilities/csv-and-json.md、references/capabilities/excel-spreadsheets.md |
| 清洗一段文本（去空白、改大小写、判前后缀）；把处理结果写回剪贴板；写口令保管箱一类的文本存取小工具 | references/capabilities/strings-and-clipboard.md | references/capabilities/regex-patterns.md、references/capabilities/file-read-write.md |
| 从文本里提取手机号/邮箱/日期等模式；批量替换/脱敏文本；正则写不出来、贪婪匹配结果不对 | references/capabilities/regex-patterns.md | references/capabilities/strings-and-clipboard.md、references/capabilities/file-read-write.md、references/capabilities/web-scraping.md、references/capabilities/plan-and-migrate.md |
| 读取文本文件内容、按行处理；把结果写到新文件、怕覆盖原文件；路径拼接与文件大小等 os.path 操作 | references/capabilities/file-read-write.md | references/capabilities/organize-files.md、references/capabilities/csv-and-json.md、references/capabilities/pdf-and-word.md、references/capabilities/strings-and-clipboard.md、references/capabilities/regex-patterns.md |
| 批量复制/移动/改名/归档文件；写备份脚本、打包 ZIP；删除文件（要能反悔） | references/capabilities/organize-files.md | references/capabilities/file-read-write.md、references/capabilities/plan-and-migrate.md、references/capabilities/pdf-and-word.md、references/capabilities/time-and-scheduling.md、references/capabilities/image-batch.md |
| 程序出错不知道怎么排查；要观察长循环里的中间值；不清楚断言/日志该用哪个、什么时候用 | references/capabilities/debugging-toolkit.md | references/capabilities/functions-and-exceptions.md、references/capabilities/gui-automation.md、references/capabilities/smart-questions.md、references/capabilities/environment-setup.md |
| 下载网页或文件保存到本地；从页面里提取标题/链接等元素；写自动查询/下载脚本 | references/capabilities/web-scraping.md | references/capabilities/csv-and-json.md、references/capabilities/regex-patterns.md、references/capabilities/gui-automation.md、references/capabilities/time-and-scheduling.md |
| 批量读取表格统计数据、筛选；批量写入/更新 Excel 单元格；把几千行手工统计变成脚本 | references/capabilities/excel-spreadsheets.md | references/capabilities/csv-and-json.md、references/capabilities/email-and-sms.md、references/capabilities/dicts-and-modeling.md |
| 合并/拆分 PDF、给 PDF 加水印页；读取 PDF 或 Word 内容；批量生成邀请函一类 Word 文档 | references/capabilities/pdf-and-word.md | references/capabilities/file-read-write.md、references/capabilities/organize-files.md |
| 读写 CSV 文件、删表头/改列；解析 JSON、从 API 取数据；CSV↔JSON 转换 | references/capabilities/csv-and-json.md | references/capabilities/dicts-and-modeling.md、references/capabilities/web-scraping.md、references/capabilities/file-read-write.md、references/capabilities/excel-spreadsheets.md |
| 让脚本每天定时自动运行；写计时/倒计时/秒表功能；并发下载、启动别的程序 | references/capabilities/time-and-scheduling.md | references/capabilities/web-scraping.md、references/capabilities/organize-files.md、references/capabilities/email-and-sms.md |
| 写脚本自动发提醒/通知邮件；批量给名单里的人发信；读收件箱、按条件搜索邮件 | references/capabilities/email-and-sms.md | references/capabilities/excel-spreadsheets.md、references/capabilities/time-and-scheduling.md |
| 批量缩放/裁剪图片、统一尺寸；给图片加水印/徽标；像素级读取或绘制 | references/capabilities/image-batch.md | references/capabilities/organize-files.md |
| 自动填网页表单/重复点击（没有接口可用）；脚本操作桌面软件；屏幕找图定位、截图 | references/capabilities/gui-automation.md | references/capabilities/debugging-toolkit.md、references/capabilities/web-scraping.md |
| pip 装不上、装完 import 还是报错；让脚本能从命令行直接运行；核对某个库的安装名与导入名 | references/capabilities/environment-setup.md | references/capabilities/debugging-toolkit.md |
| 报错卡住了，不知道怎么问/去哪问；想写一条能被快速解答的求助信息；学会自查错误信息 | references/capabilities/smart-questions.md | references/capabilities/debugging-toolkit.md |
| 要写一个复杂自动化程序，不知从哪下手；想给脚本排计划、拆步骤；把已有脚本迁移到新场景 | references/capabilities/plan-and-migrate.md | references/capabilities/organize-files.md、references/capabilities/regex-patterns.md |

**非能力类查询**：
- 书名/作者/章节/整书概览 → references/overview.md
- 术语解释 → references/glossary.md
- 决策规则速查（不需要原文依据时） → references/cheatsheet.md
- 完整意图与关键词索引（本表未覆盖的意图先查这里） → references/capability-index.md

## 加载规则

- 每次任务先读本文件，再按路由表加载 **1** 张能力卡；任务明确跨域时最多加载 2 张。
- 概览/书名类问题不加载能力卡，用「核心原则」与 overview.md 回答。
- 路由表与 capability-index.md 都无法命中的意图，明确告知超出本书范围，不要硬套。

## 边界与判停

- 用户要求对第三方网站/系统做明显违反其条款或法律的批量操作时，停止并说明边界。
- 脚本将执行不可逆的文件删除/覆盖而未经预演确认时，停止并先出示预演清单。
- 用户要求把口令/密钥直接写进源码，或把凭据粘贴进对话时，停止并改为外部传入（输入/环境变量）。
