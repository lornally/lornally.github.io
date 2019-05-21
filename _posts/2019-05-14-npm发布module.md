> 了不起的nodejs上面很简单: npm publish

然而实际上这么干不行.

1. 注册一个npm账号

2. 命令行操作

   ```sh
   npm adduser
   npm login
   npm config ls
   ```

3. 然后npm publish 依旧失败: 

   ```sh
   403 Forbidden - PUT https://registry.npmjs.org/ - You do not have permission to publish "". Are you logged in as the correct user?
   ```

4. 万能的stackoverflow说应该是名字重复了.

   ```sh
   npm search mtool
   ```

5. 真的是这样的.