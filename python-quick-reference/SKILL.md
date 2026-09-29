---
name: python-quick-reference
description: |
  当用户写或读 Python 入门级语法与标准库用法、看不懂报错信息、或需要一份「写法 + 参数口径」速查时调用本能力包：分支与循环、序列切片、列表与元组、字典与集合、字符串拆合与格式化、运算符优先级、输入输出、函数与 lambda、类与对象、异常处理、模块与 pip、文件与目录、数据库访问、进程与线程、Socket、正则、wxPython 窗口构造。
  覆盖《Python背记手册》（OCR 整理稿，源文未署名）全部 20 个能力单元，每张卡都注明底本损毁位置与「本书未覆盖」的边界。
  不适用于：现代工程实践（虚拟环境、依赖锁定、类型注解、with 上下文管理器、测试与调试器）；第三方库的完整开发教程；以及需要底本缺失部分的场合（数据类型转换表、socket 创建语句、re 模块调用函数、wx.App 与主循环、open() 签名行）。
metadata:
  cangjie.generated-by: cangjie-tools v2.5.0
  cangjie.variant: single
  cangjie.bundle-id: bundle.python-quick-reference
  cangjie.capability-count: 20
  cangjie.entrypoint-count: 1
---
# 《Python背记手册》 — 全书能力入口

## 触发与不触发

**适用**：与本书能力域相关的咨询与任务（见下方路由表的意图列）。
**不适用**：
- 非语法类问题：程序架构设计、性能调优、工程规范、代码审查
- 现代工程实践（本书未覆盖）：虚拟环境、requirements/依赖锁定、类型注解、with 上下文管理器、调试器与测试框架、异常链与自定义异常
- 第三方库的完整开发教程（wxPython / Pygame / Flask / 爬虫框架在书中只有名称与用途对照表，没有可运行程序）
- 底本缺装配图的部分：数据类型转换函数表（NR-02）、open() 签名行（NR-09）、connect() 调用示例（NR-10）、Thread/Process 的 start/join（NR-11）、socket 创建与 import（NR-12）、re 调用函数（NR-13）、wx.App 与主循环（NR-14）

## 核心原则（常驻速览，概览类问题读到这里即可回答）

1. 本书的知识按「语法形式 + 参数逐项说明 + 示例结果」三件套组织：记住形式与每一项参数的默认值口径，就能写出正确写法。
2. 分支与循环只有两种驱动方式——条件为真就执行、执行完回判（while），或从对象里逐个取值（for）；break 跳出循环、continue 跳过本轮，else 不能单独出现。
3. 列表、元组、字符串共用同一套切片三参数口径：含起点、不含终点，且切片生成的是新序列（原序列不动）。
4. 要排序先问一句「原顺序还要不要用」：还要用就 sorted（不动原列表），不要用才 sort（就地改）。
5. 出错后的动作分三段：try 放可能出现错误的代码、except 捕获指定异常并处理、finally 放任何情形都必须执行的代码；raise 用于「本层不处理」，assert 用于钉住自己以为必然成立的前提。
6. 文件与数据库都是「先取对象、再调方法」的两层结构：open 得到文件对象；connect 得到连接对象、再由 cursor() 得到游标对象——SQL 要发给游标，改完要 commit。
7. 底本在 OCR 中受损（表格压成单列、圆括号变字母 O、花括号变圆括号）：引用一律照抄原稿，不凭常识回填任何损毁条目。

## 能力路由（先读本表，按意图加载 1 张能力卡）

| 用户意图 | 先读 | 补读/备注 |
|---|---|---|
| 写「满足条件才执行」的分支逻辑；在 while 与 for 之间选型；循环提前退出或跳过某个元素（break / continue）；else 或保留字报语法错怎么改；判断一段循环代码为什么跑不起来；让程序反复询问或重复执行，直到条件不再满足（while 的典型场景） | references/capabilities/control-flow-and-loops.md | references/capabilities/operator-precedence.md、references/capabilities/input-and-print.md、references/capabilities/list-comprehension.md、references/capabilities/exception-handling.md |
| 取出序列中的一段（前几位、中间一段、每隔几个取一个）；截取字符串的某一段；切片结果少一个或多一个，要核对边界；确认切片会不会改掉原来的列表/字符串；读懂别人写的 x[1:5:2] 取的是哪几个元素 | references/capabilities/sequence-slicing.md | references/capabilities/list-and-tuple-operations.md、references/capabilities/string-encoding-split-join.md |
| 统计某元素出现几次、或它在第几位；把另一个列表的元素并进当前列表；带序号遍历列表；按范围生成列表或元组、建空列表；求和之后还要再加一个基数 | references/capabilities/list-and-tuple-operations.md | references/capabilities/sequence-slicing.md、references/capabilities/sort-vs-sorted.md、references/capabilities/list-comprehension.md、references/capabilities/dict-and-set-operations.md |
| 把列表排成升序或降序；排序后还要保留原来的顺序；就地排序、不产生新对象；按元素的某个键排序（如不区分大小写）；判断一段代码该用 sort 还是 sorted | references/capabilities/sort-vs-sorted.md | references/capabilities/list-and-tuple-operations.md、references/capabilities/function-definition-and-call.md |
| 一行生成新列表；把列表逐项映射成新列表；从列表里筛出符合条件的元素；把看不懂的推导式还原成循环 | references/capabilities/list-comprehension.md | references/capabilities/list-and-tuple-operations.md、references/capabilities/control-flow-and-loops.md |
| 创建字典并按键查表；把数据去重成集合；遍历字典拿到键值对；判断键或集合元素能不能用某个类型；选 del/pop/remove/clear 删除集合内容 | references/capabilities/dict-and-set-operations.md | references/capabilities/list-and-tuple-operations.md |
| 解决乱码与编码不一致；按分隔符把字符串拆成字段；把多个字符串合并成一个；让编解码对非法字符容错 | references/capabilities/string-encoding-split-join.md | references/capabilities/sequence-slicing.md、references/capabilities/file-and-directory-operations.md、references/capabilities/string-formatting.md、references/capabilities/regex-construction.md |
| 把变量排进一句话输出；控制小数位数；输出百分比写法；输出二进制/十六进制表示；在 % 与 format() 之间选一个 | references/capabilities/string-formatting.md | references/capabilities/string-encoding-split-join.md、references/capabilities/input-and-print.md |
| 判断一条表达式的求值顺序与结果；把复合赋值（+= 等）改写成展开形式；确认比较 / 逻辑表达式的结果是 True 还是 False；判断条件表达式里哪些部分先算、要不要加括号 | references/capabilities/operator-precedence.md | references/capabilities/control-flow-and-loops.md |
| 让程序接收用户的键盘输入；给输入写一句提示文字；用 print 输出变量或表达式的结果；用 input / print 搭出与用户对话的最小骨架（骨架里的条件与循环见 control-flow-and-loops） | references/capabilities/input-and-print.md | references/capabilities/control-flow-and-loops.md、references/capabilities/string-formatting.md |
| 把重复的代码封装成函数；定义带默认值的参数；让函数返回一个值或多个值；用 lambda 写一个临时小函数；在需要函数对象处传一个函数（如排序的 key 参数；并发场景的 target 填写见 process-and-thread） | references/capabilities/function-definition-and-call.md | references/capabilities/sort-vs-sorted.md、references/capabilities/exception-handling.md、references/capabilities/class-and-objects.md、references/capabilities/process-and-thread.md |
| 定义一个类并创建实例；写实例方法并处理 self 的位置；用 @property 做一个计算属性；用继承复用已有类并重写方法 | references/capabilities/class-and-objects.md | references/capabilities/function-definition-and-call.md、references/capabilities/gui-window-construction.md |
| 判断某个功能是否属于标准库、要不要装包；写 import / from…import 语句（含别名与 `*` 通配）；安装、查看、卸载第三方模块；排查 ImportError / ModuleNotFoundError | references/capabilities/module-import-and-pip.md | references/capabilities/database-access.md、references/capabilities/socket-communication.md |
| 程序报错就崩，想改成可控处理；写 try…except…finally 三段式并确定各块放什么；本层不处理，把异常上抛给调用方；用 assert 暴露「程序以为必然成立」的假设；按异常名（NameError / KeyError / ZeroDivisionError 等）定位错误 | references/capabilities/exception-handling.md | references/capabilities/function-definition-and-call.md、references/capabilities/file-and-directory-operations.md、references/capabilities/control-flow-and-loops.md |
| 把内容写进文件、或从文件读出来；打开文件后写不进去（默认只读导致）；删除文件或给文件改名；查看文件大小、最后修改时间等元数据；处理同目录与完整路径的写法差别 | references/capabilities/file-and-directory-operations.md | references/capabilities/exception-handling.md、references/capabilities/string-encoding-split-join.md |
| 把数据存进数据库或从数据库查出来；分清连接对象与游标对象各做什么；改数据后不生效，怀疑漏了提交；把查询结果取回来（下一条 / 指定数量 / 全部）；确认 connect() 有哪些参数、按数据库类型确定驱动模块名（驱动的安装与 import 见 module-import-and-pip） | references/capabilities/database-access.md | references/capabilities/module-import-and-pip.md |
| 用 Process 或 Thread 起一个任务（写调用与参数，含 target / args / kwargs 各填什么）；确认进程与线程的构造参数是否一致；看懂 mutex.acquire()/release() 在做什么；判断 acquire(blocking=False) 拿不到锁时会怎样；保护一段被多个执行体读写的共享数据 | references/capabilities/process-and-thread.md | references/capabilities/function-definition-and-call.md |
| 写出 TCP 服务端从绑定到收数据的调用顺序；确认 listen 的 backlog 该填几；解释 accept 为什么停住不动（阻塞式等待）；判断客户端连接失败（socket.error）在哪一步检查；查 recv/send/sendall/recvfrom/sendto 的用途 | references/capabilities/socket-communication.md | references/capabilities/module-import-and-pip.md |
| 构造一条正则模式串；查元字符的含义（点、反斜杠 w/s/d/b、^、$）；为某个位置选择重复次数限定符；选择忽略大小写/多行/跨行等标志；判断 ^ 与 $ 是按整串还是按行匹配 | references/capabilities/regex-construction.md | references/capabilities/string-encoding-split-join.md |
| 读懂或改写 wxPython 窗口与控件构造；查 wx.Frame 的参数与可省略项；给输入框设置只读/密码等 style；为控件布局选择 sizer 类别；确认顶级窗口与控件的 parent 分别传什么 | references/capabilities/gui-window-construction.md | references/capabilities/class-and-objects.md |

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

- 问题需要底本未给出的「装配图」时（见 out_of_scope 末条），停止推演并明确说明「本书未覆盖」，不得凭常识补全或编造成书里的写法。
- 需要引用数据类型转换函数表的映射（NR-02，`ord()` 行函数名被下一行顶掉）时，停止引用该表并说明底本损毁位置。
- 用户要做印刷品、教学材料等逐字引用时，提醒底本为 OCR 整理稿（NR-15：符号误识、表格压单列、跨行串接），应先换更好底本并重跑引文核验。
