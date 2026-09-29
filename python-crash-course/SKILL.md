---
name: python-crash-course
description: |
  当用户用 Python 写脚本、做练习或排查报错时调用：覆盖《Python编程从入门到实践》的 24 张能力卡——环境搭建与运行排障、变量/字符串/列表/字典等语言基础、条件与循环、函数与类、文件与异常、单元测试、Pygame 游戏项目、数据可视化与 API 取数、Django 建站、Git 与调试求助。
  
  每个请求先读路由表，加载 1 张最匹配的能力卡执行；卡内含原文依据、书内案例、执行步骤与失败边界。
  
  不适用于：非 Python 的语言问题；算法/数据结构理论；书中未覆盖的进阶特性（装饰器、生成器、异步、类型注解等）与当前版本的生产实践——本书为 2016 年技术栈快照（Python 3.5 / Pygame 1.9.2a0 / Django 1.8.4），涉及安装、依赖与平台流程处须按现环境替换。
metadata:
  cangjie.generated-by: cangjie-tools v2.5.0
  cangjie.variant: single
  cangjie.bundle-id: bundle.python-crash-course
  cangjie.capability-count: 24
  cangjie.entrypoint-count: 1
---
# 《Python编程从入门到实践》 — 全书能力入口

## 触发与不触发

**适用**：与本书能力域相关的咨询与任务（见下方路由表的意图列）。
**不适用**：
- 非 Python 语言（JavaScript、Java 等）的问题
- 算法与数据结构理论、计算机科学课程内容
- 书中未覆盖的进阶特性：装饰器、生成器、异步、类型注解、并发等
- 除 unittest 之外的测试框架（pytest 等）与 CI/CD、容器化等工程化主题
- 书中版本快照之外的生产实践（Django 4.x、现代部署平台）

## 核心原则（常驻速览，概览类问题读到这里即可回答）

1. 环境先行：把安装、编辑器配置与运行排障当独立技能处理；环境不通先解决环境，再谈代码。
2. 语言核心是一条递进链：字符串/数字 → 列表 → 条件 → 字典 → while/输入 → 函数 → 类 → 文件与异常 → 测试，每一步建立在前一步的数据结构上。
3. 报错即教学：先按类型定位（NameError / SyntaxError / IndexError / TypeError / 缩进类），术语与修复口径以书内定义为准。
4. 规范与可读性优先：PEP 8、文档字符串、命名约定、单一职责函数——代码被阅读的次数比编写的次数多。
5. 重构与模块化是必然动作：先让最简版跑通，再加功能；加新代码前先重构。
6. 2016 快照纪律：凡涉及版本号、安装命令、第三方平台（Pygame 源安装、Heroku、Weather Underground）的内容，一律标注为历史快照并按现环境替换，不得包装成「照做即可」。

## 能力路由（先读本表，按意图加载 1 张能力卡）

| 用户意图 | 先读 | 补读/备注 |
|---|---|---|
| 在新机器上装好并跑通第一个 Python 程序；判断用 python、python3 还是完整路径；程序没有输出时区分环境问题与输入错误；按七步路径逐级排查装不上、跑不起来；组织一份能获得有效帮助的求助信息 | references/capabilities/env-setup-and-troubleshooting.md | references/capabilities/install-and-editor-reference.md、references/capabilities/debugging-and-help-seeking.md |
| 看懂 traceback 并判断报错属于哪一类；修 NameError：核对赋值与拼写一致性；修 TypeError：补 str() 显式转换；清理字符串大小写与空白后再存储；给变量起名并决定是否写注释 | references/capabilities/variables-strings-and-numbers.md | references/capabilities/loops-slices-and-formatting.md、references/capabilities/input-and-while-loops.md、references/capabilities/install-and-editor-reference.md |
| 选对增删方式：del、pop、remove；排序取舍：sort 永久、sorted 临时；IndexError 排查：索引减 1 核对；用空列表 append 动态建表；取最后一个元素并处理空列表边界 | references/capabilities/list-operations.md | references/capabilities/loops-slices-and-formatting.md、references/capabilities/dicts-and-nesting.md |
| 对列表每个元素做同一件事的循环写法；给列表做一份互不影响的副本；核对 range 与切片的边界（差一）；修复缩进/冒号类报错与静默逻辑错误；提交前按格式规范自检代码 | references/capabilities/loops-slices-and-formatting.md | references/capabilities/list-operations.md、references/capabilities/variables-strings-and-numbers.md、references/capabilities/data-visualization.md、references/capabilities/dicts-and-nesting.md |
| 按档位/区间给一个值分流处理；多个条件同时成立时全部处理；判断某值是否在列表中（in / not in）；空列表与大小写带来的误判体检；else 兜底分支的安全改写 | references/capabilities/conditionals-and-if.md | references/capabilities/input-and-while-loops.md、references/capabilities/dicts-and-nesting.md |
| 用「键→值」给多属性数据建模；选择 items/keys/values 遍历方式；顺序不符预期时显式排序输出；多个同类对象或一键多值的数据组织；改值/删键前的安全核对 | references/capabilities/dicts-and-nesting.md | references/capabilities/list-operations.md、references/capabilities/data-parsing-and-cleaning.md |
| 写一个反复询问、能随时退出的交互程序；修掉停不下来、无限刷屏的 while 循环；让 input 拿到的数字先转 int 再比较计算；在循环里删除或搬移列表元素、填充字典 | references/capabilities/input-and-while-loops.md | references/capabilities/conditionals-and-if.md、references/capabilities/functions-and-modules.md、references/capabilities/variables-strings-and-numbers.md |
| 把重复出现的代码抽成一写多用的函数；传参错位或缺参报错时定位并修复调用；让函数用 return 交回结果而不是直接打印；把函数拆进独立模块并正确导入；传列表进函数前决定要不要用副本 | references/capabilities/functions-and-modules.md | references/capabilities/classes-and-inheritance.md、references/capabilities/files-and-exceptions.md、references/capabilities/unit-testing.md、references/capabilities/input-and-while-loops.md |
| 把数据与操作绑成类、按类创建实例；搭好继承链并正确初始化父类部分；让子类重写的方法真正生效；类太长时拆成小类、用实例作属性；把类拆进模块并接好导入 | references/capabilities/classes-and-inheritance.md | references/capabilities/functions-and-modules.md、references/capabilities/unit-testing.md、references/capabilities/pygame-project-setup.md |
| 把程序运行结果保存到文件、下次运行再读回来；读文件报「找不到文件」，文件放哪、路径怎么写；往已有文件追加内容，又怕清空原有数据；程序遇到坏输入或缺文件别崩溃，给出提示后继续跑；把又长又乱的脚本拆成单一职责的函数 | references/capabilities/files-and-exceptions.md | references/capabilities/functions-and-modules.md、references/capabilities/unit-testing.md、references/capabilities/data-parsing-and-cleaning.md |
| 为函数或类写一套 unittest 测试；测试没通过，判断该修代码还是改测试；测试输出一堆 E 和 F，想读懂并定位失败；多个测试重复准备数据，用 setUp 收拢；不确定要写多少测试，想先覆盖重要行为 | references/capabilities/unit-testing.md | references/capabilities/functions-and-modules.md、references/capabilities/classes-and-inheritance.md、references/capabilities/files-and-exceptions.md |
| 从零开始做一个 2D 游戏，先搭好可扩展骨架；把游戏参数集中到一处，改外观不用翻文件；项目文件越拆越多，import 自己写的模块失败；游戏越跑越卡，排查主循环里的对象创建；加新功能之前，先把既有代码重构好 | references/capabilities/pygame-project-setup.md | references/capabilities/pygame-control-and-bullets.md、references/capabilities/classes-and-inheritance.md、references/capabilities/functions-and-modules.md |
| 让角色按住键持续移动、松开即停；把角色限制在屏幕边界内；发射子弹并批量管理、回收、限数；排查游戏越玩越卡（对象与调试输出） | references/capabilities/pygame-control-and-bullets.md | references/capabilities/pygame-project-setup.md、references/capabilities/pygame-aliens-and-collisions.md |
| 生成成排成列的敌群（网格布局）；让敌群整体巡回移动、触边下移换向；判定子弹与敌人的碰撞、命中即消失；清空一波后自动刷新新一波；敌人漏过时扣命并重置场面；游戏结束后画面僵死／主循环哪些部分继续运行（用开关量做状态门控） | references/capabilities/pygame-aliens-and-collisions.md | references/capabilities/pygame-control-and-bullets.md、references/capabilities/game-scoring-and-state.md |
| 给游戏加得分、最高分、等级显示；清空一波后加快节奏、提高点数；结束后完整重开（不残留、不误触）；核对多杀时的计分是否漏计 | references/capabilities/game-scoring-and-state.md | references/capabilities/pygame-aliens-and-collisions.md、references/capabilities/pygame-project-setup.md、references/capabilities/pygame-control-and-bullets.md |
| 把一组数据画成折线图/散点图；图表读不清，逐项调线宽字号与点样式；把图表保存成文件用于交付；用 Pygal 生成可交互的在线图表 | references/capabilities/data-visualization.md | references/capabilities/data-parsing-and-cleaning.md、references/capabilities/web-api.md |
| 读懂陌生 CSV/JSON 的结构；修复 int() 转换与 ValueError 报错；缺测数据打印后继续处理；名称到编码的映射与失败排查 | references/capabilities/data-parsing-and-cleaning.md | references/capabilities/data-visualization.md、references/capabilities/web-api.md、references/capabilities/files-and-exceptions.md |
| 用 requests 调用一个 Web API；核验状态码与响应结构；排查结果与教程数字对不上；处理配额限制与缺键字段 | references/capabilities/web-api.md | references/capabilities/data-parsing-and-cleaning.md、references/capabilities/data-visualization.md |
| 从零搭一个带数据库的 Django 网站；模型改完不知道要跑哪两条命令；页面报 404 或模板找不到要排因；照既有页面给项目新增一个页面；用管理网站录入并核对初始数据 | references/capabilities/django-project-basics.md | references/capabilities/django-user-accounts.md、references/capabilities/django-styling-and-deploy.md、references/capabilities/classes-and-inheritance.md |
| 让用户只能看到自己创建的数据；表单提交报 403 或重复提交要排查；给项目加注册、登录、注销入口；模型加归属字段后迁移卡在默认值；用 commit=False 给表单数据补外键 | references/capabilities/django-user-accounts.md | references/capabilities/django-project-basics.md、references/capabilities/django-styling-and-deploy.md |
| 给跑通的网站套一个现成样式框架；把本地项目第一次部署上线；上线前核对依赖与配置漂移；线上页面没样式或报错信息过多；让不存在的对象返回 404 而非 500 | references/capabilities/django-styling-and-deploy.md | references/capabilities/django-project-basics.md、references/capabilities/django-user-accounts.md、references/capabilities/git-version-control.md |
| 新机器上装好 Python 并让命令可用；排查 python 命令找不到或版本不对；把编辑器配成一键运行并设定缩进与行长；起变量名前核对关键字与内置函数禁区 | references/capabilities/install-and-editor-reference.md | references/capabilities/env-setup-and-troubleshooting.md、references/capabilities/variables-strings-and-numbers.md |
| 卡住很久时先自检再决定要不要求助；写求助帖前把问题描述准备齐；判断搜到的博客建议可不可信；决定去哪里问、怎么问才有人答；同一个问题反复出现时先做复述自检 | references/capabilities/debugging-and-help-seeking.md | references/capabilities/env-setup-and-troubleshooting.md、references/capabilities/git-version-control.md |
| 给新项目建仓库并完成首次提交；提交后核对工作目录是否干净；把改坏的文件恢复到上次提交；判断回退该用哪一级手段；读懂 detached HEAD 这类提示 | references/capabilities/git-version-control.md | references/capabilities/django-styling-and-deploy.md、references/capabilities/debugging-and-help-seeking.md |

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

- 用户请求超出本书范围（语言或主题不符）时，明确告知超出范围，不要硬套书内流程。
- 涉及可能造成数据丢失的操作（git reset --hard、删除 .git、'w' 模式覆盖文件、数据库覆盖）时，先执行书中对应反例的检查项再动手。
- 涉及 2016 快照步骤（Pygame 源安装、Heroku 部署、旧版本 API）时，先声明版本差异并建议按现环境替换，不得直接照搬。
