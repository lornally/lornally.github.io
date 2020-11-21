> 今天遇到了莫名的问题,  npm install --save-dev webpack-dev-server #这一句报错

```sh
 #这个报错好扎心
 code ERESOLVE ERESOLVE unable to resolve dependency tree
```



###### 检查环境

- 发现我的node貌似不太对劲. 删除node之后重新安装了一下.
- node还是有brew安装吧, 除非你有使用老版本node的需求. 
- yarn也用brew安装. 官方建议用node装, 我们也用node装吧.
- eslint千万不要用brew安装, 装完了不正常.

```sh
 cnpm install -g eslint 
 cnpm install -g yarn
```

- 然后, npm依旧会报各种错误, 这个是他们的问题. 暂时用yarn顶一下吧. 

###### 当前两个问题, 没有头绪了.

```sh
#npm问题
 code ERESOLVE ERESOLVE unable to resolve dependency tree

# eslint问题
Oops! Something went wrong! :(

ESLint: 7.12.1

ESLint couldn't find the plugin "eslint-plugin-react".

(The package "eslint-plugin-react" was not found when loaded as a Node module from the directory "/Users/bergman/git/_X/code/基础项目/打包/pack实践".)

It's likely that the plugin isn't installed correctly. Try reinstalling by running the following:

    npm install eslint-plugin-react@latest --save-dev

The plugin "eslint-plugin-react" was referenced from the config file in ".eslintrc.js".

If you still can't figure out the problem, please stop by https://eslint.org/chat/help to chat with the team.
```

- 删除所有的node_module, npm问题解决了. 报错变成了warn.
- 重启一次机器吧. 

webpackserve会报错, 愿意是使用了cli-v3的写法

```js
		"start": "webpack serve"
		"start": "webpack-dev-server --open" //这个不对
     "start": "webpack serve --open", //这个也不对

```

eslint问题咋办呢?

```sh
 cnpm install -g eslint-plugin-react 
 yarn add eslint-plugin-react@latest --dev
```

- 找到原因了, 是因为eslint傻乎乎的从配置文件的位置寻找插件. eslint好傻啊. 



至此真相大白, 两个bug

- npm和node最新的bug, 打包依赖的解析不正确.  已经提交官方issue.
- eslint最新bug, 会错误的从eslint自己的配置文件去找插件.  也已经提交官方issue.

```sh
eslint --env-info #这个可以查看eslint的本地info
```

- 临时的解决方案
  - 用yarn代替npm.
  - eslint的配置文件尤其是react的, 放到项目里面. 