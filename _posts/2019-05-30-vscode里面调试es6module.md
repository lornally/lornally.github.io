###### 网上一通查

- 之前的信息大部分是配合bable的.
- vscode的官网最下方有正解: https://code.visualstudio.com/docs/nodejs/nodejs-debugging

我的luaunch.json:

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "node",
      "request": "launch",
      "name": "Launch Program",
      "program": "${workspaceFolder}/code/test.js",
      "sourceMaps": true,
      "outFiles": ["${workspaceFolder}/code/*"], // 这一行也很关键
      "runtimeArgs": ["--experimental-modules"] //核心配置是这一句
    }
  ]
}
```

> 核心内容都整理到隔壁的vscode 调试 coffee那个blog里面了
