> sublime的ruby formatter表现不正常, vscode竟然也不正常



### vscode 罢工

- 安装了ruby插件, 依旧不能正确format
  - ruby
  - solargraph

```sh
# vscode启动就报错
Couldn't start client Ruby Language Server
```

- 有个说法: typescript.format.enable

### sublime注释决定format

- beautiy ruby 可以解决问题, 但是, 需要设置快捷键

```
在key绑定可以用这个: 	{ "keys": ["alt+tab"], "command": "beautify_ruby" },
```

参考

- https://stackoverflow.com/questions/63207121/sqlbeautifier-how-to-change-superk-superf-key-binding
- https://stackoverflow.com/questions/16213381/sublime-text-comment-indentation-issue-with-ruby/18348573#18348573
