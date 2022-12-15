> 才发现我的使用方式是错误的



### 使用命令行
> 不能使用control pannel那个图形界面, 那个界面巨慢
```sh
# 在工作目录
ipython notebook

# ~/.zshrc 设置alias
alias pn="ipython notebook"


# 设置工作目录
alias pnn="jupyter notebook --notebook-dir=/Users/machangkun/Documents/__ipython_notebook"
# 也可以走设置流程
# 1查看设置目录
open ~/.jupyter 
# 产生配置文件
jupyter notebook --generate-config

# 编辑
mate ~/.jupyter/jupyter_notebook_config.py
# 为了保持灵活性, 暂时不配置默认目录, 因为, 这样的话某个目录执行notebook就不是那么方便了

```

> 参考: https://stackoverflow.com/questions/15680463/change-ipython-jupyter-notebook-working-directory


### python 和 anaconda的shortcuts

- https://docs.conda.io/projects/conda/en/4.6.0/_downloads/52a95608c49671267e40c689e0bc00ca/conda-cheatsheet.pdf
- https://know.continuum.io/rs/387-XNW-688/images/conda-cheatsheet.pdf
- https://docs.anaconda.com/anaconda/user-guide/cheatsheet/
- https://www.pythoncheatsheet.org/

### 配置缩进

- 编辑文件: ~/.jupyter/nbconfig/notebook.json

