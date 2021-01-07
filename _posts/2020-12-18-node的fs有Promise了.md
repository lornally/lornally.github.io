> fs终于原生支持promise了. 

###### 使用promise, node自身就提供了

```coffeescript
import fs from 'fs/promises'
# 追加文件内容, 保证追加内容不重复
export uniqueadd=({path, reg, str, options=encoding:'utf8'}) ->
  # 读取文件, options=encoding:'utf8'保证了读到的是str
  fstr=await fs.readFile(realpath, options)
  # 正则判断
  return if reg.test fstr
  # 不重复就插入
  await fs.appendFile realpath, str ? reg.source, options
```

###### home文件夹报错

```sh
Error: ENOENT: no such file or directory, open '~/.zshrc'
```

```coffeescript
# 因为node不认~
import os from 'os'
home = os.homedir()
realpath =if /^~/.test path then path.replace /^~/, home  else path
fstr=await fs.readFile(realpath, options)
```

###### 判断文件夹/文件存在

```coffeescript
import fts from 'fs'
exist = ({path})->
	fts.existsSync path
```

###### 判断是文件夹/文件

```coffeescript
import fs from 'fs/promises'
filetype = ({path, options})->
	stat=await fs.lstat path, options #lstat才能判断符号链接
	{
		isfile: stat.isFile()
		isdir: stat.isDirectory()
	}
```

###### 新建文件/文件夹, 写入, 添加写入

```coffeescript
#新建文件夹
await fs.mkdir path
#新建文件, 覆盖式写入
await fs.writeFile realpath, str, options
#在文件尾部追加
await fs.appendFile realpath, str ? reg.source, options
#仅仅新建啥都不写, 可以用open, 但是记得要close
await fs.open path, 'wx'
```



