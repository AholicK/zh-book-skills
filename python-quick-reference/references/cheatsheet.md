# 决策规则速查 — 《Python背记手册》

| 能力 | 一句话规则 |
|---|---|
| 分支与循环控制（if / if…else / if…elif…else / while / for / break / continue） | 条件已知用 while、对象已知用 for；else 必须与 if 成对；break 中断整个循环、continue 只结束本轮；保留字不能当名字且区分大小写。 |
| 序列切片（列表 / 元组 / 字符串同形的 start : end : step） | 切片只用一套 start : end : step——start 默认 0 且含该位、end 默认到末尾且不含该位、step 默认 1；切片生成新序列，原序列不变。 |
| 列表与元组的创建、遍历与统计（append/extend/enumerate/count/index/sum/内置函数速查） | 追加一个对象用 append、拼一整个列表用 extend；count/index 只能精确匹配；sum 的 start 默认 0，要加基数就写进第二个参数。 |
| 列表排序的两条路（sort 改原列表 / sorted 不改原列表，含 key 与 reverse） | 排序前先问一句「原顺序还要不要用」——要，就用 sorted（返回排序结果、原列表不变）；不要或明确要就地排，才用 sort。 |
| 列表推导式三形态（range 生成 / 列表映射 / 条件筛选） | 要一行生成新列表时用 `[Expression for var in range]` 或 `[Expression for var in list]`，加 `if condition` 即筛选；原列表与 range 都不被改动。 |
| 字典与集合（创建 / 键与元素约束 / items 遍历 / 增删） | 字典键必须唯一且不可变（字符串/数字/元组）、值不限；遍历先用 items() 拿（键-值对）元组列表再 for 取；集合由字面量或 set(iteration) 创建，元素只能用字符串/数字/布尔值。 |
| 字符串编解码与拆分合并（encode/decode + errors 策略、split/join） | 编解码默认 UTF-8 且 errors 默认 strict（要容错必须显式改）；split 的 sep 默认 None 即所有空白符；join 的调用者才是分隔符。 |
| 字符串格式化（% 字符表与 format() 字符表） | 按类型挑字符用 % 表（%d/%f/%s/%r/%c/%e，%% 表示百分号，浮点默认 6 位小数）；要进制转换或百分比用 format() 的 b/o/x/X。 |
| 运算符与优先级（求值顺序、复合赋值展开、布尔值口径） | 表达式按「** → 取反/正负号 → 算术 */%// → 算术 +- → 位移 → 位运算 → 比较」逐层求值，复合赋值先展开成 x = x op y，比较与逻辑运算的结果都是 True / False。 |
| 输入与输出（input 接收键盘输入、print 输出结果） | 与用户交互只需两行——变量 = input("提示文字") 收输入、print(输出内容) 打结果；输出内容可以是数字、引号括起的字符串或含运算符的表达式。 |
| 函数定义与调用（def / 默认值参数 / return / lambda） | 一段逻辑要复用就写成 def 函数名(参数列表) 加 return，调用写 函数名(实参)；常用取值设成 参数=默认值，临时小函数用 lambda 参数 比表达式（expression 必选）。 |
| 类与对象（class 定义 / 实例方法与 self / 计算属性 / 继承与重写） | 把数据与行为打包成类——class 类名 加类体（不能空，未定用 pass），实例方法第一个参数必须是 self，实例化后 实例.方法() 调用；复用走 class 子类(基类) 并在派生类里重写同名方法。 |
| 模块导入与包管理（import / from…import、标准库速查、pip 安装卸载） | 先查标准模块表判断「是不是自带的」：标准库直接 import（两种形式、可起别名或用 `*`），第三方才走 pip，且 install/uninstall 必须带模块名，模块名区分大小写。 |
| 异常处理与调试（try…except…finally / raise / assert / 常见异常对照） | 出错时先分清是可预期分支还是运行期错误：能就地收拾用 try…except…finally（finally 必写），本层不该处理的用 raise 上抛，验证「自己以为必然成立」的假设用 assert，按异常名对照表定位错误类型。 |
| 文件及目录操作（open/close/write、os.remove/rename、stat 属性） | 操作文件三步：open() 打开（默认只读，写必须显式给可写模式）→ file.write(string) 写 → file.close() 关；删文件没有内置函数，走 os.remove(path)；大小看 os.stat() 的 st_size（单位字节）。 |
| 数据库访问（连接对象 / 游标对象 / SQL 四类语句 / fetch 三法） | 数据库访问先分清两层对象：连接对象管会话与事务（close / commit / rollback / cursor），游标管执行与取数（execute / executemany / fetchone / fetchmany / fetchall）；顺序是连接 → 游标 → 执行 → 提交 → 取数 → 关闭。 |
| 进程与线程（Process / Thread 同构参数、Lock 加解锁） | Process 与 Thread 的参数表同形（group/target/name/args/kwargs）；共享数据用一把 Lock 在同一执行体内 acquire→改→release；本书不给 start()/join()。 |
| Socket 网络通信的基本方法序列 | 服务端 bind → listen(backlog≥1，常用 5) → accept（阻塞）→ recv/send → close；客户端 connect((hostname, port))（出错返回 socket.error）→ 收发 → close；顺序颠倒就没有可接受连接的监听者。 |
| 正则表达式构造（元字符 / 限定符 / 标志） | 正则按三层拼——元字符管「匹配什么」、限定符管「重复几次」、标志管「什么行为」；先定字符、再定次数、最后加标志，顺序反了最容易写错。 |
| GUI 窗口与控件构造（wxPython：wx.Frame / StaticText / TextCtrl / Button / sizer 选型） | 窗口 = 构造器 + 参数：wx.Frame(parent, id, title, pos, size, style, name)；控件共用 parent/id/pos/size/style/name；布局按尺寸是否一致在 GridSizer/FlexGridSizer/GridBagSizer/BoxSizer 里选；本书不给主循环与事件。 |

> 速查只给结论；需要原文依据、案例或反例时读对应能力卡。
