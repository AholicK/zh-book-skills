<!-- capability_id: cap.python-crash-course.pygame-project-setup | revision: 1 | status: active -->
<!-- 来源: books/work/python-crash-course/.cangjie 能力单元 v12 第12章上半 游戏项目起步：Pygame、主循环与结构重构（33 条候选） -->

# Pygame 游戏项目起步（第12章上半）

## R — 原文 (Reading)

> 每次给游戏添加新功能时，通常也将引入一些新设置。下面来编写一个名为settings的模块，其中包含一个名为Settings的类，用于将所有设置存储在一个地方，以免在代码中到处添加设置。这样，我们就能传递一个设置对象，而不是众多不同的设置。
>
> — 第12章 12.3.3 创建设置类（L15228-L15232）

## I — 方法论骨架 (Interpretation)

游戏项目与普通脚本的区别在于「持续运行」：窗口每隔一瞬重绘一次，用户的操作被收集成事件再被处理。最小可用骨架就是这条循环——`pygame.init()` → `set_mode((宽, 高))` 建出 screen（surface）→ `while True` 里先取事件（关闭窗口要 `sys.exit()`）再刷新屏幕；`pygame.display.flip()` 擦旧显新，让画面出现平滑移动。

结构从第一天就要防「乱」：把全部可调参数（窗口尺寸、RGB 背景色、预留速度）集中到 settings 模块的 Settings 类里，主程序只传递一个设置对象；飞船、编组这类贯穿整个运行期的对象一律在主循环外创建一次——循环每秒执行数十次，写在里面就会每轮都造新对象。

绘制依赖两个口径：颜色是 RGB 三元组（每个分量 0~255），位置用矩形的 rect 属性族描述（居中用 center/centerx/centery，贴边用 top/bottom/left/right）——原点在屏幕左上角，向右向下坐标增大。

素材要过三检：许可（用无需许可的站点）→ 格式（默认只加载位图 .bmp，其它格式要装图像库或先转换）→ 背景色（选透明背景或与游戏背景色对齐）。

最后是节奏：不预先搭复杂结构，先用单文件把行为跑通；**在添加新代码前**做一次重构——把事件管理与屏幕更新移出主文件，让主循环里每个函数名都能说明当前行为。

## A1 — 书中的应用 (Past Application)

### 案例 1: 从空文件到能开能关的窗口
- **类型**: 原书例题（可运行示范）（第12章 12.3.1）
- **问题**: 要起步写《外星人入侵》，先做出一个可运行、可退出的窗口。
- **方法论的使用**: alien_invasion.py 约 20 行——import sys/pygame、run_game() 内 pygame.init()、screen = pygame.display.set_mode((1200, 800))、set_caption("Alien Invasion")，进入 while True 主循环：for event in pygame.event.get() 检测 pygame.QUIT 并 sys.exit()，每轮 pygame.display.flip()；文件末尾调用 run_game()。
- **结论**: screen 是 surface（屏幕的一部分，用于显示游戏元素）；主循环由事件循环加上屏幕更新组成；flip() 让最近绘制的屏幕可见，营造平滑移动。
- **结果**: 书中给出的结果：运行这些代码，你将看到一个空的 Pygame 窗口。

### 案例 2: 创建设置类 Settings
- **类型**: 原书例题（可运行示范）（第12章 12.3.3）
- **问题**: 每次给游戏加功能都会引入新设置，散落在代码各处会越来越难改。
- **方法论的使用**: 新建 settings.py 定义 Settings 类（__init__ 里设置 screen_width=1200、screen_height=800、bg_color=(230, 230, 230)）；主程序 from settings import Settings、ai_settings = Settings()，并用 ai_settings.screen_width / ai_settings.bg_color 替换硬编码值。
- **结论**: 传一个设置对象而不是众多参数，函数调用更简单；改游戏只需改 settings.py 一处，不必「查找散布在文件中的不同设置」。
- **结果**: 未说明（本节未给出运行结果；游戏外观与此前相同）。

### 案例 3: 把事件与重绘拆进 game_functions 模块
- **类型**: 原书例题（可运行示范）（第12章 12.5）
- **问题**: 单文件 alien_invasion.py 越来越长，而下一步要添加射击功能。
- **方法论的使用**: 先在添加新代码前重构——新建 game_functions.py，把事件循环原样搬进 check_events()（含 QUIT 时 sys.exit()），把更新屏幕的代码搬进 update_screen(ai_settings, screen, ship)；主程序改为 import game_functions as gf，while 循环内只调用 gf.check_events() 与 gf.update_screen(ai_settings, screen, ship)。
- **结论**: 重构在添加新代码前进行、旨在简化结构便于扩展；一次只做原样搬移，不加新功能。
- **结果**: 未说明（书中说明重构使 while 循环更简单、后续开发更容易，未给出新的运行输出）。

## A2 — 触发场景 (Future Trigger) ★

### 用户会在什么情境下需要这个 skill?

1. 想从零开始写一个 2D 小游戏，不知道第一步搭什么
2. pygame 装好、窗口能显示，但游戏越跑越卡
3. 项目文件越来越多，import 自己写的模块报 ModuleNotFoundError
4. 改一个游戏参数要翻好几个文件，想找个地方集中管
5. 加新功能前，代码已经乱得不敢动了

### 语言信号

- 「想用 Python 做个游戏，从哪开始？」
- 「游戏跑一会儿就越来越卡」
- 「自己写的模块导不进来，报 ModuleNotFoundError」
- 「怎么改窗口大小和背景色？」
- 英文: "how to start a pygame game project", "my pygame game slows down after a while", "ModuleNotFoundError for my own module"

### 与相邻能力的区分

- 与 `pygame-control-and-bullets` 的区别: 那条是同一项目的下一段——飞船持续移动、按键与子弹；本卡到「可扩展骨架 + 第一次重构」为止，不包含操控与射击；
- 与 `classes-and-inheritance` 的区别: 那条讲类与继承的语法口径；本卡的 Settings 与 Ship 只是把参数与素材组织起来的落点，不重复讲类语法；
- 与 `functions-and-modules` 的区别: 那条讲函数与模块的一般写法（实参与导入）；本卡讲的是「加新功能前先重构」的时机与拆分位置。

## E — 可执行步骤 (Execution)

1. **先写项目描述，摆好文件结构**
   - 完成标准: 描述能回答「玩家做什么、怎么操作、什么算结束」；项目文件夹建好后，所有 .py 与素材子目录（images/）都放进去
   - 判停条件: 描述不必覆盖全部细节，能让你清楚地知道该如何动手开发它即可
2. **准备可导入的环境**
   - 完成标准: 用 pip --version（或 python -m pip --version）确认 pip 与当前使用的 Python 版本关联正确；安装 Pygame 后在解释器里执行 import pygame、无输出即成功
   - 判停条件: import 失败先核对 pip 关联的解释器，再检查对应平台的安装方式
   - 注: 安装渠道与命令为 2016 年快照，按现环境替换
3. **搭最小骨架并保持主循环干净**
   - 完成标准: run_game() 完成 init → set_mode((宽, 高)) → set_caption → while True（事件循环 + 屏幕刷新 + flip）；窗口能开、能通过关闭按钮退出
   - 输出契约: 空窗口 + 已设置的背景色（RGB 三元组，分量 0~255）
4. **参数全部集中到 Settings**
   - 完成标准: 窗口宽高、背景色、预留速度等全部放进 settings 模块的 Settings 类；主程序中不再出现裸值；长生命周期对象（ai_settings、screen、飞船实例）一律在主循环外创建
   - 输入契约: 每次要加的功能所需的可调值，先想清楚它属于 Settings 还是局部变量
5. **加入第一个游戏元素（飞船）**
   - 完成标准: 素材过三检（许可 → 位图格式 → 背景色匹配）后放入 images/；用 image.load() + get_rect()，按 rect 属性族把元素定位到预期位置（如 bottom 贴底、centerx 居中），blit 绘制后屏幕上出现预期画面
   - 判停条件: 画面出现异色矩形或加载报错时，回到素材三检逐项核对，而不是先怀疑路径
6. **在添加下一功能前重构**
   - 完成标准: 事件管理（check_events）与屏幕更新（update_screen）已移出主文件、放进 game_functions 模块；主循环里只剩少量函数调用，每个函数名都能说明当前行为
   - 判停条件: 先接受「一开始尽可能简单、复杂后再重构」的节奏——没有新功能要加、文件也不长时，不必提前拆分

## B — 边界 (Boundary) ★

### 不要在以下情况使用

- 一次性验证脚本——没有持续动画与交互需求时，主循环骨架只会增加复杂度；
- 3D 或非 Pygame 技术栈——本卡口径是 2D 位图工作流（依赖 2016 年快照的 Pygame 安装链条，按现环境替换）；
- 接手别人维护的既有游戏代码——先沿用它自己的结构，不要照搬本书从零起步的顺序。

### 作者在书中警告的失败模式

- ce101：项目文件分散在不同目录——项目内模块的 import 失败，报错发生在运行时且容易被误判为语法或安装问题；所有文件必须放进同一个项目文件夹；
- ce102：pip 关联到错误的 Python 版本——安装显示成功，`import pygame` 却说找不到模块；排查时先核对 pip 与解释器版本是否一致；
- ce105：在主循环内创建对象（飞船、编组）——每轮循环都造新对象，游戏运行时将创建数千个编组、「慢得像蜗牛」；游戏停滞时先看主循环里发生了什么；
- ce109：素材许可与背景色不匹配——随手取的图有版权风险；非透明背景的图会被整块绘制上屏，物体顶着一个难看的方块。

### 作者的盲点 / 时代局限

- 2016 年快照，按现环境替换：安装三段式绑定 2015 年的渠道（hg+bitbucket 源码、固定 whl 文件名如 cp35），素材站点与流程照搬会失败；
- 图与截图内容在抽取稿中全失（仅存图注），代码与正文存在粘连、缩进损失（如 L15266 处 pygame.init() 与 Settings() 挤在一行），照抄时须自行核对；
- 原书对安装失败只给「看看在线说明是否管用」一处处置，且未涉及分辨率自适应、资源管理等工程问题。

### 容易混淆的邻近方法论

- `pygame-control-and-bullets`: 同项目的下一段（持续移动、按键、子弹）——本卡完成后才轮到它；
- `classes-and-inheritance`: 类的语法与继承口径在那条；本卡用到的 Settings / Ship 只是配置与素材的落点；
- `functions-and-modules`: 函数与模块的一般写法在那条；本卡的判断是「什么时候拆、拆到哪」——加新功能之前。

---

## 相关 skills（阶段 3 定稿）

- depends-on: `classes-and-inheritance`（Settings / Ship 等类的基础）
- composes-with: `pygame-control-and-bullets`（同一项目的下段）、`functions-and-modules`（模块拆分与函数纪律）

---

## 审计信息

- **验证通过**: V1 ✓ / V2 ✓ / V3 ✓（见 `verified-v12.md` 的 v12 单元）
- **蒸馏时间**: 2026-09-18
