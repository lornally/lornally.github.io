如何处理gitignore

### 找到git全局忽略的ignore文件

```sh
#先看看gitconfig
open .gitconfig
#查看其中的[core]下面的excludesfile
[core]
	excludesfile = /Users/bergman/.gitignore_global
#打开这个excludesfile, 如果没有就在gitconfig建一个, 格式如上面展示.
```

### ignore文件内容:

git忽略示例, 我的全局ignore版本

```sh
##mac通用
###ds_store结尾的.
*DS_Store
*ds_store
### .和~
.*  
####所有.开头的.
*~  
####所有~结尾的
.*/  
####以点开头的目录.

###log目录和文件
log/
logs/
*.log
*.logs

##目录
temp/
*.idea/
upload/
WeiXin/
### generated files
bin/
gen/
###php & android
out/
build/
production/
captures/
gradle/
target/
###thinkphp
runtime/
###codeigniter
cache/
sessions/
*sessions/


##文件
###配置文件
config.php
local.properties
#### Android Studio
*.iml
*.swo
*.iws
*.ipr
*.swp
build.gradle
gradle.properties
gradlew
gradlew.bat
settings.gradle
###压缩文件
*.tar
*.gz
###生成文件
#### built application files
*.apk
*.ap_
#### Java class files
*.class
### files for the dex VM
*.dex
```

### 已经被跟踪的文件, ignore就不起作用了, 比如别人误传的 .ds_store

```sh
git rm -r --cached filename  #把filename移除
```





### 进阶学习: 

> http://gwiki.cn/2016/07/git忽略ignore

