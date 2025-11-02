> python有个环境概念, 还不错, 可以解决各种依赖冲突问题

### VS code自动了

* 在vscode的terminal中安装python的库ipykernel时, 自动建议弄个环境:
  * /Users/bergman/my2024/.venv/bin/python

### copilot建议这么安装库

```sh
# 设置环境
source /Users/bergman/my2024/.venv/bin/activate   

# 如果还没安装socks依赖, 那么要使用http代理, 例如.zshrc里面的别名
alias httpon="export all_proxy=http://127.0.0.1:7890"

# 安装socks依赖, 如果翻墙用的socks那么要装这个
pip install PySocks 

# 升级pip, 这个很重要, 最新的环境需要最新的pip
pip install --upgrade pip  

# 安装ipykernel, vscode执行ipynb需要这个
pip install ipykernel
```

#### 2025.3.29更新, 以flet为例

```sh
# 搭建虚拟环境, 在本地建了一个 .myvenv目录
python3 -m venv .myvenv
source .myvenv/bin/activate
# 参数扩展：
--system-site-packages：允许虚拟环境访问系统全局安装的包 
--prompt：自定义虚拟环境提示符名称，例如 python -m venv --prompt=projectA myenv 

# 使用pip在虚拟环境安装包, 此时已经不需要pip3了, 因为是虚拟环境, pip就是pip3
pip install 'flet[all]'

# 退出虚拟环境
deactivate

# 删除虚拟环境: 直接删除文件夹即可
# ! 上面这句是胡说八道 ------2025.3.30再次更新------
# 查看虚拟环境
echo $VIRTUAL_ENV
deactivate
code ~/.zshrc
搜索: flet_env
# 俺没有找到, 只能下面继续
code ~/.zprofile
code ~/.zshenv
# 俺依旧没有找到, 只能刷新看看
exec zsh
# 再看看个人根目录下面是否有这玩意
ls ~/flet_env
# 全局搜索都没有, 只能最后一招, 重启试试了
# 最终deepseek给出了正确答案. 
禁用vscode的python相关插件, 然后重启vscode再打开插件, 以便清楚插件的缓存

# conda相关操作:
# 创建环境
conda create --name myenv python=3.9

# 激活
conda activate myenv

# 退出
conda deactivate


```

###### 2025.4.4更新 vscode的py虚拟环境

> vscode会错误识别虚拟环境, 以致于弹出警报:全局安装, 
> 然后就对着代码报警: 无法解析导入“PySide6.QtWidgets”,  对应代码: from PySide6.QtWidgets import QApplication, QMainWindow, QTextEdit

```sh
# 虚拟环境中查看解释器版本
which python  # macOS/Linux

# vscode中选择虚拟环境
cmd + shift + p  -> python: select interpreter(解释器)
# 如果虚拟环境没有出现, 那么是因为根目录虚拟环境所在的目录不是vscode的根目录

# 此时会发现terminal不需要source 虚拟环境, vscode自动搞好了
# 验证下都安装了哪些库
pip list

# 安装了微软的python environments插件后, 左边出现四叶草状蛇缠绕图标, 点开就可以直接看到各种python信息
```



###### 2025.5.18 后记

> 新建虚拟环境时, 千万不要图省事用默认名字, 那玩意用一段时间之后, python自己就混乱了.

```sh
# 此时, 我们只能删除重装.
deactivate
# 一定要反复运行上面这个, 完全确认退出了环境.
# 然后新建环境时, 千万不要用vscode默认的新建, 那个玩意就是建.venv的根源.


python3 -m venv .myvenv
source .myvenv/bin/activate
```

> 后记: vscode很多时候拥有特别顽强的记忆, deactivate之后, 再次开新的terminal, 老的虚拟环境会再次出现, 即便此时, 你已经删除了它, 那么此时, 可以再次deactivate, 然后建个新的, 新的就会生效.



###### 2025.6.28后记

> ipynb比较特殊

ai说: Jupyter Notebook 的内核（kernel）列表只会显示已被 Jupyter 注册过的 Python 环境。如果你的虚拟环境没有出现在 kernel 选择列表，需要先在该虚拟环境下安装 `ipykernel`，然后注册为 kernel：

```sh
source ../venv/bin/activate
pip install ipykernel
python -m ipykernel install --user --name venv --display-name "venv"

```



###### 2025.10.13后记

* python的虚拟环境设置, 不止在.vscode/setting.json中
* 还有.env, 并且它的权限更高
* 因此存在一种情况, setting.json设置是错的, 但是py运行正常, 因为.env是正确的

```json
//setting.json最少需要这两行, 特别注意路径要正确, 如果不对, 保存这个json配置时vscode会报错
  "python.defaultInterpreterPath": "${workspaceFolder}/../../../.porjectvenv/bin/python",
  "python.terminal.activateEnvironment": true,
```



###### 2025.10.24后记

### vscode

* 这玩意有点讨厌, 他要登录github, 然后跳转浏览器登录. 登录之后再跳回vscode
* 我打开了设置同步, 然后选了使用remote, 但是, 他依旧没有通不过来.
* 至少要装这几个扩展
  * python 微软
  * pylance 微软
  * pylint 微软 据说可以不装....但是我装了
  * ruff 发布者: astral, 标识符: charliermarsh.ruff

```sh
python3 --version
# 系统预装了python
brew install python
# 更新一把
# brew安装之后位置:
/opt/homebrew/bin  # 这里时执行文件, 没有python3, 只有python3.14
/opt/homebrew/opt/  # 这里是安装文件, 很多python文件夹

# 此时我要配合pyside6, nuitka, 而python恰好此时升级了3.14. 所以我选择3.13, 保持稳定行.
# 但是, 详细看了pyside和nuitka文档, 最稳当的选择是3.12, 肯定没有任何兼容性问题. 
brew install python@3.12

# 验证下安装的正确性
python3.12 --version

# 创建目录名为 .pysidenv 的虚拟环境, 用点是因为要默认隐藏, 避免误操作
python3.12 -m venv /Users/bergman/.pysidenv

# 激活虚拟环境：
source /Users/bergman/.pysidenv/bin/activate

# 想要删除虚拟环境, 一定要先退出虚拟环境, 一定要确认是退出了环境, 可以反复运行下面这个命令
deactivate

```

### vscode中选择虚拟环境

1. 修改setting.json, 
   1. 便捷选择: vscode的右下角会要求你选择解释器.
   2. 手动选择: cmd + shift + p  -> python: select interpreter(解释器)
2. 修改.env, 这个优先级更高. 如果环境不对可以看看是否这个存在, 可以删了他.

```json
//setting.json最少需要这两行, 特别注意路径要正确, 如果不对, 保存这个json配置时vscode会报错
  "python.defaultInterpreterPath": "/Users/bergman/.pysidenv/bin/python",
  "python.terminal.activateEnvironment": true,

//下面这两个目录没必要设置, 因为这两个是默认的搜索目录.
  "python.analysis.extraPaths": [
    //"${workspaceFolder}",
    //"/Users/bergman/.pysidenv/lib/python*/site-packages"
  ],
```

```sh
# 然后平平无奇的安装包
pip install pyside6
pip install Nuitka
```

### 2025-10-29 后记

1. 真牛逼, macos升级之后, 会破坏nuitka的编译, 原因是破坏了brew安装的python, 所以根源是brew

```sh

# 升级后 CLT 往往被系统清掉
xcode-select --install        # 弹窗安装
sudo xcode-select -s /Library/Developer/CommandLineTools # 这一行可能报错, 我忽略了.

# 下面这些可能会失败, 打开系统代理可以解决问题.
# 把 Homebrew 仓库回退到与当前系统匹配的干净状态
brew update-reset

# 再升级一次 formulae
brew update && brew upgrade

# 自检并按提示修复
brew doctor

# 升级或者重新安装
pip install -U nuitka  # 升级
brew reinstall python # 重装
# 一般而言, 此时OK了.

# 还是不行, 最后只能清理brew, 重新安装
brew cleanup -s
# 这个是删除, 之前要which一下看看在哪里
sudo rm -rf ~/Library/Caches/Homebrew

```

2. python有universal概念, 在macos用这个版本的python就可以打包同时兼容intel和m的包

```sh
1. 重装/切换 universal Python
# 从python官网下载. 默认就是universal, 然后安装.
# 安装最后有个证书操作, 此时要关闭系统代理和terminal代理.
# 然后用新安装的python打包
which python3                                                 
/Library/Frameworks/Python.framework/Versions/3.12/bin/python3
# 升级证书操作
/Library/Frameworks/Python.framework/Versions/3.12/bin/python3 -m pip install --upgrade certifi
python3 -m pip install --upgrade certifi
# 然后安装包就会出错, 需要手动改.zshrc
export SSL_CERT_FILE=$(python3 -m certifi)

# 退出之前的虚拟环境.
deactivate

# 创建目录名为 .qtuniversal 的虚拟环境, 用点是因为要默认隐藏, 避免误操作
python3 -m venv /Users/bergman/.qtuniversal

# 激活虚拟环境：
source /Users/bergman/.qtuniversal/bin/activate

# 设置vscode的setting
  "python.defaultInterpreterPath": "/Users/bergman/.pysidenv/bin/python",

# 此时可以验证一下:
file $(python -c "import sys; print(sys.executable)")

# 然后安装
pip install pyside6, Nuitka



# 最后nuitka打包, 最关键是这行:--macos-target-arch=universal

python -m nuitka \
  --mode=app \
  --macos-target-arch=universal \
  --enable-plugin=pyside6 \
  pig.py
```



3. 手动安装python可能有问题



```sh


# 我之前从python官网下载安装的python
which python3 
/Library/Frameworks/Python.framework/Versions/3.12/bin/python3
# 但是, 我发现这个python还需要安装证书, 还需要修改.zshrc:    
export SSL_CERT_FILE=$(python3 -m certifi)
# 这样是不是就和我从brew安装的python冲突了? 是不是应该卸载这个手动安装的python?
# ai建议, 卸载掉他


sudo rm -rf /Library/Frameworks/Python.framework
sudo rm -rf /Applications/Python\ 3.12
sudo mv /usr/local/bin/python3.12 /usr/local/bin/python3.12.bak
```

