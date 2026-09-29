<!-- capability_id: cap.automate-boring-stuff.time-and-scheduling | revision: 1 | status: active -->
<!-- 来源: 单元 v14；归并候选 14 条（f50/f51/f52/f53/f68/p67/p68/g28/c31/c32/c33/c34/ce30/ce31）；14 条候选；promoted；含 ce30/ce31 与 NR-03（strftime 表缺失） -->

# 时间、计划任务与启动程序（time/datetime/timedelta、多线程、subprocess 与调度器）

## R — 原文 (Reading)

> 在程序的所有线程终止之前，Python 程序不会终止。
> ……
> threading 模块用于创建多个线程……要确保线程只读写局部变量，否则可能会遇到并发问题。
>
> — 第15章 · 保持时间、计划任务和启动程序「15.6 多线程」L10014-10070

> 如果你精通计算机，可能知道 Windows 上的 Task Scheduler，OS X 上的 launchd，或Linux 上的cron 调度程序。这些工具文档齐全，而且可靠……利用操作系统内置的调度程序，你不必自己写时钟检查代码来安排你的程序。
>
> — 第15章 · 保持时间、计划任务和启动程序「15.8 从Python 启动其他程序」L10190-10288

## I — 方法论骨架 (Interpretation)

这张卡解决「脚本怎么脱离手动运行」。先分三类时间值再选函数：Unix 时间戳（`time.time()` 返回浮点，1970-01-01 00:00 UTC 起秒数）、datetime 对象（一个时刻，含年月日时分秒属性）、timedelta 对象（一段时间，无 month/year 参数）。判据：只关心「过了多久/暂停」用时间戳与 timedelta；要显示或比较日期用 datetime。

测流逝时间 = 两次 `time.time()` 相减，结果用 round() 化短。`time.sleep(秒)` 是阻塞调用；IDLE 里 Ctrl-C 中断不了长 sleep，长等待要拆成短睡眠循环（ce30）。「等到某时刻」写成 `while datetime.datetime.now() < 目标: time.sleep(1)`（每秒查一次）。

并发三步：`threading.Thread(target=函数名)`（不加括号）→ `start()` → 需要等就 `join()`；所有线程终止前程序不退出。纪律：目标函数只用局部变量——同时读写同一变量会互相干扰，难重现难调试。

定时运行不自己写时钟循环：交给操作系统调度器（Task Scheduler / launchd / cron）。

## A1 — 书中的应用 (Past Application)

### 案例 1: 超级秒表（stopwatch.py）
- **类型**: 书中项目（第15章）
- **问题**: 想量化枯燥任务花的时间，找不到无广告免费秒表
- **方法论的使用**: input() 等回车开始 → startTime = time.time()；每圈用 round(time.time() - lastTime, 2) 算单圈与总时长打印；捕捉 KeyboardInterrupt 让 Ctrl-C 正常退出
- **结论/结果**: 未说明（只说明可自己写一个简单秒表程序）

### 案例 2: 多线程 XKCD 下载（multidownloadXkcd.py）
- **类型**: 书中项目（第15章）
- **问题**: 单线程下载一次只下一幅，大部分时间在等网络与写盘
- **方法论的使用**: 下载逻辑封成 downloadXkcd(start, end)；按页码范围建多个 Thread(target=downloadXkcd, args=(start,end)) 并 start()；对每个线程 join() 后才打印 Done.
- **结论**: 多线程让下载与建连接/写盘同时进行，更有效利用网络连接
- **结果**: 所有 join() 返回后才打印 'Done.'；未给完整输出样本

## A2 — 触发场景 (Future Trigger) ★

### 用户会在什么情境下需要这张卡?

1. 「让脚本每天（或每周）定时自动跑」——备份、报表、提醒
2. 要在脚本里加计时/倒计时/限速等待
3. 「怎么让两个任务同时跑」/ 批量下载太慢想并发
4. 脚本要与别的程序交接：启动外部程序、用默认应用打开文件
5. 长等待脚本按 Ctrl-C 退不出来

### 语言信号

- 「每天 9 点自动跑」「定时执行」「怎么让脚本自己跑」
- 「写个倒计时 / 计时器 / 秒表」
- 「怎么让两个任务同时跑」「下载能不能并行」
- 英文: "run daily at 9am", "schedule a script", "countdown", "run in parallel"

### 与相邻能力的区分（阶段 3 定稿）

- 与 `organize-files` 的区别: 本卡给「定时器与执行方式」；被定时的任务（备份、整理等）由那条卡负责
- 与 `web-scraping` 的区别: 并发下载是本卡 threading 口径；下载与解析内容走那条卡
- 与 `email-and-sms` 的区别: 本卡管「到点跑」，那条卡管「把结果通知出去」；两者串成自动提醒

## E — 可执行步骤 (Execution)

1. **选时间机制（先分清三类值）**
   - 完成标准: 判定属于「等一会儿 / 某一时刻做 / 测时长」哪类，给出对应对象（时间戳/datetime/timedelta）
   - 判停条件: 「每天固定时刻自动跑」→ 直接走第 4 步，不在脚本里写时钟循环
2. **写等待/计时**
   - 完成标准: sleep 用 time.sleep(秒)；测时长用两次 time.time() 相减并 round()；长睡眠拆成短睡眠循环（ce30）
3. **要并发时按三步改**
   - 完成标准: Thread(target=函数名)（不加括号）+ start() + join()；目标函数只用局部变量；检查 target 不带括号、不读写函数外变量
   - 判停条件: 需要锁/共享状态 → 书中未覆盖，停下并降级为串行或分文件
4. **定时交给调度器**
   - 完成标准: 给出「用操作系统调度器（Task Scheduler / launchd / cron）」口径并注明细节见各系统文档；循环等某时刻写成 while datetime.datetime.now() < 目标: time.sleep(1)
5. **启动其他程序**
   - 完成标准: subprocess.Popen([程序, 参数...])；用 poll()/wait() 查状态

**输入契约**: 必需 = ①任务；②时间语义（立即/延时/某时刻/周期）；③运行环境（是否 IDLE、各系统）；④是否要并发。缺 ② 先问；缺 ③ 按不写死平台处理。

**输出契约**: 代码 + 调度方案 + 选型理由；调用可回指 f50/f51/f52/f53/f68/p67/p68。

## B — 边界 (Boundary) ★

### 不要在以下情况使用这张卡

- 需要毫秒级实时控制或高频轮询（sleep 精度未覆盖）；需要锁、队列等同步原语（书中承认多线程超出本书范围）
- 需要跨时区/夏令时的日期计算（书中未展开）；需要调度器分步教程时细节见系统文档

### 作者在书中警告的失败模式

- IDLE 里 Ctrl-C 中断不了长 sleep（ce30）：不要用一次 time.sleep(30)，改 for 循环 30 次 time.sleep(1)
- target= 写成函数调用（ce31）：「关键字参数是target=takeANap，而不是target=takeANap()」——加括号会立刻执行且新线程什么都没做
- 多线程共享变量（ce31）：互相干扰且「很难一致地重现，所以难以调试」；纪律是目标函数只用局部变量
- 主线程先结束程序不退出：「在程序的所有线程终止之前，Python 程序不会终止」，要等就 join()

### 作者盲点与底本缺口

- **书内明确外包（unit_note）**：调度器完整步骤书中不提供，细节见系统文档；多线程只讲最基础用法，ce31 即边界
- **NR-03**：strftime 指令表在源稿中不存在；Box 元组四整数顺序被截断——不得自行补齐，需要时指向 Python 官方文档 / Pillow 官方文档（外部资料）
- **书中未覆盖**：时区/夏令时、sleep 精度、锁；底本为 OCR 稿，引文照抄

### 容易混淆的邻近方法论

- `email-and-sms`：到点执行 vs 通知出口，两者串联构成定时提醒

---

## 相关能力（阶段 3 定稿）

- composes-with: `organize-files`（备份 + 调度器 = 定时自动备份）
- composes-with: `web-scraping`（多线程下载复用抓取脚本）
- composes-with: `email-and-sms`（定时 + 通知 = 到点自动提醒）

---

## 审计信息

- **验证通过**: V1 ✓ / V2 ✓ / V3 ✓（阶段 1.5，单元 v14）
- **证据**: 候选 14 条（f50/f51/f52/f53/f68/p67/p68/g28/c31/c32/c33/c34/ce30/ce31）；ck-dec527ee9a2c / ck-96b6b91ab3fb / ck-b0f36272060d
- **去向**: 阶段 1.6 promoted 独立入口（排序第 7）
- **蒸馏时间**: 2026-09-18（分片 B）
