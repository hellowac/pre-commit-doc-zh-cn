# 安装

**Installation**

=== "中文"

    在您可以运行钩子之前，需要安装 pre-commit 包管理器。
    
    使用 pip：
    
    ```bash
    pip install pre-commit
    ```
    
    在 Python 项目中，将以下内容添加到您的 requirements.txt（或 requirements-dev.txt）中：
    
    ```
    pre-commit
    ```
    
    作为一个零依赖的 [zipapp]：
    
    - 从 [github releases] 找到并下载 `.pyz` 文件
    - 用 `python pre-commit-#.#.#.pyz ...` 替代 `pre-commit ...` 运行
    
    [zipapp]: https://docs.python.org/3/library/zipapp.html  
    [github releases]: https://github.com/pre-commit/pre-commit/releases

=== "英文"

    Before you can run hooks, you need to have the pre-commit package manager
    installed.
    
    Using pip:
    
    ```bash
    pip install pre-commit
    ```
    
    In a python project, add the following to your requirements.txt (or
    requirements-dev.txt):
    
    ```
    pre-commit
    ```
    
    As a 0-dependency [zipapp]:
    
    - locate and download the `.pyz` file from the [github releases]
    - run `python pre-commit-#.#.#.pyz ...` in place of `pre-commit ...`
    
    [zipapp]: https://docs.python.org/3/library/zipapp.html
    [github releases]: https://github.com/pre-commit/pre-commit/releases

## 快速开始

**Quick start**

### 1. 安装 pre-commit

**1. Install pre-commit**

=== "中文"
    
    - 按照上面的[安装](#安装)指示操作
    - 运行 `pre-commit --version` 应该能显示你正在使用的版本号

    ```cmd
    pre-commit --version
    ```


=== "英文"

    - follow the [install](#安装) instructions above
    - `pre-commit --version` should show you what version you're using

    ```cmd
    pre-commit --version
    ```

### 2. 添加一个 pre-commit 配置

**2. Add a pre-commit configuration**

=== "中文"
    
      - 创建一个名为 `.pre-commit-config.yaml` 的文件
      - 你可以使用 [`pre-commit sample-config`](./cli.md#pre-commit-sample-config) 生成一个非常基础的配置
      - 配置的完整选项列表在[下面](./plugins.md)列出
      - 这个例子使用了 Python 代码的格式化器，但是 `pre-commit` 适用于任何编程语言
      - 其他[支持的钩子](./hooks.md)也是可用的
    
      ```yaml
      repos:
      -   repo: https://github.com/pre-commit/pre-commit-hooks 
          rev: v2.3.0
          hooks:
          -   id: check-yaml
          -   id: end-of-file-fixer
          -   id: trailing-whitespace
      -   repo: https://github.com/psf/black 
          rev: 22.10.0
          hooks:
          -   id: black
      ```


=== "英文"
    
      - create a file named `.pre-commit-config.yaml`
      - you can generate a very basic configuration using
        [`pre-commit sample-config`](./cli.md#pre-commit-sample-config)
      - the full set of options for the configuration are listed [below](#plugins)
      - this example uses a formatter for python code, however `pre-commit` works for
        any programming language
      - other [supported hooks](./hooks.md) are available
    
      ```yaml
      repos:
      -   repo: https://github.com/pre-commit/pre-commit-hooks
          rev: v2.3.0
          hooks:
          -   id: check-yaml
          -   id: end-of-file-fixer
          -   id: trailing-whitespace
      -   repo: https://github.com/psf/black
          rev: 22.10.0
          hooks:
          -   id: black
      ```

### 3. 安装 git hook 脚本

**3. Install the git hook scripts**

=== "中文"

    - 运行 `pre-commit install` 来设置 Git 钩子脚本
    
    ```console
    $ pre-commit install
    pre-commit installed at .git/hooks/pre-commit
    ```
    
    - 现在 `pre-commit` 会在 `git commit` 时自动运行！
    

=== "英文"

    - run `pre-commit install` to set up the git hook scripts

    ```console
    $ pre-commit install
    pre-commit installed at .git/hooks/pre-commit
    ```

    - now `pre-commit` will run automatically on `git commit`!

### 4. （可选）针对所有文件运行

**4. (optional) Run against all the files**

=== "中文"

    - 通常在添加新钩子时，对所有文件运行钩子是个好主意（通常 `pre-commit` 只会在 Git 钩子期间对变更的文件运行）
    
    ```pre-commit
    $ pre-commit run --all-files
    [INFO] Initializing environment for https://github.com/pre-commit/pre-commit-hooks. 
    [INFO] Initializing environment for https://github.com/psf/black. 
    [INFO] Installing environment for https://github.com/pre-commit/pre-commit-hooks. 
    [INFO] Once installed this environment will be reused.
    [INFO] This may take a few minutes...
    [INFO] Installing environment for https://github.com/psf/black. 
    [INFO] Once installed this environment will be reused.
    [INFO] This may take a few minutes...
    Check Yaml...............................................................Passed
    Fix End of Files.........................................................Passed
    Trim Trailing Whitespace.................................................Failed
    - hook id: trailing-whitespace
    - exit code: 1
    
    Files were modified by this hook. Additional output:
    
    Fixing sample.py
    
    black....................................................................Passed
    ```
    
    - 哎呀！看起来我的文件有一些尾随空白
    - 考虑在[持续集成](./advanced.md#在持续集成中的使用)中也运行这个钩子
    

=== "英文"

    - it's usually a good idea to run the hooks against all of the files when adding
      new hooks (usually `pre-commit` will only run on the changed files during
      git hooks)

    ```pre-commit
    $ pre-commit run --all-files
    [INFO] Initializing environment for https://github.com/pre-commit/pre-commit-hooks.
    [INFO] Initializing environment for https://github.com/psf/black.
    [INFO] Installing environment for https://github.com/pre-commit/pre-commit-hooks.
    [INFO] Once installed this environment will be reused.
    [INFO] This may take a few minutes...
    [INFO] Installing environment for https://github.com/psf/black.
    [INFO] Once installed this environment will be reused.
    [INFO] This may take a few minutes...
    Check Yaml...............................................................Passed
    Fix End of Files.........................................................Passed
    Trim Trailing Whitespace.................................................Failed
    - hook id: trailing-whitespace
    - exit code: 1

    Files were modified by this hook. Additional output:

    Fixing sample.py

    black....................................................................Passed
    ```

    - oops! looks like I had some trailing whitespace
    - consider running that in [CI](./advanced.md#在持续集成中的使用) too
