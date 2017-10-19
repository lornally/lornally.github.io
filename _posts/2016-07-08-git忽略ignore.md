> git必须忽略某些文件, 比如log文件, 比如该死的[.DS_Store]

### 如果gitigore不起作用

1. 注意： .gitignore 和 excludes只能忽略那些原来没有被track的文件，如果某些文件已经被纳入了版本管理中，则修改.gitignore是无效的。
2. 因为, .gitignore这个文件是用于 untracked 文件的忽略列表,  用add将文件标记为 tracked 状态 .gitignore就对其无效了
3. git rm --cached filename  把他移除
4. 如果是pull的时候出问题, 还需要先rm filename 物理移除

```sh
rm filename #物理移除
git reset HEAD <file> #移除那些被错误的add的文件
git pull --rebase #这个pull方式比较好, 如果本地有commit的时候, 可以有效避免merge.
git rm --cached filename #git移除
git rm --ignore-unmatch  --cached -r */*/.DS_Store #移除两层目录下的ds, -r循环移除, --ignore-unmatch忽略不匹配的情况.
find . -name .DS_Store -print0 | xargs -0 git rm -f --ignore-unmatch --cached

zsh 里面这些都报错. 所以mate .zshrc , 加下面这句: 
setopt no_nomatch

########zsh命令
rm filename #这个一样的
gl --rebase
g rm --cached filename
gaa #git add -A
gcam "log" #git commit -am "log" 
gp #git push
gss #git status简洁模式
gst #git status
```

### 可以使用gitignore

1. 默认忽略用ignore文件

2. 还可以
   1. 在每个clone下来的仓库中手动设置不要检查特定文件的更改情况。
   2. git update-index --assume-unchanged PATH    在PATH处输入要忽略的文件。
   3. 取消这种设定可以使用:  git update-index --no-assume-unchanged

3. 另外 git 提供了一个全局的 .gitignore，你可以在你的用户目录下创建 ~/.gitignore，以同样的规则来划定哪些文件是不需要版本控制的。

   ```sh
   git config --global core.excludesfile ~/.gitignore #需要执行, 来使得它生效
   ```

4. 或者使用配置文件在 ~/.gitconfig 中引入 .gitignore文件, 比如:

   ```ini
   [user]
      name = xiaoronglv
      email = xxxxx@gmail.com
   [push]
      default = matching
   [core]
      excludesfile = ~/.gitignore
   ```

### git ignore格式:

1. shell的glob模式正则匹配

2. 所有空行或已#开头的行都会被git忽略（即相当于不读取）。

3. 可以使用标准的 glob 模式匹配。

4. 匹配模式最后跟反斜杠（/）说明要忽略的是目录。

5. 要忽略指定模式以外的文件或目录，可以在模式前加上惊叹号（!）取反（即保守模式）。

   ```sh
   *.[oa]   #第一行告诉git忽略以 .a 或 .o 结尾的文件，， 
   *~  
   *.swp
   *.bak
   *.DS_Store  #第二、三、四、五行告诉git忽略以～、swp、bak、DS_Store结尾的文件
   logs/*  #第六行告诉git忽略logs目录下的所有文件。
   *.a                      #忽略以a结尾的文件
   !core.a                  #跟踪core.a
   /config                  #忽略根目录下的 config文件，但 /abc/config 不会被忽略
   logs/                    #忽略logs目录
   doc/*.pyc                #忽略doc目录下所有以pyc结尾的文件，但不包括 doc/abc/*.pyc
   /www/*
   !/www/base/              #这两行表示只跟踪根目录下的www中的base目录
   .*         #忽略[.]开头的系统文件, 比如臭名昭著的 .ds_store.
   *~				#忽略以[~]结尾的临时文件.
   .DS_Store    #重要的事情做兩邊
   ```

   ​

### git忽略示例, 我的全局ignore版本

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

### 参考这个很详细的说明: 

1. http://blog.sina.com.cn/s/blog_601f224a01012a9r.html
2. http://blog.csdn.net/xmyzlz/article/details/8592302
3. http://www.cnlvzi.com/index.php/Index/article/id/122