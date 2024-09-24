# 创建新钩子

**Creating new hooks**

=== "中文"
    
    pre-commit 当前支持用[许多语言](#支持的语言)编写的钩子。只要您的 git 仓库是一个可安装的包（gem、npm、pypi 等）或公开了一个可执行文件，就可以与 pre-commit 一起使用。每个 git 仓库可以支持您想要的任意数量的语言/钩子。
    
    钩子必须在失败时退出非零状态，或修改文件。
    
    包含 pre-commit 插件的 git 仓库必须包含一个 `.pre-commit-hooks.yaml` 文件，该文件告诉 pre-commit：
    
    | 配置项                                                                                                               | 描述                                                                                                                        |
    | -------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
    | [`id`](#hooks-id)<span id="hooks-id"></span>                                                                         | 钩子的 ID - 在 `pre-commit-config.yaml` 中使用。                                                                            |
    | [`name`](#hooks-name)<span id="hooks-name"></span>                                                                   | 钩子的名称 - 在钩子执行期间显示。                                                                                           |
    | [`entry`](#hooks-entry)<span id="hooks-entry"></span>                                                                | 入口点 - 要运行的可执行文件。`entry` 还可以包含不会被覆盖的参数，例如 `entry: autopep8 -i`。                                |
    | [`language`](#hooks-language)<span id="hooks-language"></span>                                                       | 钩子的语言 - 告诉 pre-commit 如何安装钩子。                                                                                 |
    | [`files`](#hooks-files)<span id="hooks-files"></span>                                                                | （可选，默认为 `''`）要运行的文件的模式。                                                                                   |
    | [`exclude`](#hooks-exclude)<span id="hooks-exclude"></span>                                                          | （可选，默认为 `^$`）排除与 [`files`](#hooks-files) 匹配的文件。                                                            |
    | [`types`](#hooks-types)<span id="hooks-types"></span>                                                                | （可选，默认为 `[file]`）要运行的文件类型列表（AND）。参见[使用类型过滤文件](./advanced.md#根据文件类型过滤)。              |
    | [`types_or`](#hooks-types_or)<span id="hooks-types_or"></span>                                                       | （可选，默认为 `[]`）要运行的文件类型列表（OR）。参见[使用类型过滤文件](./advanced.md#根据文件类型过滤)。_2.9.0 版本新增_。 |
    | [`exclude_types`](#hooks-exclude_types)<span id="hooks-exclude_types"></span>                                        | （可选，默认为 `[]`）要排除的文件的模式。                                                                                   |
    | [`always_run`](#hooks-always_run)<span id="hooks-always_run"></span>                                                 | （可选，默认为 `false`）如果为 `true`，则即使没有匹配的文件，这个钩子也会运行。                                             |
    | [`fail_fast`](#hooks-fail_fast)<span id="hooks-fail_fast"></span>                                                    | （可选，默认为 `false`）如果为 `true`，pre-commit 会在该钩子失败时停止运行钩子。_2.16.0 版本新增_。                         |
    | [`verbose`](#hooks-verbose)<span id="hooks-verbose"></span>                                                          | （可选，默认为 `false`）如果为 `true`，则即使钩子通过，也强制打印钩子的输出。                                               |
    | [`pass_filenames`](#hooks-pass_filenames)<span id="hooks-pass_filenames"></span>                                     | （可选，默认为 `true`）如果为 `false`，则不会向钩子传递文件名。                                                             |
    | [`require_serial`](#hooks-require_serial)<span id="hooks-require_serial"></span>                                     | （可选，默认为 `false`）如果为 `true`，则这个钩子将使用单个进程而不是并行执行。                                             |
    | [`description`](#hooks-description)<span id="hooks-description"></span>                                              | （可选，默认为 `''`）钩子的描述。仅用于元数据目的。                                                                         |
    | [`language_version`](#hooks-language_version)<span id="hooks-language_version"></span>                               | （可选，默认为 `default`）参见[覆盖语言版本](./advanced.md#覆盖语言版本)。                                                  |
    | [`minimum_pre_commit_version`](#hooks-minimum_pre_commit_version)<span id="hooks-minimum_pre_commit_version"></span> | （可选，默认为 `'0'`）允许指示最低兼容的 pre-commit 版本。                                                                  |
    | [`args`](#hooks-args)<span id="hooks-args"></span>                                                                   | （可选，默认为 `[]`）要传递给钩子的附加参数列表。                                                                           |
    | [`stages`](#hooks-stages)<span id="hooks-stages"></span>                                                             | （可选，默认为所有阶段）选择要运行的 git 钩子。参见[限制钩子在特定阶段运行](./advanced.md#限制钩子在某些阶段运行)。         |
    
    例如：
    
    ```yaml
    -   id: trailing-whitespace
        name: Trim Trailing Whitespace
        description: This hook trims trailing whitespace.
        entry: trailing-whitespace-fixer
        language: python
        types: [text]
    ```
    
    
=== "英文"

    pre-commit currently supports hooks written in
    [many languages](#supported-languages). As long as your git repo is an
    installable package (gem, npm, pypi, etc.) or exposes an executable, it can be
    used with pre-commit. Each git repo can support as many languages/hooks as you
    want.
    
    The hook must exit nonzero on failure or modify files.
    
    A git repo containing pre-commit plugins must contain a `.pre-commit-hooks.yaml`
    file that tells pre-commit:
    
    ```table
    =r=
        =c= [`id`](_#hooks-id)
        =c= the id of the hook - used in pre-commit-config.yaml.
    =r=
        =c= [`name`](_#hooks-name)
        =c= the name of the hook - shown during hook execution.
    =r=
        =c= [`entry`](_#hooks-entry)
        =c= the entry point - the executable to run.  `entry` can also contain
            arguments that will not be overridden such as `entry: autopep8 -i`.
    =r=
        =c= [`language`](_#hooks-language)
        =c= the language of the hook - tells pre-commit how to install the hook.
    =r=
        =c= [`files`](_#hooks-files)
        =c= (optional: default `''`) the pattern of files to run on.
    =r=
        =c= [`exclude`](_#hooks-exclude)
        =c= (optional: default `^$`)  exclude files that were matched by [`files`](#hooks-files).
    =r=
        =c= [`types`](_#hooks-types)
        =c= (optional: default `[file]`)  list of file types to run on (AND).  See
            [Filtering files with types](#filtering-files-with-types).
    =r=
        =c= [`types_or`](_#hooks-types_or)
        =c= (optional: default `[]`)  list of file types to run on (OR).  See
            [Filtering files with types](#filtering-files-with-types).
            _new in 2.9.0_.
    =r=
        =c= [`exclude_types`](_#hooks-exclude_types)
        =c= (optional: default `[]`)  the pattern of files to exclude.
    =r=
        =c= [`always_run`](_#hooks-always_run)
        =c= (optional: default `false`) if `true` this hook will run even if there
            are no matching files.
    =r=
        =c= [`fail_fast`](_#hooks-fail_fast)
        =c= (optional: default `false`) if `true` pre-commit will stop running
            hooks if this hook fails.  _new in 2.16.0_.
    =r=
        =c= [`verbose`](_#hooks-verbose)
        =c= (optional: default `false`) if `true`, forces the output of the hook to be printed even when
            the hook passes.
    =r=
        =c= [`pass_filenames`](_#hooks-pass_filenames)
        =c= (optional: default `true`) if `false` no filenames will be passed to
            the hook.
    =r=
        =c= [`require_serial`](_#hooks-require_serial)
        =c= (optional: default `false`) if `true` this hook will execute using a
            single process instead of in parallel.
    =r=
        =c= [`description`](_#hooks-description)
        =c= (optional: default `''`) description of the hook.  used for metadata
            purposes only.
    =r=
        =c= [`language_version`](_#hooks-language_version)
        =c= (optional: default `default`) see
            [Overriding language version](#overriding-language-version).
    =r=
        =c= [`minimum_pre_commit_version`](_#hooks-minimum_pre_commit_version)
        =c= (optional: default `'0'`) allows one to indicate a minimum
            compatible pre-commit version.
    =r=
        =c= [`args`](_#hooks-args)
        =c= (optional: default `[]`) list of additional parameters to pass to the hook.
    =r=
        =c= [`stages`](_#hooks-stages)
        =c= (optional: default (all stages)) selects which git hook(s) to run for.
            See [Confining hooks to run at certain stages](#confining-hooks-to-run-at-certain-stages).
    
    ```
    
    For example:
    
    ```yaml
    -   id: trailing-whitespace
        name: Trim Trailing Whitespace
        description: This hook trims trailing whitespace.
        entry: trailing-whitespace-fixer
        language: python
        types: [text]
    ```

## 以交互方式开发钩子

**Developing hooks interactively**

=== "中文"
    
    由于 `.pre-commit-config.yaml` 中的 [`repo`](./cli.md#repos-repo) 属性可以指向 `git clone ...` 理解的任何内容，因此在开发钩子时，通常很有用的做法是将其指向一个本地目录。

    [`pre-commit try-repo`](./cli.md#pre-commit-try-repo-repo-options) 简化了这一过程，提供了一种快速尝试仓库的方法。以下是如何交互式地使用它：
    
    _注意_：当使用钩子类型 `prepare-commit-msg` 和 `commit-msg` 时，可能需要提供 `--commit-msg-filename` 参数。
    
    对本地目录使用 `try-repo` 不需要提交。`pre-commit` 会克隆任何已跟踪但未提交的更改。
    
    ```pre-commit
    ~/work/hook-repo $ git checkout origin/main -b feature
    
    # ... 做一些更改
    
    # 在另一个终端或标签页
    
    ~/work/other-repo $ pre-commit try-repo ../hook-repo foo --verbose --all-files
    ===============================================================================
    Using config:
    ===============================================================================
    repos:
    -   repo: ../hook-repo
        rev: 84f01ac09fcd8610824f9626a590b83cfae9bcbd
        hooks:
        -   id: foo
    ===============================================================================
    [INFO] Initializing environment for ../hook-repo.
    Foo......................................................................Passed
    - hook id: foo
    - duration: 0.02s
    
    Hello from foo hook!
    ```

=== "英文"

    Since the [`repo`](#repos-repo) property of `.pre-commit-config.yaml` can refer to anything
    that `git clone ...` understands, it's often useful to point it at a local
    directory while developing hooks.
    
    [`pre-commit try-repo`](#pre-commit-try-repo) streamlines this process by
    enabling a quick way to try out a repository.  Here's how one might work
    interactively:
    
    _note_: you may need to provide `--commit-msg-filename` when using this
    command with hook types `prepare-commit-msg` and `commit-msg`.
    
    a commit is not necessary to `try-repo` on a local
    directory. `pre-commit` will clone any tracked uncommitted changes.
    
    ```pre-commit
    ~/work/hook-repo $ git checkout origin/main -b feature
    
    # ... make some changes
    
    # In another terminal or tab
    
    ~/work/other-repo $ pre-commit try-repo ../hook-repo foo --verbose --all-files
    ===============================================================================
    Using config:
    ===============================================================================
    repos:
    -   repo: ../hook-repo
        rev: 84f01ac09fcd8610824f9626a590b83cfae9bcbd
        hooks:
        -   id: foo
    ===============================================================================
    [INFO] Initializing environment for ../hook-repo.
    Foo......................................................................Passed
    - hook id: foo
    - duration: 0.02s
    
    Hello from foo hook!
    
    ```

## 支持的语言

**Supported languages**

=== "中文"

    - [conda](#conda)
    - [coursier](#coursier)
    - [dart](#dart)
    - [docker](#docker)
    - [docker\_image](#docker_image)
    - [dotnet](#dotnet)
    - [fail](#fail)
    - [golang](#golang)
    - [haskell](#haskell)
    - [lua](#lua)
    - [node](#node)
    - [perl](#perl)
    - [python](#python)
    - [python\_venv](#python_venv)
    - [r](#r)
    - [ruby](#ruby)
    - [rust](#rust)
    - [swift](#swift)
    - [pygrep](#pygrep)
    - [script](#script)
    - [system](#system)

=== "英文"

    - [conda](#conda)
    - [coursier](#coursier)
    - [dart](#dart)
    - [docker](#docker)
    - [docker\_image](#docker_image)
    - [dotnet](#dotnet)
    - [fail](#fail)
    - [golang](#golang)
    - [haskell](#haskell)
    - [lua](#lua)
    - [node](#node)
    - [perl](#perl)
    - [python](#python)
    - [python\_venv](#python_venv)
    - [r](#r)
    - [ruby](#ruby)
    - [rust](#rust)
    - [swift](#swift)
    - [pygrep](#pygrep)
    - [script](#script)
    - [system](#system)

### conda

=== "中文"

    钩子仓库必须包含一个 `environment.yml` 文件，该文件将通过 `conda env create --file environment.yml ...` 用于创建环境。

    `conda` 语言还支持 [`additional_dependencies`](./plugins.md#config-additional_dependencies)，并将任何值直接传递给 `conda install`。因此，这种语言可以与 [local](./advanced.md#本地仓库钩子) 钩子一起使用。
    
    _2.17.0 版本新增_：可以通过设置环境变量 `PRE_COMMIT_USE_MAMBA=1` 或 `PRE_COMMIT_USE_MICROMAMBA=1` 使用 `mamba` 或 `micromamba` 进行安装。
    
    __支持：__只要系统安装了 `conda` 二进制文件（例如 [`miniconda`](https://docs.conda.io/en/latest/miniconda.html)），`conda` 钩子就可以工作。它已在 Linux、macOS 和 Windows 上进行了测试。
    

=== "英文"

    The hook repository must contain an `environment.yml` file which will be used
    via `conda env create --file environment.yml ...` to create the environment.
    
    The `conda` language also supports [`additional_dependencies`](./plugins.md#config-additional_dependencies)
    and will pass any of the values directly into `conda install`.  This language can therefore be
    used with [local](./advanced.md#本地仓库钩子) hooks.
    
    _new in 2.17.0_: `mamba` or `micromamba` can be used to install instead via the
    `PRE_COMMIT_USE_MAMBA=1` or `PRE_COMMIT_USE_MICROMAMBA=1` environment
    variables.
    
    __Support:__ `conda` hooks work as long as there is a system-installed `conda`
    binary (such as [`miniconda`](https://docs.conda.io/en/latest/miniconda.html)).
    It has been tested on linux, macOS, and windows.

### coursier

=== "中文"

    新特性 2.8.0：

    钩子仓库必须包含一个 `.pre-commit-channel` 文件夹，该文件夹必须包含用于安装钩子的 Coursier [应用程序描述符](https://get-coursier.io/docs/2.0.0-RC6-10/cli-install.html#application-descriptor-reference)。对于配置 Coursier 钩子，你的 [`entry`](#hooks-entry) 应该对应于从仓库的 `.pre-commit-channel` 文件夹安装的可执行文件。
    
    支持情况：已知 `coursier` 钩子可以在安装了 `cs` 或 `coursier` 包管理器的任何系统上工作。你安装的特定 Coursier 应用程序可能依赖于不同版本的 JVM，请查阅钩子的文档以获取澄清。它已在 Linux 上进行了测试。
    
    新特性 2.18.0：pre-commit 现在支持 `coursier` 包管理器可执行文件的命名。
    
    新特性 3.0.0：`language: coursier` 钩子现在支持 `repo: local` 和 `additional_dependencies`。
    

=== "英文"

    _new in 2.8.0_
    
    The hook repository must have a `.pre-commit-channel` folder and that folder
    must contain the coursier
    [application descriptors](https://get-coursier.io/docs/2.0.0-RC6-10/cli-install.html#application-descriptor-reference)
    for the hook to install. For configuring coursier hooks, your
    [`entry`](#hooks-entry) should correspond to an executable installed from the
    repository's `.pre-commit-channel` folder.
    
    __Support:__ `coursier` hooks are known to work on any system which has the
    `cs` or `coursier` package manager installed. The specific coursier
    applications you install may depend on various versions of the JVM, consult
    the hooks' documentation for clarification.  It has been tested on linux.
    
    _new in 2.18.0_: pre-commit now supports the `coursier` naming of the package
    manager executable.
    
    _new in 3.0.0_: `language: coursier` hooks now support `repo: local` and
    `additional_dependencies`.

### dart

=== "中文"

    新特性 2.15.0：

    钩子仓库必须包含一个 `pubspec.yaml` 文件——这个文件必须包含一个 `executables` 部分，这部分将列出安装后可用的二进制文件。将 [`entry`](#hooks-entry) 与某个可执行文件匹配。
    
    `pre-commit` 将使用 `dart compile exe bin/{executable}.dart` 构建每个可执行文件。
    
    `language: dart` 还支持 [`additional_dependencies`](./plugins.md#config-additional_dependencies) 来指定依赖项的版本，通过 `:` 分隔包名：
    
    ```yaml
            additional_dependencies: ['hello_world_dart:1.0.0']
    ```
    
    支持情况：已知 `dart` 钩子可以在安装了 `dart` SDK 的任何系统上工作。它已在 Linux、macOS 和 Windows 上进行了测试。
    

=== "英文"

    _new in 2.15.0_
    
    The hook repository must have a `pubspec.yaml` -- this must contain an
    `executables` section which will list the binaries that will be available
    after installation.  Match the [`entry`](#hooks-entry) to an executable.
    
    `pre-commit` will build each executable using `dart compile exe bin/{executable}.dart`.
    
    `language: dart` also supports [`additional_dependencies`](./plugins.md#config-additional_dependencies).
    to specify a version for a dependency, separate the package name by a `:`:
    
    ```yaml
            additional_dependencies: ['hello_world_dart:1.0.0']
    ```
    
    __Support:__ `dart` hooks are known to work on any system which has the `dart`
    sdk installed.  It has been tested on linux, macOS, and windows.

### docker

=== "中文"

    钩子仓库必须包含一个 `Dockerfile`。它将通过 `docker build .` 进行安装。

    运行 Docker 钩子需要主机上有一个运行中的 Docker 引擎。对于配置 Docker 钩子，你的 [`entry`](#hooks-entry) 应该对应于 Docker 容器内的某个可执行文件，并用于覆盖默认容器的入口点。当 pre-commit 向运行容器命令传递文件列表作为参数时，你的 Docker `CMD` 不会运行。Docker 允许你使用任何 pre-commit 没有内置支持的语言。
    
    pre-commit 将自动将仓库源代码作为卷挂载使用 `-v $PWD:/src:rw,Z`，并使用 `--workdir /src` 设置工作目录。
    
    支持情况：已知 docker 钩子可以在有可用的 `docker` 可执行文件的任何系统上工作。它已在 Linux 和 macOS 上进行了测试。通过 `boot2docker` 运行的钩子已知无法对文件进行修改。
    
    可以查看 [这个仓库](https://github.com/pre-commit/pre-commit-docker-flake8) 来获取一个基于 Docker 的钩子示例。
    

=== "英文"

    The hook repository must have a `Dockerfile`.  It will be installed via
    `docker build .`.
    
    Running Docker hooks requires a running Docker engine on your host.  For
    configuring Docker hooks, your [`entry`](#hooks-entry) should correspond to an executable
    inside the Docker container, and will be used to override the default container
    entrypoint. Your Docker `CMD` will not run when pre-commit passes a file list
    as arguments to the run container command. Docker allows you to use any
    language that's not supported by pre-commit as a builtin.
    
    pre-commit will automatically mount the repository source as a volume using
    `-v $PWD:/src:rw,Z` and set the working directory using `--workdir /src`.
    
    __Support:__ docker hooks are known to work on any system which has a working
    `docker` executable.  It has been tested on linux and macOS.  Hooks that are
    run via `boot2docker` are known to be unable to make modifications to files.
    
    See [this repository](https://github.com/pre-commit/pre-commit-docker-flake8)
    for an example Docker-based hook.

### docker_image

=== "中文"

=== "英文"

    相对于 `docker` 钩子，`docker_image` 提供了一种更轻量级的方法。`docker_image` "语言"使用现有的 Docker 镜像来提供钩子可执行文件。
    
    `docker_image` 钩子可以方便地配置为 [local](./advanced.md#本地仓库钩子) 钩子。
    
    [`entry`](#hooks-entry) 指定了要使用的 Docker 标签。如果镜像定义了 `ENTRYPOINT`，不需要特别操作就可以连接到可执行文件。如果容器没有指定 `ENTRYPOINT` 或者你想要更改入口点，你可以在 [`entry`](#hooks-entry) 中指定它。
    
    例如：
    
    ```yaml
    -   id: dockerfile-provides-entrypoint
        name: ...
        language: docker_image
        entry: my.registry.example.com/docker-image-1:latest
    -   id: dockerfile-no-entrypoint-1
        name: ...
        language: docker_image
        entry: --entrypoint my-exe my.registry.example.com/docker-image-2:latest
    # 另一种等效的解决方案
    -   id: dockerfile-no-entrypoint-2
        name: ...
        language: docker_image
        entry: my.registry.example.com/docker-image-3:latest my-exe
    ```
    
    在这个配置中：
    
    - `dockerfile-provides-entrypoint` 使用了带有预定义 `ENTRYPOINT` 的镜像。
    - `dockerfile-no-entrypoint-1` 和 `dockerfile-no-entrypoint-2` 指定了要使用的可执行文件 `my-exe`，这在镜像没有定义 `ENTRYPOINT` 或需要覆盖 `ENTRYPOINT` 时很有用。
    

### dotnet

=== "中文"

    新特性 2.8.0：

    dotnet 钩子使用系统安装的 dotnet CLI 进行安装。
    
    钩子仓库必须包含一个可以按照[这个](https://docs.microsoft.com/en-us/dotnet/core/tools/global-tools-how-to-create)示例进行 `pack` 和 `install` 的 dotnet CLI 工具。`entry` 应该匹配构建仓库后创建的可执行文件。目前不支持附加依赖项。
    
    支持情况：已知 dotnet 钩子可以在安装了 dotnet CLI 的任何系统上工作。它已在 Linux 和 Windows 上进行了测试。
    

=== "英文"

    _new in 2.8.0_
    
    dotnet hooks are installed using the system installation of the dotnet CLI.
    
    Hook repositories must contain a dotnet CLI tool which can be `pack`ed and
    `install`ed as per [this](https://docs.microsoft.com/en-us/dotnet/core/tools/global-tools-how-to-create)
    example. The `entry` should match an executable created by building the
    repository. Additional dependencies are not currently supported.
    
    __Support:__ dotnet hooks are known to work on any system which has the dotnet
    CLI installed.  It has been tested on linux and windows.

### fail

=== "中文"

    `fail` 是一种轻量级的 [`language`](#hooks-language)，用于通过文件名禁止文件。`fail` 语言尤其适用于 [local](./advanced.md#本地仓库钩子) 钩子。
    
    当钩子失败时，[`entry`](#hooks-entry) 将会被打印出来。建议在 [`name`](#hooks-name) 中提供简短的描述，并在 [`entry`](#hooks-entry) 中提供更详细的修复说明。
    
    以下是一个示例，它防止除了以 `.rst` 结尾的文件之外的任何文件被添加到 `changelog` 目录：
    
    ```yaml
    -   repo: local
        hooks:
        -   id: changelogs-rst
            name: changelogs must be rst
            entry: 'changelog filenames must end in .rst'
            language: fail
            files: 'changelog/.*(?<!\.rst)$'
    ```
    
    在这个配置中：
    
    - `id` 是钩子的唯一标识符。
    - `name` 描述了钩子的用途。
    - `entry` 指定了当钩子失败时打印的消息。
    - `language` 设置为 `fail`，表示这是一个失败的钩子。
    - `files` 指定了匹配文件的模式，这里使用了正则表达式来匹配不以 `.rst` 结尾的文件。
    

=== "英文"

    A lightweight [`language`](#hooks-language) to forbid files by filename.  The `fail` language is
    especially useful for [local](./advanced.md#本地仓库钩子) hooks.
    
    The [`entry`](#hooks-entry) will be printed when the hook fails.  It is suggested to provide
    a brief description for [`name`](#hooks-name) and more verbose fix instructions in [`entry`](#hooks-entry).
    
    Here's an example which prevents any file except those ending with `.rst` from
    being added to the `changelog` directory:
    
    ```yaml
    -   repo: local
        hooks:
        -   id: changelogs-rst
            name: changelogs must be rst
            entry: changelog filenames must end in .rst
            language: fail
            files: 'changelog/.*(?<!\.rst)$'
    ```

### golang

=== "中文"

    钩子仓库必须包含 Go 源代码。它将通过 `go install ./...` 进行安装。pre-commit 将为每个钩子创建一个隔离的 `GOPATH`，[`entry`](#hooks-entry) 应该匹配将要安装到 `GOPATH` 的 `bin` 目录中的可执行文件。
    
    这种语言支持 `additional_dependencies` 并将任何值直接传递给 `go install`。它可以用作 `repo: local` 钩子。
    
    _2.17.0 版本更改_：之前使用的是 `go get ./...`。
    
    _3.0.0 版本新增_：如果未安装，pre-commit 将引导安装 `go`。`language: golang` 现在还支持 `language_version`。
    
    支持情况：已知 golang 钩子可以在安装了 go 的任何系统上工作。它已在 Linux、macOS 和 Windows 上进行了测试。
    

=== "英文"

    The hook repository must contain go source code.  It will be installed via
    `go install ./...`.  pre-commit will create an isolated `GOPATH` for each hook
    and the [`entry`](#hooks-entry) should match an executable which will get installed into the
    `GOPATH`'s `bin` directory.
    
    This language supports `additional_dependencies` and will pass any of the values directly to `go
    install`. It can be used as a `repo: local` hook.
    
    _changed in 2.17.0_: previously `go get ./...` was used
    
    _new in 3.0.0_: pre-commit will bootstrap `go` if it is not present. `language: golang`
    also now supports `language_version`
    
    __Support:__ golang hooks are known to work on any system which has go
    installed.  It has been tested on linux, macOS, and windows.

### haskell

=== "中文"
    
    新特性 3.4.0：

    钩子仓库必须包含一个或多个 `*.cabal` 文件。安装后这些包中的 `executable` 将可用于 [`entry`](#hooks-entry)。
    
    这种语言支持 `additional_dependencies`，因此它可以用作 `repo: local` 钩子。
    
    支持情况：已知 haskell 钩子可以在安装了 `cabal` 的任何系统上工作。它已在 Linux、macOS 和 Windows 上进行了测试。

=== "英文"

    _new in 3.4.0_
    
    The hook repository must have one or more `*.cabal` files.  Once installed
    the `executable`s from these packages will be available to use with `entry`.
    
    This language supports `additional_dependencies` so it can be used as a
    `repo: local` hook.
    
    __Support:__ haskell hooks are known to work on any system which has `cabal`
    installed.  It has been tested on linux, macOS, and windows.

### lua

=== "中文"

    新特性 2.17.0：

    Lua 钩子使用 Luarocks 使用的 Lua 版本进行安装。
    
    支持情况：已知 Lua 钩子可以在安装了 Luarocks 的任何系统上工作。它已在 Linux 和 macOS 上进行了测试，并且可能在 Windows 上也能工作。
    

=== "英文"

    _new in 2.17.0_
    
    Lua hooks are installed with the version of Lua that is used by Luarocks.
    
    __Support:__ Lua hooks are known to work on any system which has Luarocks
    installed.  It has been tested on linux and macOS and _may_ work on windows.

### node

=== "中文"
    
    钩子仓库必须包含一个 `package.json` 文件。它将通过 `npm install .` 进行安装。安装的包将提供一个可执行文件，该文件将匹配 [`entry`](#hooks-entry) —— 通常是通过 package.json 中的 `bin` 提供。
    
    支持情况：node 钩子不需要任何系统级依赖项即可工作。它已在 Linux、Windows 和 macOS 上进行了测试，并且可能在 cygwin 下也能工作。
    

=== "英文"

    The hook repository must have a `package.json`.  It will be installed via
    `npm install .`.  The installed package will provide an executable that will
    match the [`entry`](#hooks-entry) – usually through `bin` in package.json.
    
    __Support:__ node hooks work without any system-level dependencies.  It has
    been tested on linux, windows, and macOS and _may_ work under cygwin.

### perl

=== "中文"
    
    新特性 2.1.0：

    Perl 钩子使用 Perl 自带的 CPAN 包安装程序 [cpan](https://perldoc.perl.org/cpan) 进行安装。
    
    钩子仓库必须包含 `cpan` 支持的内容，通常是 `Makefile.PL` 或 `Build.PL`，它用这些文件来安装要在钩子的 [`entry`](#hooks-entry) 定义中使用的可执行文件。仓库将通过 `cpan -T .` 进行安装（安装的文件存储在您的 pre-commit 缓存中，不会污染其他 Perl 安装）。
    
    当为 Perl 指定 [`additional_dependencies`](./plugins.md#config-additional_dependencies) 时，您可以使用 [`cpan`](https://perldoc.perl.org/CPAN#get%2c-make%2c-test%2c-install%2c-clean-modules-or-distributions) 理解的任何[安装参数格式](https://perldoc.perl.org/CPAN#get%2c-make%2c-test%2c-install%2c-clean-modules-or-distributions)。
    
    支持情况：Perl 钩子当前需要预先存在的 Perl 安装，包括在 `PATH` 中的 `cpan` 工具。它已在 Linux、macOS 和 Windows 上进行了测试。


=== "英文"

    _new in 2.1.0_
    
    Perl hooks are installed using the system installation of
    [cpan](https://perldoc.perl.org/cpan), the CPAN package installer
    that comes with Perl.
    
    Hook repositories must have something that `cpan` supports, typically
    `Makefile.PL` or `Build.PL`, which it uses to install an executable to
    use in the [`entry`](#hooks-entry) definition for your hook. The repository will be installed
    via `cpan -T .` (with the installed files stored in your pre-commit cache,
    not polluting other Perl installations).
    
    When specifying [`additional_dependencies`](./plugins.md#config-additional_dependencies) for Perl, you can use any of the
    [install argument formats understood by `cpan`](https://perldoc.perl.org/CPAN#get%2c-make%2c-test%2c-install%2c-clean-modules-or-distributions).
    
    __Support:__ Perl hooks currently require a pre-existing Perl installation,
    including the `cpan` tool in `PATH`.  It has been tested on linux, macOS, and
    Windows.

### python

=== "中文"

    钩子仓库必须能够通过 `pip install .` 进行安装（通常使用 `setup.py` 或 `pyproject.toml`）。安装的包将提供一个可执行文件，该文件将匹配 [`entry`](#hooks-entry) —— 通常是通过 `setup.py` 中的 `console_scripts` 或 `scripts`。

    这种语言还支持 `additional_dependencies`，因此它可以与 [local](./advanced.md#本地仓库钩子) 钩子一起使用。指定的依赖项将被追加到 `pip install` 命令中。
    
    支持情况：python 钩子不需要任何系统级依赖项即可工作。它已在 Linux、macOS、Windows 和 cygwin 上进行了测试。
    

=== "英文"

    The hook repository must be installable via `pip install .` (usually by either
    `setup.py` or `pyproject.toml`).  The installed package will provide an
    executable that will match the [`entry`](#hooks-entry) – usually through `console_scripts` or
    `scripts` in setup.py.
    
    This language also supports `additional_dependencies`
    so it can be used with [local](./advanced.md#本地仓库钩子) hooks.
    The specified dependencies will be appended to the `pip install` command.
    
    __Support:__ python hooks work without any system-level dependencies.  It
    has been tested on linux, macOS, windows, and cygwin.

### python_venv

=== "中文"

    新特性 2.4.0：

    `python_venv` 语言现在是 `python` 的别名，因为 `virtualenv>=20` 创建的结构相同的环境。以前，这个 [`language`](#hooks-language) 使用 [venv] 模块创建环境。
    
    这个 [`language`](#hooks-language) 将最终被移除，因此建议使用 `python`。
    
    [venv]: https://docs.python.org/3/library/venv.html
    
    支持情况：python 钩子不需要任何系统级依赖项即可工作。它已在 Linux、macOS、Windows 和 cygwin 上进行了测试。
    

=== "英文"

    _new in 2.4.0_: The `python_venv` language is now an alias to `python` since
    `virtualenv>=20` creates equivalently structured environments.  Previously,
    this [`language`](#hooks-language) created environments using the [venv] module.
    
    This [`language`](#hooks-language) will be removed eventually so it is suggested to use `python`
    instead.
    
    [venv]: https://docs.python.org/3/library/venv.html
    
    __Support:__ python hooks work without any system-level dependencies.  It
    has been tested on linux, macOS, windows, and cygwin.

### r

=== "中文"

    新特性 2.11.0：

    这个钩子仓库必须包含一个 `renv.lock` 文件，该文件将在钩子安装时通过 [`renv::restore()`](https://rstudio.github.io/renv/reference/restore.html) 恢复。如果仓库是一个 R 包（即在 `DESCRIPTION` 中有 `Type: Package`），它将被安装。在 [`entry`](#hooks-entry) 中支持的语法是 `Rscript -e {expression}` 或 `Rscript path/relative/to/hook/root`。跳过 R 启动进程（模拟 `--vanilla`），因为所有配置应通过 [`args`](#hooks-args) 公开，以实现最大透明度和可移植性。
    
    当为 R 指定 [`additional_dependencies`](./plugins.md#config-additional_dependencies) 时，您可以使用任何 [`renv::install()`](https://rstudio.github.io/renv/reference/install.html#examples) 理解的安装参数格式。
    
    支持情况：只要安装了 [`R`](https://www.r-project.org) 并在 `PATH` 上，`r` 钩子就可以工作。它已在 Linux、macOS 和 Windows 上进行了测试。
    

=== "英文"

    _new in 2.11.0_
    
    This hook repository must have a `renv.lock` file that will be restored with
    [`renv::restore()`](https://rstudio.github.io/renv/reference/restore.html) on
    hook installation. If the repository is an R package (i.e. has `Type: Package`
    in `DESCRIPTION`), it is installed. The supported syntax in [`entry`](#hooks-entry) is
    `Rscript -e {expression}` or `Rscript path/relative/to/hook/root`. The
    R Startup process is skipped (emulating `--vanilla`), as all configuration
    should be exposed via [`args`](#hooks-args) for maximal transparency and portability.
    
    When specifying [`additional_dependencies`](./plugins.md#config-additional_dependencies)
    for R, you can use any of the install argument formats understood by
    [`renv::install()`](https://rstudio.github.io/renv/reference/install.html#examples).
    
    __Support:__ `r` hooks work as long as [`R`](https://www.r-project.org) is
    installed and on `PATH`. It has been tested on linux, macOS, and windows.


### ruby

=== "中文"
    
    钩子仓库必须包含一个 `*.gemspec` 文件。它将通过 `gem build *.gemspec && gem install *.gem` 进行安装。安装的包将生成一个可执行文件，该文件将匹配 [`entry`](#hooks-entry) —— 通常是通过你在 gemspec 中的 `executables` 指定的。

    支持情况：ruby 钩子不需要任何系统级依赖项即可工作。它已在 Linux 和 macOS 上进行了测试，并且可能在 cygwin 下也能工作。


=== "英文"

    The hook repository must have a `*.gemspec`.  It will be installed via
    `gem build *.gemspec && gem install *.gem`.  The installed package will
    produce an executable that will match the [`entry`](#hooks-entry) – usually through
    `executables` in your gemspec.
    
    __Support:__ ruby hooks work without any system-level dependencies.  It has
    been tested on linux and macOS and _may_ work under cygwin.

### rust

=== "中文"

    Rust 钩子使用 [Cargo](https://github.com/rust-lang/cargo)，Rust 的官方包管理器进行安装。
    
    钩子仓库必须包含一个 `Cargo.toml` 文件，该文件至少生成一个二进制文件（例如：[example-rust-pre-commit-hook](https://github.com/chriskuehl/example-rust-pre-commit-hook)），其名称应与钩子的 [`entry`](#hooks-entry) 定义匹配。仓库将通过 `cargo install --bins` 进行安装（二进制文件存储在 pre-commit 缓存中，不会污染您的用户级 Cargo 安装）。
    
    当为 Rust 指定 [`additional_dependencies`](./plugins.md#config-additional_dependencies) 时，您可以使用语法 `{package_name}:{package_version}` 来指定一个新的库依赖项（用于构建 _你的_ 钩子仓库），或者使用特殊语法 `cli:{package_name}:{package_version}` 用于 CLI 依赖项（单独构建，其生成的二进制文件可供钩子使用）。
    
    新特性 2.21.0：如果未安装，pre-commit 将引导安装 `rust`。`language: rust` 现在还支持 `language_version`。
    
    支持情况：它已在 Linux、Windows 和 macOS 上进行了测试。
    

=== "英文"

    Rust hooks are installed using [Cargo](https://github.com/rust-lang/cargo),
    Rust's official package manager.
    
    Hook repositories must have a `Cargo.toml` file which produces at least one
    binary ([example](https://github.com/chriskuehl/example-rust-pre-commit-hook)),
    whose name should match the [`entry`](#hooks-entry) definition for your hook. The repo will be
    installed via `cargo install --bins` (with the binaries stored in your
    pre-commit cache, not polluting your user-level Cargo installations).
    
    When specifying [`additional_dependencies`](./plugins.md#config-additional_dependencies) for Rust, you can use the syntax
    `{package_name}:{package_version}` to specify a new library dependency (used to
    build _your_ hook repo), or the special syntax
    `cli:{package_name}:{package_version}` for a CLI dependency (built separately,
    with binaries made available for use by hooks).
    
    _new in 2.21.0_: pre-commit will bootstrap `rust` if it is not present.
    `language: rust` also now supports `language_version`
    
    __Support:__ It has been tested on linux, Windows, and macOS.

### swift

=== "中文"
    
    钩子仓库必须包含一个 `Package.swift` 文件。它将通过 `swift build -c release` 进行安装。[`entry`](#hooks-entry) 应该匹配构建仓库时创建的可执行文件。

    支持情况：已知 swift 钩子可以在安装了 swift 的任何系统上工作。它已在 Linux 和 macOS 上进行了测试。


=== "英文"

    The hook repository must have a `Package.swift`.  It will be installed via
    `swift build -c release`.  The [`entry`](#hooks-entry) should match an executable created by
    building the repository.
    
    __Support:__ swift hooks are known to work on any system which has swift
    installed.  It has been tested on linux and macOS.

### pygrep

=== "中文"
    
    跨平台的 Python 实现 `grep` —— pygrep 钩子是编写简单钩子的快速方法，通过文件匹配防止提交。将正则表达式指定为 [`entry`](#hooks-entry)。[`entry`](#hooks-entry) 可以是任何 Python [正则表达式](./advanced.md#正则表达式)。对于不区分大小写的正则表达式，可以在条目的开头使用 `(?i)` 标志，或者使用 `args: [-i]`。

    对于多行匹配，请使用 `args: [--multiline]`。
    
    新特性 2.8.0：要要求所有文件匹配，请使用 `args: [--negate]`。
    
    支持情况：pygrep 钩子支持 pre-commit 运行的所有平台。


=== "英文"

    A cross-platform python implementation of `grep` – pygrep hooks are a quick
    way to write a simple hook which prevents commits by file matching.  Specify
    the regex as the [`entry`](#hooks-entry).  The [`entry`](#hooks-entry) may be any python
    [regular expression](./advanced.md#正则表达式).  For case insensitive regexes you
    can apply the `(?i)` flag as the start of your entry, or use `args: [-i]`.
    
    For multiline matches, use `args: [--multiline]`.
    
    _new in 2.8.0_: To require all files to match, use `args: [--negate]`.
    
    __Support:__ pygrep hooks are supported on all platforms which pre-commit runs
    on.

### script

=== "中文"

    脚本钩子提供了一种编写验证文件的简单脚本的方法。[`entry`](#hooks-entry) 应该是相对于钩子仓库根目录的路径。

    这种类型的钩子不会提供一个虚拟环境来工作——如果它需要额外的依赖项，使用者必须手动安装它们。
    
    支持情况：脚本钩子的支持取决于脚本本身。


=== "英文"

    Script hooks provide a way to write simple scripts which validate files. The
    [`entry`](#hooks-entry) should be a path relative to the root of the hook repository.
    
    This hook type will not be given a virtual environment to work with – if it
    needs additional dependencies the consumer must install them manually.
    
    __Support:__ the support of script hooks depend on the scripts themselves.

### system

=== "中文"
    
    系统钩子提供了一种编写系统级可执行文件钩子的方法，这些可执行文件在上面没有支持的语言（或者有特殊的环境要求，不允许它们独立运行，例如 pylint）。

    这种类型的钩子不会提供一个虚拟环境来工作——如果它需要额外的依赖项，使用者必须手动安装它们。
    
    支持情况：系统钩子的支持取决于可执行文件本身。


=== "英文"

    System hooks provide a way to write hooks for system-level executables which
    don't have a supported language above (or have special environment
    requirements that don't allow them to run in isolation such as pylint).
    
    This hook type will not be given a virtual environment to work with – if it
    needs additional dependencies the consumer must install them manually.
    
    __Support:__ the support of system hooks depend on the executables.
