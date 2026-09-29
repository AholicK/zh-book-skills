<!-- capability_id: cap.python-crash-course.unit-testing | revision: 1 | status: active -->
<!-- 来源: books/work/python-crash-course/.cangjie 能力单元 v11 第11章 测试代码：unittest 单元测试（25 条候选） -->

# 单元测试 unittest（第11章）

## R — 原文 (Reading)

> Python标准库中的模块unittest提供了代码测试工具。单元测试用于核实函数的某个方面没有问题；测试用例是一组单元测试，这些单元测试一起核实函数在各种情形下的行为都符合要求。
>
> — 第11章 11.1.1 单元测试和测试用例（L13826-L13828）

## I — 方法论骨架 (Interpretation)

单元测试把「这段代码应该有什么行为」从口头约定变成可以重复执行的检查：为函数或类写一个继承 `unittest.TestCase` 的测试类，把每个行为写成 `test_` 打头的方法，运行测试文件时 Python 自动调用它们，失败时点名到具体方法。

写法固定为四件套：`import unittest` 与被测对象 → 测试类（名字与被测对象相关、含字样 Test）→ 每个行为一个描述性的 `test_` 方法（调用被测函数、把结果存入变量、用断言核对）→ 文件末尾 `unittest.main()`。

断言是判据的载体，按「相等 / 真假 / 包含」三类选择（assertEqual、assertNotEqual、assertTrue、assertFalse、assertIn、assertNotIn 六个），且只能在继承 TestCase 的类里使用。测类的路径多一步：先创建实例，再断言它的状态。

核心纪律只有一条：**在检查条件没错的前提下，测试未通过意味着代码有错——修代码，而不是修改测试**。修改测试等于篡改判据，测试从此失去发现回归的能力。

运行输出用单字符回报：通过一个句点、引发错误一个 E、断言失败一个 F，首行字符序列与末尾的 OK / FAILED 总结就是全部信息。范围上先覆盖重要行为——大型项目的全覆盖很难，项目早期不要试图写全覆盖式测试用例。

## A1 — 书中的应用 (Past Application)

### 案例 1: get_formatted_name() 的红灯与修复
- **类型**: 原书例题（可运行示范）（第11章 11.1）
- **问题**: 为 name_function.py 的 get_formatted_name() 写好测试后，函数新增了一个「中间名必填」的修改，测试立即失败。
- **方法论的使用**: 测试文件按四件套组织（import unittest 与被测函数、继承 TestCase 的测试类、test_first_last_name、assertEqual 断言、unittest.main()）；运行得到首行 E 与 FAILED (errors=1)，报告点出 TypeError: get_formatted_name() missing 1 required positional argument: 'last'；按纪律不改测试，把中间名改成可选（middle=''）并加 if middle: 分支。
- **结论**: 未通过的测试让「新代码破坏了函数原来的行为」暴露出来，修复因此容易；处置动作是修代码。
- **结果**: 书中给出的输出：通过版为句点 + Ran 1 test in 0.000s + OK；失败版为 E + ERROR: test_first_last_name + TypeError … + FAILED (errors=1)；修改后重跑再次 OK。

### 案例 2: 追加中间名测试与命名纪律
- **类型**: 原书例题（可运行示范）（第11章 11.1.5）
- **问题**: 函数现在支持「名+中间名+姓」的输入，需要为这个新行为补一条测试。
- **方法论的使用**: 在 NamesTestCase 中新增 test_first_last_middle_name()，用三个实参调用 get_formatted_name()，用 assertEqual 断言 'Wolfgang Amadeus Mozart'；重跑测试文件。
- **结论**: 方法名必须以 test_ 打头才会自动运行；名称要描述性——测试未通过时能立刻看出受影响的是哪种姓名情形，名字长没关系。
- **结果**: 书中给出的输出：两个句点 + Ran 2 tests in 0.000s + OK。

### 案例 3: 测试 AnonymousSurvey 类——assertIn 与 setUp()
- **类型**: 原书例题（可运行示范）（第11章 11.2）
- **问题**: 要为 survey.py 的 AnonymousSurvey 类验证「答案被妥善存储」这一行为；两个测试方法各自重复创建实例与答案列表。
- **方法论的使用**: 测试方法里先 my_survey = AnonymousSurvey(question)、调用 store_response('English')，再 self.assertIn('English', my_survey.responses)；随后加 test_store_three_responses() 用循环验证三个答案；最后把实例与答案列表移进 setUp()，用 self.my_survey / self.responses 供各方法直接取用。
- **结论**: 测类先建实例再断言其状态；setUp() 在运行每个 test_ 方法前自动执行，让共享对象只创建一次。
- **结果**: 书中给出的输出：单答案测试 Ran 1 test … OK；两测试版 .. + Ran 2 tests … OK；language_survey.py 输出 Survey results: 与 - English / - Spanish / - English / - Mandarin。

## A2 — 触发场景 (Future Trigger) ★

### 用户会在什么情境下需要这个 skill?

1. 改动一个函数或类之前，想先有一张「原有行为」的安全网
2. 为代码写测试，但不确定文件该怎么组织、断言该怎么选
3. 测试跑出一片 E 和 F，不知道哪个测试失败、该怎么查
4. 测试没通过，犹豫是改代码还是改测试
5. 想知道测试要写多少——要不要追求全覆盖

### 语言信号

- 「怎么用 unittest 给函数写测试」
- 「测试跑出来一堆 E 和 F，看不懂」
- 「测试没通过，是不是把测试改一下就行了」
- 「这次改动会不会破坏原来的功能」
- 英文: "how do I write unit tests for my function", "my test failed — should I fix the code or the test", "does this change break existing behavior"

### 与相邻能力的区分

- 与 `functions-and-modules` 的区别: 那条负责把代码拆成职责单一的函数；本卡为这些函数与类建立可重复的行为检查；
- 与 `classes-and-inheritance` 的区别: 那条讲类的写法与继承机制（测试类也是普通类，但本卡聚焦测试组织，不教类语法）；
- 与 `files-and-exceptions` 的区别: 那条让程序出错可控（异常兜底）；本卡让改动可验证（回归安全网）——两者都服务于「程序不因修改而变坏」。

## E — 可执行步骤 (Execution)

1. **列重要行为清单**
   - 完成标准: 每个要测的行为写成一句话（输入 → 期望结果），并标注它是最容易被后续改动碰坏的行为之一
   - 判停条件: 不追求穷举——项目早期只测重要行为，除非有充分理由
2. **搭四件套骨架**
   - 完成标准: 测试文件包含 import unittest 与被测对象、继承 unittest.TestCase 的测试类（命名与所测对象相关并含 Test）、至少一个 test_ 方法、文件末尾 unittest.main()
   - 输出契约: 测试文件可独立运行
3. **写函数级测试方法**
   - 完成标准: 每个方法以描述性的 test_ 开头；调用被测函数、把返回值存入变量、用断言核对
   - 输入契约: 断言选自六个常用方法（相等/不等/真假/包含），且写在 TestCase 子类内
4. **写类级测试并收拢夹具**
   - 完成标准: 先创建实例再断言其状态（如存入后用 assertIn 核实）；多个方法重复准备数据时，共享实例与数据移进 setUp()、用 self 属性存储，各方法直接取用
5. **运行并读输出**
   - 完成标准: 能逐个说出首行每个句点 / E / F 对应哪个测试；能报出 Ran N tests 与结尾 OK / FAILED 的含义
6. **处置失败**
   - 完成标准: 先复核断言条件本身是否写错；条件没错时修改代码而不是测试，修复后全部测试转为通过
   - 判停条件: 若确认是「检查条件」写错（如期望值本来就不对），可以改测试——但必须说清条件错在哪里，而不是为了变绿

## B — 边界 (Boundary) ★

### 不要在以下情况使用

- 一次性试验脚本——作者明确说初学者并非必须为每个项目写测试；
- 把「全覆盖」当验收门槛——大型项目全覆盖很难，先测重要行为，等项目被广泛使用时再考虑；
- 需要跨模块协作、外部接口的集成级验证——原书范围是单元测试（unittest 标准库），不含这类测试组织。

### 作者在书中警告的失败模式

- ce82：测试未通过时去修改测试而不是代码——测试通过而缺陷仍在，从此失去发现回归的能力（自欺的绿灯）；
- ce86：每个测试方法重复「创建实例 + 准备数据」——准备逻辑要改就得改所有方法，漏改即出错，应该用 setUp() 消除；
- ce84、ce85：示例本身带缺陷——survey.py 的 show_question()/show_results() 缺 self. 前缀 [原文如此]，代码类名 TestAnonmyousSurvey 与正文拼写不一致；照抄排查时优先核对这两处；
- ce87：扩展功能时连带改坏原有行为（如让用户可输入多个答案时破坏了单个答案的处理方式）——没有针对类的测试，缺陷会留到用户使用才发现。

### 作者的盲点 / 时代局限

- 2016 年快照，按现环境替换：全书测试内容基于 2016 年前后的 Python 3 与标准库 unittest——pytest 等第三方运行器、tearDown、测试目录组织均未涉及，按现环境替换；
- 断言方法只有表 11-1 的 6 个（L14196-L14250），assertRaises 等未覆盖，属超出原书范围；
- 转档缺陷：抽取稿中图与截图内容全失、代码缩进有损失（且原书即有两处未标注的示例缺陷，见上），照抄时须自行核对。

### 容易混淆的邻近方法论

- `functions-and-modules`: 定义被测对象的单一职责与导入组织；测试是为其建立检查，而不是替代它；
- `classes-and-inheritance`: 测试类本身就继承 TestCase——类的语法口径在那条，本卡只关心测试组织；
- `files-and-exceptions`: 异常兜底让程序出错可控；测试让「改动是否破坏行为」可验证，两者是彼此独立的安全网。

---

## 相关 skills（阶段 3 定稿）

- depends-on: `functions-and-modules`、`classes-and-inheritance`（测试的对象是函数与类）
- composes-with: `files-and-exceptions`（重构与改动的回归验证）

---

## 审计信息

- **验证通过**: V1 ✓ / V2 ✓ / V3 ✓（见 `verified-v11.md` 的 v11 单元）
- **蒸馏时间**: 2026-09-18
