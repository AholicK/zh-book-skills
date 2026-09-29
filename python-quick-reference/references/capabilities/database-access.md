<!-- capability_id: cap.python-quick-reference.database-access | revision: 2 | status: active -->
<!-- 来源: 单元 v16；归并候选 7 条（f29/f30/p29/p30/p40/g20/g21） -->

# 数据库访问（连接对象 / 游标对象 / SQL 四类语句 / fetch 三法）

## R — 原文 (Reading)

> ConnectO函数返回连接对象。这个对象表示目前和数据库的会话，连接对象支持的方法如下表所示。
> 方法名
> 说明 closeO
> 关闭数据库连接 commitO
> 提交事务 rollbackO
> 回滚事务 cursorO
> 获取游标对象，操作数据库，如执行DML操作，调用存储过程等 2．游标对象
>
> — 第 14 章 · 操作数据库「必背必记 1-2」L1977-2023

> 查看用户信息的代码与插入数据信息大致相同，不同点在于使用的SQL语句不同。此外，查询数据时通常使用如下3种方式：
> fetchoneO：获取查询结果集中的下一条记录。
> fetchmany(size)：获取指定数量的记录。
> fetchallO：获取结构集的所有记录。
>
> — 第 14 章 · 操作数据库「必背必记 3．操作 SQLite」L2053-2097

## I — 方法论骨架 (Interpretation)

这张卡解决「一次数据库操作该由谁来做」：书里把访问拆成两层对象——先用 `connect()` 取**连接对象**（一次会话），再用它的 `cursor()` 取**游标对象**；SQL 发给游标执行、结果也从游标取回。两层混用（SQL 发给连接对象、在游标上提交事务）是最常见的形态错误。

连接对象的职责写在方法表里：close 关闭连接、commit 提交事务、rollback 回滚事务、cursor 获取游标对象（操作数据库，如执行 DML 操作）——**会话与事务归连接对象**，方法表里没有执行 SQL 的入口。

游标对象代表数据库中的游标。**执行与取数都归它**：execute(operation[, parameters]) 执行 SQL 或数据库命令、executemany(operation, seq_of_params) 批量操作；取结果三种方式——fetchone() 下一条、fetchmany(size) 指定数量、fetchall() 所有记录。

SQL 用四类模板配合 execute()：`insert into 表名(字段名1，…，字段名n) values(…)`、`select 字段名1，… from 表名 where 查询条件`、`update 表名 set 字段名 = 字段值 where 查询条件`、`delete from 表名 where 查询条件`。顺序：连接 → 游标 → execute → （改数据时）commit → 取结果 → close。

底本缺口：书中只有 `connect()` 的**参数名表**（dsn / user / password / host / database）、**无任何一次具体调用**，本卡只走到对象模型与调用顺序这一层（NR-10）；参数化占位符、事务隔离级别、ORM 均未涉及。

## A1 — 应用示例 (Application Example)

### 示例 1: 建表、插一条、立刻查回来（原书未提供案例）
- **类型**: 合成演练（按原书规则构造，非作者案例）
- **问题**: 用 SQLite 建一个 user 表，插入一条记录并立刻查回来
- **怎么用**: 按书中顺序推演——①`connect()` 取连接对象 → ②`cursor()` 取游标 → ③游标 `execute(insert 语句)` → ④连接对象 `commit()` → ⑤游标 `execute(select 语句)` → ⑥`fetchone()` → ⑦`close()`。
- **结果**: 书中未说明运行结果。**演练到对象模型层为止**：书中只有 connect() 的参数名表、无具体调用，故不写参数值与占位符（NR-10）。

## A2 — 触发场景 (Future Trigger) ★

### 用户会在什么情境下需要这张卡?
1. 要把程序里的数据存进数据库，或从数据库里查出来
2. 拿到一段数据库代码，不确定某句该发给连接对象还是游标
3. 改了数据却不生效，怀疑漏了提交
4. 查询结果取不回来，或只取到了第一条

### 语言信号
- 「怎么连数据库 / 怎么建表」「cursor 是什么」
- 「连接对象和游标对象有什么区别」
- 「insert / select / update / delete 怎么写」
- 英文: "connect to database", "cursor", "fetch results"

### 与相邻能力的区分（阶段 3 定稿）
- 与 `module-import-and-pip` 的区别: 那条解决「驱动模块从哪来、是不是标准库、要不要 pip 装、import 怎么写」；本卡从「已经能拿到 connect()」开始讲会话、游标与执行顺序。按 depends-on：连库前先走那条卡引入对应的数据库模块。

## E — 可执行步骤 (Execution)

1. **取连接对象与游标**
   - 完成标准: 写出「用 connect() 得到连接对象」「用 `cursor()` 取得游标对象」，并说明参数随数据库类型不同、常用参数名有 dsn / user / password / host / database（**只给参数名，不给具体值**）、「执行 SQL 的是游标」
   - 判停条件: 用户要「一行能直接跑的连接代码」时，如实说明本书未覆盖（NR-10）；答不出驱动模块名则转 `module-import-and-pip`

2. **执行语句**
   - 完成标准: 用四类模板之一构造 SQL（insert / select / update / delete，可带 where），交给游标的 `execute(operation[, parameters])`；批量场景用 `executemany(operation, seq_of_params)`
   - 判停条件: 用户要求参数化 / 占位符写法时，说明本书未涉及，不编造占位符

3. **提交或回滚、取结果与自检（改数据时必须提交）**
   - 完成标准: 改数据之后调用连接对象的 `commit()`（出错用 `rollback()`）；从游标取结果——`fetchone()` 下一条 / `fetchmany(size)` 指定数量 / `fetchall()` 全部（书中明写通常用这 3 种方式）；用 `close()` 收尾
   - 判停条件: 事务类方法必须落在连接对象上、执行与取结果必须落在游标上；出现具体连接参数值或占位符即删除

**输入契约**: 必需 = ①数据库类型（决定驱动模块与 connect() 的参数口径）；②要做的动作（增 / 查 / 改 / 删）；③表名与字段名（答「还不知道」时先给出四类模板的形状）。缺 ① 不得假设某个数据库。

**输出契约**: 调用序列（连接 → 游标 → execute → commit / rollback → fetch → close）+ 职责归属（连接对象管会话与事务，游标管执行与取数）。完成标准：每一步都能回指 g20 / g21 或 p29 / p30 / p40。

## B — 边界 (Boundary) ★

### 不要在以下情况使用这张卡
- 需要可复制即跑的连接代码——本书未覆盖（NR-10）
- 需要参数化查询 / 防注入写法、事务隔离级别、连接池或 ORM——本书未覆盖

### 作者在书中警告的失败模式
- 把 SQL 发给连接对象: 书中的划分是「cursor() 获取游标对象，操作数据库，如执行DML操作」——连接对象的四个方法里没有执行语句的入口（p29 / g20）
- 改完不提交: 方法表里 commit()「提交事务」、rollback()「回滚事务」——改数据后要走事务收尾
- 取结果的方式与需求不匹配: f30 明写三种取法各自的语义（下一条 / 指定数量 / 全部），本该取全部却用 fetchone() 只会拿到一条

### 作者盲点与底本缺口
- **底本缺口 NR-10**: `connect()` 只有参数名表（dsn / user / password / host / database）、**无任何一次具体调用**——故本卡不给参数值与可运行示例，演练只到对象模型层
- **本书未覆盖**参数化查询、事务隔离级别、ORM（单元 v16 note）；驱动模块安装归 `module-import-and-pip`
- 底本缺陷: 两张方法表被压成单列且顺序交错（引文里的「说明 closeO」「关闭数据库连接 commitO」即为原样），对应关系需按语义核对（NR-15）

### 容易混淆的邻近方法论
- `module-import-and-pip`: 产物是「模块名 + pip 命令」，本卡是调用序列

---

## 相关能力（阶段 3 定稿）

- depends-on: `module-import-and-pip`（连库前先 pip 装驱动，并判断是不是标准库）

---

## 审计信息

- **验证通过**: V1 ✓ / V2 ✓ / V3 ✓（阶段 1.5，单元 v16）；router（判据 3 不通过：无一次具体 connect() 调用，NR-10）
- **证据**: 候选 7 条；ck-931b9ea0da82 ｜ **蒸馏时间**: 2026-09-17
