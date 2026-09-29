<!-- capability_id: cap.python-crash-course.django-project-basics | revision: 1 | status: active -->
<!-- 来源: books/work/python-crash-course/.cangjie 能力单元 v19 Django 项目入门：模型、迁移、视图与模板（50 条候选） -->

# Django 项目入门：模型、迁移、视图与模板（第18章）

## R — 原文 (Reading)

> 我们将修改数据库称为迁移数据库。首次执行命令migrate时，将让Django确保数据库与项目的当前状态匹配。在使用SQLite（后面将更详细地介绍）的新项目中首次执行这个命令时，Django将新建一个数据库。
>
> — 第18章 18.1.7 创建数据库（L24642-L24644）

## I — 方法论骨架 (Interpretation)

Django 把「一个能存数据、能打开页面的网站」拆成一条固定流水线：**项目骨架 → 应用 → 模型（类）→ 迁移（同步数据库）→ 管理网站与 shell（录入并验证数据）→ 逐个页面走 URL→视图→模板**。每一步都有可见的检查点，不需要一次看懂整条链路。

- **模型就是类**：继承 `models.Model`，属性即字段——少量文本用 `CharField`（必须带 `max_length`），长文本用 `TextField`，时间戳用 `DateTimeField(auto_now_add=True)`；`__str__()` 给出方便阅读的表示，嵌套 `Meta` 放管理用的额外信息，表与表之间用外键建立关联（多对一）。
- **迁移是模型与数据库之间的唯一同步通道**：改 `models.py` 只是改代码；`makemigrations` 决定如何修改数据库并生成迁移文件，`migrate` 真正应用改动，输出里的 `...OK` 就是检查点。改一次模型，这套动作重复一次。
- **URL→视图→模板是每个页面的固定三步**：URL 模式由「正则 + 视图函数 + 名字」三部分构成；模板用名字生成链接（改路由不必动模板）；视图取数据、装进上下文（context）字典后交给模板；模板用 `{{ }}` 显示值、`{% %}` 写逻辑，循环必须显式 `{% endfor %}` 收尾。
- **先验证数据，再写页面**：初始数据从管理网站录入；查询先在 `manage.py shell` 里试验——在简单环境里排错的反馈速度远高于「改视图 → 刷浏览器」。
- 判据：页面打不开时先查三件事——URL 模式匹配得上吗（不匹配一律 404）、视图有没有传上下文、模板有没有放在 `应用名/templates/应用名/` 这层同名目录里。

## A1 — 书中的应用 (Past Application)

### 案例 1: 激活 Topic 模型的迁移工作流
- **类型**: 原书例题（可运行示范）（第18章 18.2.2 激活模型）
- **问题**: 新定义的模型还只存在于代码里，数据库中没有对应的表
- **方法论的使用**: 把 `'learning_logs'` 追加进 `settings.py` 的 `INSTALLED_APPS`；执行 `python manage.py makemigrations learning_logs`；再执行 `python manage.py migrate`
- **结论**: 两步分工明确——`makemigrations` 确定如何修改数据库并生成迁移文件，`migrate` 负责应用；两道命令都完成，模型才算生效
- **结果**: 书中给出的结果：makemigrations 输出 `0001_initial.py - Create model Topic`，migrate 输出 `Applying learning_logs.0001_initial... OK`

### 案例 2: 主页按 URL→视图→模板建立
- **类型**: 原书例题（可运行示范）（第18章 18.3 创建网页）
- **问题**: 想把 Django 默认欢迎页换成「学习笔记」自己的主页
- **方法论的使用**: 项目级 `urls.py` 用 `include('learning_logs.urls', namespace='learning_logs')` 引入应用路由；应用级 `urls.py` 写 `url(r'^$', views.index, name='index')`；视图 `def index(request):` 直接 `return render(request, 'learning_logs/index.html')`；模板写两个 `<p>` 段落
- **结论**: 一个页面就是三处改动，顺序固定：定义 URL → 编写视图 → 编写模板；先让页面尽可能简单，样式以后再加
- **结果**: 书中给出的结果：请求项目基础 URL 时显示刚创建的新主页（图18-3）

### 案例 3: 用 base.html 建立模板继承
- **类型**: 原书教学示例（可运行示范）（第18章 18.4.1 模板继承）
- **问题**: 所有网页都要重复写页首标题与到主页的链接，改一处要动很多文件
- **方法论的使用**: 新建 `base.html` 容纳通用元素（含用 `{% url 'learning_logs:index' %}` 生成的主页链接），并预留 `{% block content %}{% endblock content %}` 占位；子模板第一行写 `{% extends "learning_logs/base.html" %}`，只填自己特有的块
- **结论**: 通用元素集中在父模板，子模板只保留独有内容；修改父模板会传导到所有继承它的页面
- **结果**: 未说明（该节没有独立的运行截图，其效果在 18.4.2 的主题页面中体现）

## A2 — 触发场景 (Future Trigger) ★

### 用户会在什么情境下需要这个 skill?

1. 第一次用 Django 搭一个带数据库的网站，不确定从哪一步开始
2. 改完模型继续往下写，运行时报「表不存在 / 列不存在」，不知道还差哪条命令
3. 页面返回 404，但视图函数和模板文件明明都在
4. 要新增一个页面，想照既有页面把 URL、视图、模板三处补齐
5. 模板里写循环，渲染时报模板语法错误

### 语言信号

- 「Django 项目怎么建？模型改完要跑什么命令？」
- 「makemigrations 跑过了，还要 migrate 吗？」
- 「404 / TemplateDoesNotExist 到底差在哪？」
- 英文: "django makemigrations vs migrate", "django url routing", "TemplateDoesNotExist"

### 与相邻能力的区分

- 与 `django-user-accounts` 的区别: 本卡只解决「数据怎么建模、页面怎么出来」；注册登录、页面访问控制与数据归属属于那一条。
- 与 `django-styling-and-deploy` 的区别: 本卡用最简 HTML 把功能跑通，外观与上线属于那一条。
- 与 `classes-and-inheritance` 的区别: 模型写起来就是普通类；类名、属性、继承的通用口径见那一条，本卡只加「字段用 Django 提供的类型、改完必须迁移」。

## E — 可执行步骤 (Execution)

1. **搭骨架并建库**
   - 完成标准: 在活动虚拟环境中执行 `django-admin.py startproject 项目名 .`（末尾句点不可省），目录里出现 `manage.py` 与项目包；执行 `python manage.py migrate` 后出现数据库文件；`python manage.py runserver` 启动成功，浏览器能打开基础 URL
   - 判停条件: 提示「端口已被占用」就递增端口重试；提示找不到 django-admin.py 或 Django 不可用，先回到虚拟环境检查激活状态
   - 输入契约: 一个可写的项目目录 + 已激活的虚拟环境；不在其中的命令一律先补激活动作

2. **定义模型并把应用接进项目**
   - 完成标准: 用 `manage.py startapp` 生成应用目录；`models.py` 里每个字段选对类型（少量文本 `CharField` 带 `max_length`、长文本 `TextField`、时间戳 `DateTimeField(auto_now_add=True)`），并给出 `__str__()`；应用名写进 `INSTALLED_APPS`
   - 输出契约: 可被导入的模型类 + 注册完成的 `INSTALLED_APPS`

3. **迁移并向数据库验证**
   - 完成标准: `python manage.py makemigrations 应用名` 生成迁移文件；`python manage.py migrate` 的输出里能看到 `Applying ...OK`；进入 `manage.py shell` 能用 `模型.objects.all()` 列出数据
   - 判停条件: 只改了 `models.py` 而没走这两条命令时先补齐再继续；改动模型后，原先打开的 shell 会话要重新进入

4. **用管理网站录入并核对初始数据**
   - 完成标准: `createsuperuser` 建好超级用户；自建模型在 `admin.py` 里用 `admin.site.register()` 手工注册；后台能增删改，且列表里显示的是 `__str__()` 给出的表示

5. **逐个页面走三步，并做模板检查**
   - 完成标准: 每个新页面都补齐 URL 模式（`name` 有值）+ 视图（`render` 带 context 字典）+ 模板（放在 `应用名/templates/应用名/`）；模板里 `{% for %}` 与 `{% endfor %}` 成对、空列表走 `{% empty %}`；页面之间用 `{% url %}` 链接而不是硬编码地址

## B — 边界 (Boundary) ★

### 不要在以下情况使用

- 不需要数据库的纯静态页面或脚本类任务——模型与迁移这套装置会一直挡在路上。
- 项目已经选定其他 Web 框架（Flask、FastAPI 等）——这里的目录结构、命令与模板语法只对 Django 成立。
- 只想做一次性演示、不打算长期维护数据——管理网站、shell、迁移的成本回收不了。

### 作者在书中警告的失败模式

- **ce161（漏掉末尾句点）**: `startproject` 不写末尾句点，开发期间一切照常，直到部署才发现目录结构不对，届时只能删掉重建。
- **ce166（改模型不走三步）**: 只改 `models.py` 不迁移，数据库结构与代码不一致，报错在后续操作时才冒出来（表/列不存在），定位成本远高于当场补一条命令。
- **ce169（模板目录层级）**: 模板必须放在 `应用名/templates/应用名/` 这层同名目录下；少了它，Django 找不到模板报 `TemplateDoesNotExist`，项目变大后同名模板还会互相覆盖。
- **ce172（URL 全靠正则）**: URL 模式就是正则表达式，多一个斜杠或锚点写错，表现都只是 404，错误信息不会指向正则本身。

### 作者的盲点 / 时代局限

- **2016 年快照，按现环境替换**: 本卡全部基于 Django 1.8.4 与 `url()` + 正则的路由写法；后续版本提供了更友好的写法（如 `path()`），版本号与命令请以现环境为准。
- 版本兼容残留: 书中专门注明 Python 2.7 要写 `__unicode__()` 而非 `__str__()`（ce164）——按现代环境只需 `__str__()`。
- 原文缺陷照抄: 18.2.1 的正文把 `auto_now_add` 写成 `auto_add_now`（ce167）；18-3 练习的文档链接写成 Django `en/7.8`（实为 1.8，ce168）。照文字手打会踩坑。
- 图与截图在文本转档中整体丢失: `图18-1`～`图18-5` 只剩图注，页面外观无法按书核对；代码块与正文粘连处有缩进损失，抄写时以实际结构为准。

### 容易混淆的邻近方法论

- `django-user-accounts`: 同一条 Django 路线上的下一段——本卡解决「数据怎么建模、页面怎么出来」，多用户、权限与归属是它的主题。
- `classes-and-inheritance`: 模型与类同源，但那是一条一般的 Python 类语法；只有涉及字段类型、迁移、管理网站时才归本卡。

---

## 相关 skills（阶段 3 定稿）

- depends-on: `classes-and-inheritance`（模型就是类）
- composes-with: `django-user-accounts`、`django-styling-and-deploy`（同一项目的后续两段）

---

## 审计信息

- **验证通过**: V1 ✓ / V2 ✓ / V3 ✓（见 verified-v19.md 的 v19 单元）
- **蒸馏时间**: 2026-09-18
