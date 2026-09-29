<!-- capability_id: cap.python-quick-reference.socket-communication | revision: 1 | status: active -->
<!-- 来源: 单元 v18；归并候选 2 条（f33, p35） -->

# Socket 网络通信的基本方法序列

## R — 原文 (Reading)

> 被动接受TCP客户端连接,(阻塞式)等待连接的到来 s.acceptO
主动初始化TCP服务器连接，一般address的格式为元组（hostname,port），如 s.connectO
果连接出错，返回socket.error错误
>
> — 第 19 章 · 网络编程「必背必记 4．Socket 对象的内置方法」L2601-2641

> 表示地址 s.listenO
开始 TCP监听。backlog 指定在拒绝连接之前，操作系统可以挂起的最大连接
数量。该值至少为1，大部分应用程序设为5就可以了
>
> — 第 19 章 · 网络编程「必背必记 4．Socket 对象的内置方法」L2601-2641

## I — 方法论骨架 (Interpretation)

底本给的是**方法表**而不是程序：它回答的是"一次点对点连接里谁先动、谁在等、按什么次序调方法"。

- **服务端序列**：`bind` 把地址绑到套接字 → `listen` 开始 TCP 监听（`backlog` 指定在拒绝连接之前操作系统可以挂起的最大连接数量，书中口径是**该值至少为 1，大部分应用程序设为 5 就可以**）→ `accept` 被动接受 TCP 客户端连接，且是**阻塞式**等待 → `recv`/`send`/`sendall` 收发 → `close`。accept 是"卡住"的那一步：它不返回，就没有连接可以收数据。
- **客户端序列**：`connect((hostname, port))` 主动初始化 TCP 服务器连接（出错返回 `socket.error`）→ `recv`/`send` → `close`。
- **另两个方法**：`recvfrom`/`sendto`（书中只列名称与作用，未给适用场景的取舍口径）。

三条可复算的判据：①服务端必须"先 bind、再 listen、再 accept"，顺序颠倒就没有可接受连接的监听者；②`backlog` 至少 1，常用 5；③收发之前必须已经建立连接（accept 或 connect 的返回）。本卡交付的是**调用顺序**，不交付"能跑起来的程序"——底本没有创建套接字与 import 的写法（NR-12）。

## A1 — 应用示例 (Application Example)

本单元候选里没有原书示例（只有方法表），以下两条都是演练。

### 示例 1: 一个 TCP 服务端从绑定到收数据的方法序列
- **类型**: 合成演练（按原书规则构造，非作者案例）
- **问题**: 服务端要把本地地址暴露出去并收一次数据，方法按什么顺序排
- **怎么用**: `s.bind((host, port))` → `s.listen(5)`（backlog 至少 1，书中说大部分应用程序设为 5 就可以）→ `s.accept()`（阻塞等待连接）→ `s.recv(bufsize)` 取数据 → `s.close()`
- **结果**: 书中未说明（纸面推演）。演练**未编造**套接字创建语句与 import——书中未给

### 示例 2: 客户端连不上时在哪一步判断
- **类型**: 合成演练（按原书规则构造，非作者案例）
- **问题**: 客户端发起连接后，失败要在哪里发现
- **怎么用**: 在 `s.connect((hostname, port))` 这一步判断——书中写明"如果连接出错，返回socket.error错误"；确认成功后再进入 recv/send
- **结果**: 书中未说明（无示例代码；重试与超时口径书中也未给）

## A2 — 触发场景 (Future Trigger) ★

### 用户会在什么情境下需要这张卡?
1. 写 TCP 服务端，不确定 bind／listen／accept 谁先谁后
2. `listen` 的 backlog 该填几
3. `accept` 停住不动，想确认它本来就是阻塞式等待
4. 客户端连接失败时不知道在哪里判断（`socket.error`）
5. 收发的机会名记不清（`recv`/`send`/`sendall`/`recvfrom`/`sendto`）

### 语言信号
- 「服务端要先 bind 还是先 listen」「backlog 写几」
- 「accept 阻塞在这不动」「connect 报 socket.error」
- 英文: "socket bind listen accept order", "listen backlog value", "accept is blocking", "send vs sendall"

### 与相邻能力的区分（阶段 3 定稿）
- 与 `module-import-and-pip` 的区别: 底本没有 `import socket` 与套接字创建语句（NR-12），"怎么把模块引进来、怎么装依赖"走那条能力；本卡只管**拿到套接字对象之后**的调用序列

## E — 可执行步骤 (Execution)

**输入契约**：①哪一侧（服务端/客户端）②地址与端口（服务端绑定用、客户端是连接目标）③收发什么类型的数据。缺②时先问地址与端口，不要替用户编一个。

1. **判定角色** — 完成标准: 明确写出"本次是服务端还是客户端"，以及对应的第一句调用（服务端 bind、客户端 connect）
2. **排服务端序列** — 完成标准: bind → listen(backlog) → accept → recv/send → close 依次给出，`backlog` 取值 ≥1（常用 5）；判停条件: 若为客户端 → 跳到第 3 步
3. **排客户端序列** — 完成标准: connect((hostname, port)) → recv/send → close，并标出连接出错返回 `socket.error` 的判定点
4. **标明阻塞点** — 完成标准: 指出 accept 是阻塞式等待，close 出现在序列末尾
5. **声明边界** — 完成标准: 交付物中不出现 `socket()` 创建语句与 `import socket`（书中未给，NR-12），也不出现协议族/套接字类型参数

**输出契约**：交付物 = 一份带序号的调用序列（每行一个方法 + 关键参数），外加一句"该序列取自书中方法表，创建套接字与 import 不在本书范围"。完成标准：每个方法的先后关系都能在书中方法表找到依据，且不存在"先 accept 后 listen"这类倒置。

## B — 边界 (Boundary) ★

### 不要在以下情况使用这张卡
- 要一段能直接跑的最小 TCP 程序：底本没有套接字创建语句与 `import socket`（NR-12），**本书未覆盖**
- 要选协议族/套接字类型（AF_INET、SOCK_STREAM/SOCK_DGRAM），或要 TCP 与 UDP 的选择口径：书中未讲 UDP 与协议族选择
- 要并发连接、超时、重连：书中未涉及
- 只是引入模块与装库 → `module-import-and-pip`

### 作者在书中警告的失败模式
- `connect` 的失败有原文口径：书中写"如果连接出错，返回socket.error错误"——序列里必须留这个判定点
- `accept` 是阻塞式等待：写代码时若把它当成"立刻返回"，会被卡在那一行
- `backlog` 的口径是"该值至少为1，大部分应用程序设为5就可以了"；越出这个口径的取值书中未讨论

### 作者盲点与底本缺口
- 套接字创建语句与 `import socket` 未出现（NR-12）——**本书未覆盖**，所以本卡只能到方法序列层
- UDP 与协议族选择、并发与超时处理——**本书未覆盖**
- 证据基础仅 2 条候选（f33/p35），是全书最薄的单元之一
- OCR 缺陷（NR-15）：整张方法表被压成单列，方法名与说明交错（如 `s.listenO` 落在说明文字之后），逐条对应由本项目按语义核对

### 容易混淆的邻近方法论
- `module-import-and-pip`: 它负责模块引入与依赖安装，本卡负责套接字对象上的方法顺序；两卡在一次任务里前后相接，但交付物不同

---

## 相关能力（阶段 3 定稿）

- depends-on: `module-import-and-pip`（底本未给 import socket 与 socket() 创建语句，NR-12；引入模块与装第三方库要走该口径）

---

## 审计信息

- **验证通过**: V1 ✓ / V2 ✓ / V3 ✓（阶段 1.5，单元 v18）
- **证据**: 候选 2 条；ck-fdc491d9b81d
- **蒸馏时间**: 2026-09-17
