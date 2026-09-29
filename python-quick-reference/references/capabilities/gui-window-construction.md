<!-- capability_id: cap.python-quick-reference.gui-window-construction | revision: 1 | status: active -->
<!-- 来源: 单元 v20；归并候选 3 条（f34, f35, g24） -->

# GUI 窗口与控件构造（wxPython：wx.Frame / StaticText / TextCtrl / Button / sizer 选型）

## R — 原文 (Reading)

> wx.Frame 构造器的语法格式如下：
wx.Frame(parent, id=-1, title="", pos=wx.DefaultPosition, size=wx.DefaultSize,
style=Wx.DEFAULT_FRAME_STYLE, name="frame") 参数说明：
>
> — 第 15 章 · GUI 界面编程「必背必记 2．使用 wx.Frame 框架」L2167-2195

> parent：父窗口部件。
id：标识符。使用-1可以自动创建一个唯一的标识。
label：显示在静态控件中的文本内容。
pos：一个wx.Point或一个Python元组，它是窗口部件的位置。
size：一个wx.Size或一个Python元组，它是窗口部件的尺寸。
style：样式标记。
>
> — 第 15 章 · GUI 界面编程「必背必记 3-5」L2197-2257

## I — 方法论骨架 (Interpretation)

在 wxPython 里，**窗口和控件都是构造出来的对象**：读懂一个例子的关键是把构造器的参数逐个对上号，而不是去猜运行效果。

- **窗口（`wx.Frame`）七参数**：`parent` 父窗口（顶级填 `None`）、`id`（通常设 -1 让 wxPython 自动生成唯一标识）、`title` 标题、`pos`/`size`（默认由系统决定）、`style` 窗口类型常量（可用或运算组合）、`name` 框架内部名字（用于查找窗口）。
- **控件三件套**：`StaticText` 显示文本（label 为文本内容）；`TextCtrl` 输入框（value 为初始文本；style 可取 wx.TE_CENTER / LEFT / RIGHT / PASSWORD / READONLY / PROCESS_ENTER / PROCESS_TAB / NOHIDESEL）；`Button`（参数与 TextCtrl 基本相同，label 为按钮上的文本）。公共参数：`parent`/`id`/`pos`/`size`/`style`/`name`。
- **布局（sizer）只管怎么排**：书中给出五类（BoxSizer / GridSizer / FlexGridSizer / GridBagSizer / StaticBoxSizer）与各自适用场景；选型判据：等尺寸放规则网格用 GridSizer、尺寸不一用 FlexGridSizer、最灵活用 GridBagSizer、单行/单列用 BoxSizer。

本卡的能力边界是**按参数说明正确构造**：全书没有 `wx.App`、主循环与事件绑定（NR-14），所以不承诺"程序能跑起来"。

## A1 — 应用示例 (Application Example)

本单元候选里没有原书示例（全章无完整示例代码），以下两条都是演练。

### 示例 1: 「计算器」窗口 + 一个标签 + 只读输入框 + 一个按钮
- **类型**: 合成演练（按原书规则构造，非作者案例）
- **问题**: 标题为"计算器"、位置与尺寸由系统决定的窗口，里面放标签、只读输入框、按钮，布局用哪个 sizer
- **怎么用**: 窗口 `wx.Frame(None, -1, "计算器")`（parent 填 `None`、id 设 -1、pos/size 用默认）；标签 `wx.StaticText(parent, -1, "标签文本")`；输入框 `wx.TextCtrl(parent, -1, "", style=wx.TE_READONLY)`（样式取自书中 style 取值列表）；按钮 `wx.Button(parent, -1, "确定")`；单行排列用 BoxSizer
- **结果**: 书中未说明（纸面推演）。演练**未写** `wx.App`、主循环、事件绑定（书中均未出现），只声称"构造参数正确"

### 示例 2: 省略掉的参数各对应哪个默认值
- **类型**: 合成演练（按原书规则构造，非作者案例）
- **问题**: `wx.Frame(None, -1, "计算器")` 省略的 pos/size/style/name 分别是什么
- **怎么用**: 按书中签名逐位对齐——pos=wx.DefaultPosition、size=wx.DefaultSize、style=Wx.DEFAULT_FRAME_STYLE、name="frame"（后两项按原文照抄，含其大小写）
- **结果**: 书中未说明（无示例代码）

## A2 — 触发场景 (Future Trigger) ★

### 用户会在什么情境下需要这张卡?
1. 读懂或改写一段 wxPython 例子里的窗口与控件构造
2. 记不清 `wx.Frame` 有哪些参数、哪些可以省略
3. 输入框要只读或密码显示，不知道用哪个 style 常量
4. 放了几个控件，不知道该选哪一类 sizer
5. 顶级窗口与控件的 `parent` 分别传什么

### 语言信号
- 「wx.Frame 的参数」「wxPython 怎么建窗口」「TextCtrl 只读怎么设」
- 「sizer 选哪个」「parent 传 None 还是 self」
- 英文: "wx.Frame constructor arguments", "wxPython TextCtrl readonly style", "which sizer to use"

### 与相邻能力的区分（阶段 3 定稿）
- 与 `class-and-objects` 的区别: 本卡只覆盖"用哪个构造器、参数怎么填"这一层；窗口与控件都是类的实例，"类与实例化"、parent 逐层传递与点操作符调用方法这套机制走那条能力

## E — 可执行步骤 (Execution)

**输入契约**：①窗口标题与用途 ②要放哪些控件（类型、显示文本、是否需要特殊样式）③位置尺寸是否显式指定 ④控件怎么排。缺②时先问"界面上要有哪几样东西"。

1. **列控件清单** — 完成标准: 每行一个控件（类型 + 显示文本 + 是否需要只读/密码等样式）
2. **定窗口参数** — 完成标准: 写出 `wx.Frame(...)` 并把七个参数逐一落到取值上；省略的参数必须说明"用默认、由系统决定"
3. **逐控件选构造器与 style** — 完成标准: 每个控件一个构造调用，`parent`/`id`/文本参数齐全；所用 style 常量都能在书中的 style 取值列表里找到
4. **选 sizer** — 完成标准: 给出所选 sizer 类别并说明理由（部件尺寸是否一致、要几行几列）
5. **声明边界** — 完成标准: 交付物中不出现 `wx.App`、主循环、事件绑定（书中均未出现，NR-14）；说明交付的是"构造参数正确"，不是"可运行程序"

**输出契约**：交付物 = ①一行一个控件的结构清单 ②窗口与全部控件参数填全的构造调用 ③sizer 选型与理由 ④边界声明。完成标准：构造调用中每个参数都能对应书中的参数说明，不出现书中没有的构造器或常量。

## B — 边界 (Boundary) ★

### 不要在以下情况使用这张卡
- 要一个"能弹出来的窗口程序"：全书无 `wx.App`、主循环与事件绑定（NR-14），**本书未覆盖**，不能声称程序能跑起来
- 要写按钮点击后的处理逻辑（事件绑定/回调）：**本书未覆盖**
- 要用 PyQt/Tkinter 等其他 GUI 库：第 15 章只讲 wxPython 的构造参数与 sizer 选型
- 要 sizer 的实际布局代码：书中只给适用场景的对照

### 作者在书中警告的失败模式
- 本节没有写成警告句的失败模式；可作约束的只有参数口径（`id` 用 -1 自动创建唯一标识、`pos`/`size` 省略由系统决定），以及书中对 BoxSizer 的说法"当尺寸改变时，控制窗口部件的行为"[原文如此]
- 主要风险是越界承诺：底本没有"怎么让窗口显示出来"的部分

### 作者盲点与底本缺口
- 全书无 `wx.App`、主循环、事件绑定（NR-14）——**本书未覆盖**；本单元能力边界只能是"按参数说明正确构造"
- 全章无完整示例代码，证据仅 3 条候选（f34/f35/g24），无可复算的示例输出
- OCR 现象（NR-15）：窗口样式常量按原文照抄为 `Wx.DEFAULT_FRAME_STYLE`（大小写未规范化）；style 取值与说明列可能错行，对应由本项目按语义核对

### 容易混淆的邻近方法论
- `class-and-objects`: 构造器就是实例化；本卡不给"类怎么定义、实例怎么调方法"的内容

---

## 相关能力（阶段 3 定稿）

- depends-on: `class-and-objects`（窗口与控件都是类的实例，parent 逐层传递、方法用点操作符调用）

---

## 审计信息

- **验证通过**: V1 ✓ / V2 ✓ / V3 ✓（阶段 1.5，单元 v20）
- **证据**: 候选 3 条；ck-2647e1bf37c4
- **蒸馏时间**: 2026-09-17
