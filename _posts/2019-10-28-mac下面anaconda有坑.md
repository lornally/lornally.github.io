> 安装很正常, 一用就有坑.

1. 最简单的一个坑, 如果用zsh, 那么就会发现navigator下面正常的东西到了命令行就完全不正常了, 完全找不到. 万能的Stack Overflow告诉我们, 是配置文件错了. anaconda自己改了bash的配置, 但是, 没改zsh的, 剩下的就简单了, 打开两个配置文件, 然后, 把一个改为另一个就OK了.

2. 如何安装package呢?

   ```sh
   # https://docs.anaconda.com/anaconda/user-guide/tasks/install-packages/
   conda install package-name
   ```

3. 举个例子安装mlflow

   ```sh
   conda install mlflow
   #此时会告诉你, 这个conda包不存在.
   # https://anaconda.org/conda-forge/mlflow
   conda install -c conda-forge mlflow 
   ```

4. 这里使用了conda-forge

   - https://conda-forge.org
   - https://anaconda.org/conda-forge
   - https://anaconda.org/search?q=mlflow
   - 简单地说, 就是为conda提供源的组织, 不得不说, python的世界, 大家分工真细致.

5. pip官方文档也说mlflow需要conda在path中

   - https://pypi.org/project/mlflow/
   - 这个地址也可以作为入口地址, 上面还是有不少资料的

6. 那么新的问题来了, 这个mlflow装到哪里去了?

   1. 这个mlflow装在了anaconda, 所以并不重要, 不需要再理会了.
   2. 但是, 我们还是要下载mlflow项目, 以便进一步的工作. 

   ```sh
   git clone https://github.com/mlflow/mlflow
   ```

   