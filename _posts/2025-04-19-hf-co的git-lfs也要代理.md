

###### 2025-4-19-hf-co的git-lfs也要代理

```sh
git clone git@hf.co:stabilityai/stable-diffusion-xl-base-1.0
# 这里报错认证错误, 我就错误的理解为https问题, 其实是是https://huggingface.co 需要设置ssh key
https://huggingface.co/settings/keys
```

顺便补充点代理下载的知识:

```sh
# 临时方案
git -c http.proxy=socks5://127.0.0.1:7890 clone <repo-url>

# 全局配置
git config --global https.proxy socks5://127.0.0.1:7891
# .gitconfig配置
[includeIf "gitdir/i:github.com/"]
    path = ~/.gitconfig-proxy  # 代理专用配置
[includeIf "gitdir/i:corp.intra/"]
    path = ~/.gitconfig-noproxy  # 内网无代理配置

# 临时忽略证书验证（不推荐长期使用）
git config --global http.sslVerify false
# 常规 clone
GIT_SSL_NO_VERIFY=1 git clone https://example.com/repo.git

# LFS 专用（需同时设置）
GIT_SSL_NO_VERIFY=1 git -c http.sslVerify=false lfs clone https://example.com/repo.git

export GIT_SSL_NO_VERIFY=1  # 当前终端会话有效
git  clone git@hf.co:stabilityai/stable-diffusion-xl-base-1.0
unset GIT_SSL_NO_VERIFY     # 操作后立即恢复

# 检查生效配置
git config --show-scope --get http.proxy

# 测试流量走向
GIT_CURL_VERBOSE=1 git fetch 2>&1 | grep -i proxy

# 查看 ClashX 实时日志
tail -f ~/Library/Logs/clashx.log | grep 'git'

```

###### 2020-3-17 时隔多年github也要代理

> github拿项目巨慢, 3k/s, 这得拿到猴年.

http模式的代理, 其实mac下面这个很鸡肋. 

```sh
#github加代理给http模式用
alias giton='git config --global http.https://github.com.proxy socks5://127.0.0.1:1080'

#取消代理
alias gitoff='git config --global --unset http.https://github.com.proxy'
```

如果是pac模式, 那么记得在代理软件的设置中要加入下面规则

```
||github.com^
```

设置ssh模式的代理, 这个才是重点, 比如这种clone方式就是ssh模式: git clone git@github.com:play-with-docker/play-with-docker.github.io.git

```sh
code ~/.ssh/config   # 用别的工具也可以, mate, vi, cat, echo....

文件内编辑: 
Host github.com
    ProxyCommand          nc -x 127.0.0.1:1080 %h %p   #sockes代理的写法
    ServerAliveInterval   10

ProxyCommand nc -X connect -x localhost:1080 %h %p #http代理的写法 
```





###### 2019-4-29 git也要代理

针对http的设置

```sh
git config --global http.proxy 'http://127.0.0.1:1081' && git config --global https.proxy 'http://127.0.0.1:1081
```

```sh
#这个是全局的 在不需要的时候关闭
git config --global http.proxy '' && git config --global https.proxy ''
```



针对shadowsocks的设置

```sh
git config --global http.proxy 'socks5://127.0.0.1:1080' && git config --global https.proxy 'socks5://127.0.0.1:1080'
```

