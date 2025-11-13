> vscode拥有强大的功能. 但是, 之前却没有用到, 这些年都用了个寂寞. 呵呵呵, 一直在用假的vscode....

###### 代码跳转

  * 跳转到定义：`Cmd + 单击`（类/方法名）
* **返回调用处**：`Ctrl + -`（Control + 减号）
* **符号搜索**：`Cmd + T` 全局符号搜索

###### 查看api

* **悬停提示**：鼠标悬停在类/方法上显示文档, 悬停可以查看一条极简介绍.
* 内联提示:
  * 鼠标右键: qt doc
  * 需要: 
    * 安装qt python扩展
    * // settings.json
      "qtForPython.documentation": "PySide6"



###### 附加python帮助

  * help(QMainWindow)
  * print(dir(QMainWindow))



###### 20251024 补充

最终使用这些工具:

* python 微软, 基础包, 语法高亮, 环境, jupyter, 调试
* pylance 微软, 语言服务器, 自动补全, 类型检查, 跳转定义, 引用查找, intellisense(智能感知)
* pylint 微软 代码规范,  导入的import被本地变量/函数覆盖, 唯有这个可以报出来: 2025-08-13-python重定义外部变量
* ruff 发布者: astral, 标识符: charliermarsh.ruff, 极速linter+formatter

### 20251104

ruff 也可以报告变量遮盖问题

```toml
[tool.ruff]
fixable = ["F401", "I001"] # 自动修正, 必须被select包含       
ignore = ["E", "W", "RUF003", "RUF052"] # 忽略得规则       
indent-width = 2 
line-length = 1200 # 允许长行
# 会报出来                 
select = ["A","F", "I", "B", "UP", "T", "RUF", # 这里的A就是变量遮盖报警
    "E711", "E712", "E721", "E731",      # 逻辑/可读性
    "E117", # 过度缩进
    "E101",      # 混用tab
    "E741", "E742", "E743",  # 易混淆变量名
    "SIM","C90", "ARG", "PIE",
] 
preview = true
[tool.ruff.format] # 纯粹美化排版
quote-style = "preserve" # 单引号
skip-magic-trailing-comma = true # 不因为尾随逗号就展开
```

```sh
# 查看自己缺少哪些
ruff rule --all | awk -F'[– ]' '/^[A-Z]/ {print $1}' | sort -u | comm -23 - <(\
  python -c "import tomli,sys;print('\n'.join(sys.argv[1:]))" A F I B UP T RUF C90 D ERA FURB G N PTH RET SIM TCH TRY \
  | sort -u)

# 查看规则是否冲突
ruff check --preview --select=A,F,I,B,UP,T,RUF,SIM,C90,ARG,PIE,E117,E101,E711,E712,E721,E731,E741,E742,E743 .
```

| 前缀     | 说明                      | 典型规则举例                                        |
| -------- | ------------------------- | --------------------------------------------------- |
| **C90**  | 圈复杂度 > 10 告警        | `C901`                                              |
| **D**    | pydocstyle 文档字符串规范 | `D100` ~ `D417`                                     |
| **ERA**  | 注释掉的无用代码          | `ERA001`                                            |
| **FURB** | refurb 性能/可读性提示    | `FURB113` 用 `Path.read_text` 代替 `open`           |
| **G**    | flake8-logging-format     | `G001` 日志里 `%` 格式化                            |
| **N**    | pep8 命名规范             | `N801` 类名驼峰、`N806` 小写函数里用 CamelCase 变量 |
| **PTH**  | flake8-use-pathlib        | `PTH123` 用 `Path.open` 代替 `open`                 |
| **RET**  | flake8-return             | `RET504` 没必要先赋值再 return                      |
| **SIM**  | flake8-simplify           | `SIM118` 用 `key in dict` 代替 `key in dict.keys()` |
| **TCH**  | flake8-type-checking      | `TCH003` 延迟导入，解决循环依赖                     |
| **TRY**  | flake8-try-except         | `TRY004` 用 `ValueError` 而不是宽泛的 `Exception`   |

| 前缀    | 说明                                                         |
| ------- | ------------------------------------------------------------ |
| **ANN** | flake8-annotations，强制函数签名写类型注解                   |
| **ARG** | flake8-unused-arguments，函数签名里没用到的参数              |
| **COM** | flake8-commas，尾随逗号风格统一                              |
| **DTZ** | flake8-datetimezone，强制 `datetime.now(tz=)`                |
| **EM**  | flake8-errmsg，异常信息必须写成 `raise ValueError("...")` 而不是 f-string |
| **ICN** | flake8-import-conventions，强制 `import numpy as np`         |
| **INP** | flake8-no-pep420，不允许隐式 namespace package               |
| **PIE** | flake8-pie，各种 Python 语法糖简化                           |
| **PL**  | pylint 等价规则（ruff 移植版），**量大且慢**，可挑着开，例如 `PLR0913` 参数过多 |
| **PT**  | flake8-pytest-style，pytest 风格                             |
| **Q**   | flake8-quotes，引号风格（你用了 `preserve`，可不开）         |
| **S**   | flake8-bandit，安全漏洞，如 `S311 random.random` 用于密码    |
| **SLF** | flake8-self，私有属性访问检查                                |
| **TID** | flake8-tidy-imports，禁止相对导入、禁止 import 被撤包的模块  |
| **YTT** | flake8-2020，不要把 `sys.version[0]` 当成大版本              |



### setting.json样例

* 在oh-my-mac项目: 
  * 项目地址: git@github.com:lornally/oh-my-mac.git
  * 安装列表->python里面
