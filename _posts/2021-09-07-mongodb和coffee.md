> mongo这些年有了很大的变化

### 安装

```sh
brew tap mongodb/brew
brew install mongodb-community

# 后台启动
brew services start mongodb-community

# 前台启动
mongod --config /usr/local/etc/mongod.conf

# 有问题就重装
brew untap mongodb/brew && brew tap mongodb/brew
brew install mongodb-community
```

### 使用

```sh
# shell也要安装
brew install mongosh
# 启动shell
mongosh
# 有一批tools可以用
mongotop
```

