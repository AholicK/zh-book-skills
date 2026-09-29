# 决策规则速查 — 《Python编程快速上手——让繁琐工作自动化》

| 能力 | 一句话规则 |
|---|---|
| 表达式、数据类型与程序结构（含交互式环境、输入输出、条件与循环、模块导入） | 把任务拆成「表达式 + 变量 + 条件 + 循环 + 模块调用」五件套，是最小可运行脚本的写法。 |
| 函数、作用域与异常处理（def/return、局部全局作用域、try/except） | 把一段逻辑封成函数、把可预期的错误放进 try——后续所有脚本的组织基元。 |
| 列表、元组与引用语义（下标/切片/方法、可变与不可变、copy 与 deepcopy） | 列表存的是「引用」：改副本却动了原数据，十有八九是它。 |
| 字典与结构化数据建模（键值访问、get/setdefault、pprint、用嵌套结构建模） | 用字典把「真实世界的东西」变成可编程对象——键值访问前先想 get/setdefault。 |
| 字符串处理与剪贴板自动化（字符串方法、转义与原始字符串、pyperclip 三段式） | 字符串方法都不改原串——必须写回；剪贴板操作走 paste → 处理 → copy 三段式。 |
| 正则表达式文本模式处理（构造、分组、量词、字符分类、findall/sub、VERBOSE） | 先写笨版本定位问题，再用正则压缩——模式构造四步：compile → search/findall → 取分组。 |
| 文件读写与路径操作（open/read/write/close、路径与 os.path、shelve、pprint 落盘） | open → 读/写 → close 三步走；'w' 会立刻清空同名文件，相对路径锚在「当前工作目录」。 |
| 批量组织文件（shutil 复制/移动、send2trash 安全删除、os.walk 遍历、zipfile 压缩、危险操作先验证） | 危险操作分两步：先注释掉真动作 + print() 预演清单，核对无误再放开执行。 |
| 调试工具与缺陷排查（raise/断言/日志/调试器四件套及其选择判据） | 四件套按错误性质选：外部输入错误→异常；内部假设→断言；观察中间量→日志；不要用 print。 |
| 从 Web 抓取信息（requests 下载、BeautifulSoup 解析、selenium 控制浏览器、webbrowser） | 先 raise_for_status() 检查下载结果，再用 BeautifulSoup 的 select() 取元素——不要用正则解析 HTML。 |
| Excel 电子表格处理（openpyxl 读写、单元格/行列、公式与样式、批处理骨架） | 读到内存改完必须 save()，且永远另存新文件——不要写回原表。 |
| PDF 与 Word 文档处理（PyPDF2 读写合并加解密、python-docx 读样式与写文档） | PDF 合并五步（'rb' 读入 → getPage → addPage → write）；Word 用 python-docx 按段落读、按样式写。 |
| CSV 与 JSON 数据处理（csv 模块读写、json 的 loads/dumps、从 API 取数） | CSV 走 csv 模块（别用 split(',')）；JSON 走 json.loads/dumps——API 返回的就是它。 |
| 时间、计划任务与启动程序（time/datetime/timedelta、多线程、subprocess 与调度器） | 定时交给系统调度器（cron/任务计划），不要在脚本里写时钟循环；并发用 threading 三步。 |
| 电子邮件与短信通知（SMTP 发信六步、IMAP 收信与搜索、Twilio 短信） | SMTP 六步顺序不能乱（连接→ehlo→starttls→login→sendmail→quit）；口令一律从外部传入。 |
| 图像批处理（Pillow 打开/裁剪/粘贴/缩放/旋转/逐像素、ImageDraw 绘制） | 图像批处理 = 打开 → 变换（resize/crop/paste）→ 另存；坐标是左上原点、y 向下。 |
| GUI 自动化（pyautogui 鼠标/键盘/截图/图像识别，及三道安全机制） | 最后手段：没有 API 可用时才模拟键鼠——必须保留 PAUSE/FAILSAFE 和校验，让它「点错就停」。 |
| 环境搭建与脚本运行（pip 安装第三方模块、#! 行、批处理与 PATH、-O 选项） | 装模块用 pip（注意导入名≠安装名），运行脚本用 #! 行或批处理——先验证 import 再写代码。 |
| 编程求助与提问（错误信息自查、聪明提问清单、求助礼节） | 卡住先搜索错误原文；提问要带：目的、完整错误、已尝试、版本环境。 |
| 开发方法与迁移习惯（先草拟计划、先笨后巧、危险预演、类似程序迁移） | 开工先列「程序要做的事」再翻译成「代码要做的事」；一次只加一个功能（先笨后巧）。 |

> 速查只给结论；需要原文依据、案例或反例时读对应能力卡。
