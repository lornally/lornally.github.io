> 最近发现, python环境出了问题, 引用包, 引用不到
> 仔细观察发现: brew安装了各个版本的python和anaconda

### 原因和症状

- 因为: anaconda只直接支持到python3.9, 因此, 各种包连带着单独安装了python3.10和3.11, 由此导致, python执行的不是一个python, 导致各种运行问题
- 症状: 引用某个包就引用不到了
- 解决思路: 卸载所有的python, 只使用anaconda

### conda升级python3.11

```sh
# 参考https://stackoverflow.com/questions/63216201/how-to-install-python-with-conda
conda create -n py11 python=3.11
# 如果遇到各种下载, md5校验, 字节量校验问题, 重新执行上面这条命令就好
# 此时会报错: not writable paths with conda?
# 解决办法: https://stackoverflow.com/questions/59619442/how-does-one-fix-the-issue-of-not-writable-paths-with-conda
sudo chown -R $USER ~/.conda


# 然后再次安装, 此后会成功, 然后设置当前使用的环境
conda activate py11

# 下面这个并不是脚本, 只是原本的答案中的说明, 意思是: py11是环境名, 我的神啊
py11 - environment name
```

### 此时依旧有问题, 各种调用还是顽强的使用3.9, 需要修改默认环境

```sh
# 修改~/.zshrc
conda activate py11

# 如果按照stackoverflow修改成下面这样, 并不行
source conda activate py11
```

### 简直就涛声依旧

```python
# 参考: https://github.com/jupyter/notebook/issues/1524
# 进入python命令行
python
# 引入sys
import sys
sys.path
# 显示: ['', '/opt/homebrew/anaconda3/envs/py11/lib/python311.zip', '/opt/homebrew/anaconda3/envs/py11/lib/python3.11', '/opt/homebrew/anaconda3/envs/py11/lib/python3.11/lib-dynload', '/opt/homebrew/anaconda3/envs/py11/lib/python3.11/site-packages']
sys.executable
# 显示: '/opt/homebrew/anaconda3/envs/py11/bin/python'
import openai
# 此时正常, 也就是说不是anaconda的问题, 是vscode的问题
```

### 解决vscode环境问题

```sh
# 参考: https://stackoverflow.com/questions/40185437/no-module-named-numpy-visual-studio-code
# 组合键呼出输入框
cmd+shift+p
# 查找
python select interpreter
# 选择你期望的环境, 按照上面设置的情况, 需要选择: p11
```

### apple的m芯片加速

> 参考: https://developer.apple.com/metal/pytorch/

安装anaconda 针对mac/m芯片优化的版本

```sh
curl -O https://repo.anaconda.com/miniconda/Miniconda3-latest-MacOSX-arm64.sh
sh Miniconda3-latest-MacOSX-arm64.sh
# 装好了有提示: If you'd prefer that conda's base environment not be activated on startup,    set the auto_activate_base parameter to false: 
conda config --set auto_activate_base false
```

安装对应的pytorch等等

```sh
conda install pytorch torchvision torchaudio -c pytorch-nightly
```

python测试

```python
import torch
if torch.backends.mps.is_available():
    mps_device = torch.device("mps")
    x = torch.ones(1, device=mps_device)
    print (x)
else:
    print ("MPS device not found.")
```

- 苹果有各种加速, TensorFlow也有加速
  -  https://github.com/orgs/apple/repositories
  - https://huggingface.co/apple
