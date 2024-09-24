# 命令行接口

**Command line interface**

=== "中文"
    
    所有 pre-commit 命令都接受以下选项：

    - `--color {auto,always,never}`：是否在输出中使用颜色。默认为 `auto`。可以通过使用 `PRE_COMMIT_COLOR={auto,always,never}` 覆盖，或者通过使用 `TERM=dumb` 禁用颜色。
    - `-c CONFIG`, `--config CONFIG`：指向替代配置文件的路径。
    - `-h`, `--help`：显示帮助和可用选项。

    新特性 2.8.0：`pre-commit` 现在使用更具体的退出代码：
    - `1`：检测到的/预期的错误
    - `3`：意外的错误
    - `130`：进程被 `^C` 中断


=== "英文"

    All pre-commit commands take the following options:
    
    - `--color {auto,always,never}`: whether to use color in output.
      Defaults to `auto`.  can be overridden by using
      `PRE_COMMIT_COLOR={auto,always,never}` or disabled using `TERM=dumb`.
    - `-c CONFIG`, `--config CONFIG`: path to alternate config file
    - `-h`, `--help`: show help and available options.
    
    _new in 2.8.0_: `pre-commit` now exits with more specific codes:
    - `1`: a detected / expected error
    - `3`: an unexpected error
    - `130`: the process was interrupted by `^C`

## pre-commit autoupdate [options]

<span id="pre-commit-autoupdate"></span>

=== "中文"
    
    自动更新 pre-commit 配置到仓库的最新版本。
    
    选项：
    
    - `--bleeding-edge`：更新到默认分支的最新开发版本，而不是最新的已标记版本（默认行为）。
    - `--freeze`：在 [`rev`](./plugins.md#repos-rev) 中存储“冻结”的哈希值，而不是标签名称。
    - `--repo REPO`：只更新这个仓库。这个选项可以多次指定。
    - `-j` / `--jobs`：_3.3.0 版本新增_ 要使用的线程数量（默认：1）。
    
    以下是使用 `.pre-commit-config.yaml` 的一些示例调用：
    
    ```yaml
    repos:
    -   repo: https://github.com/pre-commit/pre-commit-hooks 
        rev: v2.1.0
        hooks:
        -   id: trailing-whitespace
    -   repo: https://github.com/asottile/pyupgrade 
        rev: v1.25.0
        hooks:
        -   id: pyupgrade
            args: [--py36-plus]
    ```
    
    ```console
    $ : 默认：更新到默认分支的最新标签
    $ pre-commit autoupdate  # 默认：选择标签
    Updating https://github.com/pre-commit/pre-commit-hooks  ... updating v2.1.0 -> v2.4.0.
    Updating https://github.com/asottile/pyupgrade  ... updating v1.25.0 -> v1.25.2.
    $ grep rev: .pre-commit-config.yaml
        rev: v2.4.0
        rev: v1.25.2
    ```
    
    ```console
    $ : 更新特定仓库到默认分支的最新修订版
    $ pre-commit autoupdate --bleeding-edge --repo https://github.com/pre-commit/pre-commit-hooks 
    Updating https://github.com/pre-commit/pre-commit-hooks  ... updating v2.1.0 -> 5df1a4bf6f04a1ed3a643167b38d502575e29aef.
    $ grep rev: .pre-commit-config.yaml
        rev: 5df1a4bf6f04a1ed3a643167b38d502575e29aef
        rev: v1.25.0
    ```
    
    ```console
    $ : 更新到冻结版本
    $ pre-commit autoupdate --freeze
    Updating https://github.com/pre-commit/pre-commit-hooks  ... updating v2.1.0 -> v2.4.0 (frozen).
    Updating https://github.com/asottile/pyupgrade  ... updating v1.25.0 -> v1.25.2 (frozen).
    $ grep rev: .pre-commit-config.yaml
        rev: 0161422b4e09b47536ea13f49e786eb3616fe0d7  # 冻结：v2.4.0
        rev: 34a269fd7650d264e4de7603157c10d0a9bb8211  # 冻结：v1.25.2
    ```
    
    新特性 2.18.0：如果存在并列情况，pre-commit 将优先选择包含 `.` 的标签。
    
    
=== "英文"

    Auto-update pre-commit config to the latest repos' versions.
    
    Options:
    
    - `--bleeding-edge`: update to the bleeding edge of the default branch instead
      of the latest tagged version (the default behaviour).
    - `--freeze`: Store "frozen" hashes in [`rev`](./plugins.md#repos-rev) instead of tag names.
    - `--repo REPO`: Only update this repository. This option may be specified
      multiple times.
    - `-j` / `--jobs`: _new in 3.3.0_ Number of threads to use (default: 1).
    
    Here are some sample invocations using this `.pre-commit-config.yaml`:
    
    ```yaml
    repos:
    -   repo: https://github.com/pre-commit/pre-commit-hooks
        rev: v2.1.0
        hooks:
        -   id: trailing-whitespace
    -   repo: https://github.com/asottile/pyupgrade
        rev: v1.25.0
        hooks:
        -   id: pyupgrade
            args: [--py36-plus]
    ```
    
    ```console
    $ : default: update to latest tag on default branch
    $ pre-commit autoupdate  # by default: pick tags
    Updating https://github.com/pre-commit/pre-commit-hooks ... updating v2.1.0 -> v2.4.0.
    Updating https://github.com/asottile/pyupgrade ... updating v1.25.0 -> v1.25.2.
    $ grep rev: .pre-commit-config.yaml
        rev: v2.4.0
        rev: v1.25.2
    ```
    
    ```console
    $ : update a specific repository to the latest revision of the default branch
    $ pre-commit autoupdate --bleeding-edge --repo https://github.com/pre-commit/pre-commit-hooks
    Updating https://github.com/pre-commit/pre-commit-hooks ... updating v2.1.0 -> 5df1a4bf6f04a1ed3a643167b38d502575e29aef.
    $ grep rev: .pre-commit-config.yaml
        rev: 5df1a4bf6f04a1ed3a643167b38d502575e29aef
        rev: v1.25.0
    ```
    
    ```console
    $ : update to frozen versions
    $ pre-commit autoupdate --freeze
    Updating https://github.com/pre-commit/pre-commit-hooks ... updating v2.1.0 -> v2.4.0 (frozen).
    Updating https://github.com/asottile/pyupgrade ... updating v1.25.0 -> v1.25.2 (frozen).
    $ grep rev: .pre-commit-config.yaml
        rev: 0161422b4e09b47536ea13f49e786eb3616fe0d7  # frozen: v2.4.0
        rev: 34a269fd7650d264e4de7603157c10d0a9bb8211  # frozen: v1.25.2
    ```
    
    _new in 2.18.0_: pre-commit will preferentially pick tags containing a `.` if
    there are ties.

## pre-commit clean [options]

<span id="pre-commit-clean"></span>

=== "中文"
    
    清理缓存的 pre-commit 文件。

    选项：（无其他选项）


=== "英文"

    Clean out cached pre-commit files.
    
    Options: (no additional options)

## pre-commit gc [options]

<span id="pre-commit-gc"></span>

=== "中文"
    
    清理未使用的缓存仓库。
    
    `pre-commit` 会保留已安装钩子仓库的缓存，随着时间的推移，这个缓存会不断增长。可以定期运行此命令以清理缓存目录中未使用的仓库。
    
    选项：（无其他选项）


=== "英文"

    Clean unused cached repos.
    
    `pre-commit` keeps a cache of installed hook repositories which grows over
    time.  This command can be run periodically to clean out unused repos from
    the cache directory.
    
    Options: (no additional options)

## pre-commit init-templatedir DIRECTORY [options]

<span id="pre-commit-init-templatedir"></span>

=== "中文"

    在用于与 `git config init.templateDir` 一起使用的目录中安装钩子脚本。

    选项：
    
    - `-t HOOK_TYPE, --hook-type HOOK_TYPE`：要安装的钩子类型。
    
    一些示例调用：
    
    ```bash
    git config --global init.templateDir ~/.git-template
    pre-commit init-templatedir ~/.git-template
    ```
    
    对于 Windows cmd.exe 使用 `%HOMEPATH%` 而不是 `~`：
    
    ```batch
    pre-commit init-templatedir %HOMEPATH%\.git-template
    ```
    
    对于 Windows PowerShell 使用 `$HOME` 而不是 `~`：
    
    ```powershell
    pre-commit init-templatedir $HOME\.git-template
    ```
    
    现在，无论何时克隆或创建仓库，它都会预先设置好钩子！
    

=== "英文"

    Install hook script in a directory intended for use with
    `git config init.templateDir`.
    
    Options:
    
    - `-t HOOK_TYPE, --hook-type HOOK_TYPE`:
      which hook type to install.
    
    Some example useful invocations:
    
    ```bash
    git config --global init.templateDir ~/.git-template
    pre-commit init-templatedir ~/.git-template
    ```
    
    For Windows cmd.exe use `%HOMEPATH%` instead of `~`:
    
    ```batch
    pre-commit init-templatedir %HOMEPATH%\.git-template
    ```
    
    For Windows PowerShell use `$HOME` instead of `~`:
    
    ```powershell
    pre-commit init-templatedir $HOME\.git-template
    ```
    
    Now whenever a repository is cloned or created, it will have the hooks set up
    already!

## pre-commit install [options]

<span id="pre-commit-install"></span>

=== "中文"

    安装 pre-commit 脚本。

    选项：
    
    - `-f`, `--overwrite`：用 pre-commit 脚本替换任何现有的 git 钩子。
    - `--install-hooks`：现在就安装所有可用钩子的环境（而不是在它们第一次执行时）。参见 [`pre-commit install-hooks`](#pre-commit-install-hooks)。
    - `-t HOOK_TYPE, --hook-type HOOK_TYPE`：指定要安装的钩子类型。
    - `--allow-missing-config`：如果配置文件缺失，钩子脚本将允许这种情况。
    
    一些示例调用：
    
    - `pre-commit install`：默认调用。在任何现有的 git 钩子旁安装钩子脚本。
    - `pre-commit install --install-hooks --overwrite`：用 pre-commit 替换现有的 git 钩子脚本，并且安装钩子环境。
    
    新特性 2.18.0：如果命令行上没有指定 `--hook-type`，`pre-commit install` 现在将从 [`default_install_hook_types`](./plugins.md#top_level-default_install_hook_types) 安装钩子。
    

=== "英文"
    
    Install the pre-commit script.
    
    Options:
    
    - `-f`, `--overwrite`: Replace any existing git hooks with the pre-commit
      script.
    - `--install-hooks`: Also install environments for all available hooks now
      (rather than when they are first executed). See [`pre-commit
      install-hooks`](#pre-commit-install-hooks).
    - `-t HOOK_TYPE, --hook-type HOOK_TYPE`:
      Specify which hook type to install.
    - `--allow-missing-config`: Hook scripts will permit a missing configuration
      file.
    
    Some example useful invocations:
    
    - `pre-commit install`: Default invocation. Installs the hook scripts
       alongside any existing git hooks.
    - `pre-commit install --install-hooks --overwrite`: Idempotently replaces
       existing git hook scripts with pre-commit, and also installs hook
       environments.
    
    _new in 2.18.0_: `pre-commit install` will now install hooks from
    [`default_install_hook_types`](./plugins.md#top_level-default_install_hook_types) if
    `--hook-type` is not specified on the command line.

## pre-commit install-hooks [options]

<span id="pre-commit-install-hooks"></span>

=== "中文"
    
    安装所有可用钩子的缺失环境。除非执行了此命令或 `install --install-hooks`，否则每个钩子的环境将在第一次调用钩子时创建。

    每个钩子都在适合其编写语言的独立环境中初始化。参见 [支持的语言](./new-hooks.md#支持的语言)。
    
    此命令不会安装 pre-commit 脚本。要在一个命令中安装脚本和钩子环境，请使用 `pre-commit install --install-hooks`。
    
    选项：（无其他选项）


=== "英文"

    Install all missing environments for the available hooks. Unless this command or
    `install --install-hooks` is executed, each hook's environment is created the
    first time the hook is called.
    
    Each hook is initialized in a separate environment appropriate to the language
    the hook is written in. See [supported languages](./new-hooks.md#支持的语言).
    
    This command does not install the pre-commit script. To install the script along with
    the hook environments in one command, use `pre-commit install --install-hooks`.
    
    Options: (no additional options)

## pre-commit migrate-config [options]

<span id="pre-commit-migrate-config"></span>

=== "中文"
    
    迁移列表配置到新的映射配置格式。

    选项：（无其他选项）


=== "英文"

    Migrate list configuration to the new map configuration format.
    
    Options: (no additional options)

## pre-commit run [hook-id] [options]

<span id="pre-commit-run"></span>

=== "中文"

    运行钩子。

    选项：
    
    - `[hook-id]`：指定单个 hook-id，只运行该钩子。
    - `-a`, `--all-files`：在仓库中的所有文件上运行。
    - `--files [FILES [FILES ...]]`：特定的文件名来运行钩子。
    - `--from-ref FROM_REF` + `--to-ref TO_REF`：在 git 中 `FROM_REF...TO_REF` 之间更改的文件上运行。
      - _2.2.0 版本新增_：在 2.2.0 版本之前参数为 `--source` 和 `--origin`。
    - `--hook-stage STAGE`：选择要运行的 [`阶段`](./advanced.md#限制钩子在某些阶段运行)。
    - `--show-diff-on-failure`：当钩子失败时，立即运行 `git diff`。
    - `-v`, `--verbose`：无论成功与否都产生钩子输出。在输出中包含钩子 id。
    
    一些示例调用：
    - `pre-commit run`：这是提交时 pre-commit 默认运行的操作。这将对当前暂存的文件运行所有钩子。
    - `pre-commit run --all-files`：对所有文件运行所有钩子。如果您在 CI 中使用 pre-commit，这是一个有用的调用。
    - `pre-commit run flake8`：对所有暂存的文件运行 `flake8` 钩子。
    - `git ls-files -- '*.py' | xargs pre-commit run --files`：对仓库中所有的 `*.py` 文件运行所有钩子。
    - `pre-commit run --from-ref HEAD^^^ --to-ref HEAD`：对 `HEAD^^^` 和 `HEAD` 之间更改的文件运行。当在 pre-receive 钩子中使用时，这种形式很有用。
    

=== "英文"

    Run hooks.
    
    Options:
    
    - `[hook-id]`: specify a single hook-id to run only that hook.
    - `-a`, `--all-files`: run on all the files in the repo.
    - `--files [FILES [FILES ...]]`: specific filenames to run hooks on.
    - `--from-ref FROM_REF` + `--to-ref TO_REF`: run against the files changed
      between `FROM_REF...TO_REF` in git.
        - _new in 2.2.0_: prior to 2.2.0 the arguments were `--source` and
          `--origin`.
    - `--hook-stage STAGE`: select a [`stage` to run](./advanced.md#限制钩子在某些阶段运行).
    - `--show-diff-on-failure`: when hooks fail, run `git diff` directly afterward.
    - `-v`, `--verbose`: produce hook output independent of success.  Include hook
      ids in output.
    
    Some example useful invocations:
    - `pre-commit run`: this is what pre-commit runs by default when committing.
      This will run all hooks against currently staged files.
    - `pre-commit run --all-files`: run all the hooks against all the files.  This
      is a useful invocation if you are using pre-commit in CI.
    - `pre-commit run flake8`: run the `flake8` hook against all staged files.
    - `git ls-files -- '*.py' | xargs pre-commit run --files`: run all hooks
      against all `*.py` files in the repository.
    - `pre-commit run --from-ref HEAD^^^ --to-ref HEAD`: run against the files that
      have changed between `HEAD^^^` and `HEAD`.  This form is useful when
      leveraged in a pre-receive hook.

## pre-commit sample-config [options]

<span id="pre-commit-sample-config"></span>

=== "中文"

    生成一个示例 `.pre-commit-config.yaml`。

    选项：（无其他选项）
    
    以下是 `.pre-commit-config.yaml` 的一个示例：
    
    ```yaml
    repos:
    -   repo: https://github.com/pre-commit/pre-commit-hooks
        rev: v2.3.0
        hooks:
        -   id: check-yaml
        -   id: end-of-file-fixer
        -   id: trailing-whitespace-fixer
    -   repo: https://github.com/psf/black
        rev: 22.10.0
        hooks:
        -   id: black
    -   repo: https://github.com/asottile/pyupgrade
        rev: v2.29.0
        hooks:
        -   id: pyupgrade
            args: [--py36-plus]
    -   repo: https://github.com/pre-commit/mirrors-prettier
        rev: v2.7.0
        hooks:
        -   id: prettier
    -   repo: local
        hooks:
        -   id: pylint
            name: pylint
            entry: pylint --rcfile=.pylintrc $(git ls-files '*.py')
            language: script
            types: [python]
    -   repo: https://github.com/pre-commit/mirrors-mypy
        rev: v0.931
        hooks:
        -   id: mypy
            additional_dependencies: [types-pkg_resources]
    -   repo: https://github.com/hadolint/hadolint
        rev: v2.8.0
        hooks:
        -   id: hadolint-docker
    -   repo: https://github.com/shellcheck-py/shellcheck-py
        rev: v0.7.2.1
        hooks:
        -   id: shellcheck
    -   repo: https://github.com/pre-commit/mirrors-eslint
        rev: v8.10.0
        hooks:
        -   id: eslint
            additional_dependencies:
              - eslint@7.32.0
              - eslint-plugin-react@7.28.0
              - eslint-plugin-import@2.25.2
              - eslint-config-airbnb@18.2.1
              - eslint-config-prettier@8.3.0
    ```
    
    这个示例配置包括了多个流行的钩子仓库，用于执行诸如格式化代码、检查文件结尾、修复尾随空格、运行 `black` 代码格式化程序、升级 Python 包、运行 `prettier` 代码格式化程序、运行 `mypy` 静态类型检查器、检查 Dockerfile 的 `hadolint`、检查 shell 脚本的 `shellcheck` 以及运行 `eslint` 代码检查器等任务。此外，还展示了如何配置本地钩子和指定额外依赖项。
    

=== "英文"

    Produce a sample `.pre-commit-config.yaml`.
    
    Options: (no additional options)

## pre-commit try-repo REPO [options]

<span id="pre-commit-try-repo"></span>

=== "中文"

    尝试一个仓库中的钩子，这对于开发新的钩子非常有用。
    `try-repo` 也可以用来在添加到你的配置之前测试一个仓库。`try-repo` 在运行钩子之前会打印出基于远程钩子仓库生成的配置。
    
    选项：
    
    - `REPO`: 必需的可克隆的钩子仓库。可以是磁盘上的本地路径。
    - `--ref REF`: 手动选择一个引用来运行，否则将使用 `HEAD` 修订版。
    - `pre-commit try-repo` 也支持所有可用的 [`pre-commit run`](#pre-commit-run-hook-id) 选项。
    
    一些有用的调用示例：

    - `pre-commit try-repo https://github.com/pre-commit/pre-commit-hooks`: 在 `pre-commit/pre-commit-hooks` 的最新修订版中运行所有钩子。
    - `pre-commit try-repo ../path/to/repo`: 在磁盘上的仓库中运行所有钩子。
    - `pre-commit try-repo ../pre-commit-hooks flake8`: 仅运行在本地 `../pre-commit-hooks` 仓库中配置的 `flake8` 钩子。
    - 参见 [`pre-commit run`](#pre-commit-run-hook-id) 获取更多有用的 `run` 调用示例，这些也被 `pre-commit try-repo` 支持。
    

=== "英文"

    Try the hooks in a repository, useful for developing new hooks.
    `try-repo` can also be used for testing out a repository before adding it to
    your configuration.  `try-repo` prints a configuration it generates based on
    the remote hook repository before running the hooks.
    
    Options:
    
    - `REPO`: required clonable hooks repository.  Can be a local path on
      disk.
    - `--ref REF`: Manually select a ref to run against, otherwise the `HEAD`
      revision will be used.
    - `pre-commit try-repo` also supports all available options for
      [`pre-commit run`](#pre-commit-run-hook-id).
    
    Some example useful invocations:
    - `pre-commit try-repo https://github.com/pre-commit/pre-commit-hooks`: runs
      all the hooks in the latest revision of `pre-commit/pre-commit-hooks`.
    - `pre-commit try-repo ../path/to/repo`: run all the hooks in a repository on
      disk.
    - `pre-commit try-repo ../pre-commit-hooks flake8`: run only the `flake8` hook
      configured in a local `../pre-commit-hooks` repository.
    - See [`pre-commit run`](#pre-commit-run-hook-id) for more useful `run` invocations
      which are also supported by `pre-commit try-repo`.


## pre-commit uninstall [options]

<span id="pre-commit-uninstall"></span>

=== "中文"

    卸载 pre-commit 脚本。

    选项：
    
    - `-t HOOK_TYPE, --hook-type HOOK_TYPE`: 指定要卸载的钩子类型。
    

=== "英文"

    Uninstall the pre-commit script.
    
    Options:
    
    - `-t HOOK_TYPE, --hook-type HOOK_TYPE`: which hook type to uninstall.
