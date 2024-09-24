# 高级功能

**Advanced features**

## 迁移模式(migration)下运行

**Running in migration mode**

=== "中文"
    
    默认情况下，如果您已存在钩子，`pre-commit install` 将以迁移模式安装，它会同时运行您现有的钩子和 pre-commit 的钩子。
    
    要禁用此行为，请在 `install` 命令中传递 `-f` / `--overwrite` 参数。
    
    如果您决定不使用 pre-commit，`pre-commit uninstall` 将恢复您的钩子到安装前的状态。
    

=== "英文"

    By default, if you have existing hooks `pre-commit install` will install in a
    migration mode which runs both your existing hooks and hooks for pre-commit.
    To disable this behavior, pass `-f` / `--overwrite` to the `install` command.
    If you decide not to use pre-commit, `pre-commit uninstall` will
    restore your hooks to the state prior to installation.

## 暂时关闭钩子

**Temporarily disabling hooks**

=== "中文"
    
    不是所有的钩子都是完美的，所以有时您可能需要跳过一个或多个钩子的执行。pre-commit 通过查询 `SKIP` 环境变量来解决这个问题。`SKIP` 环境变量是一个用逗号分隔的钩子 ID 列表。这允许您跳过单个钩子，而不是使用 `--no-verify` 跳过整个提交。
    
    ```console
    $ SKIP=flake8 git commit -m "foo"
    ```


=== "英文"

    Not all hooks are perfect so sometimes you may need to skip execution of one
    or more hooks. pre-commit solves this by querying a `SKIP` environment
    variable. The `SKIP` environment variable is a comma separated list of hook
    ids. This allows you to skip a single hook instead of `--no-verify`ing the
    entire commit.
    
    ```console
    $ SKIP=flake8 git commit -m "foo"
    ```

## 限制钩子在某些阶段运行

**Confining hooks to run at certain stages**

=== "中文"

    pre-commit 支持许多不同类型的 `git` 钩子（不仅仅是 `pre-commit`！）。

    钩子的提供者可以通过在 `.pre-commit-hooks.yaml` 中设置 [`stages`](./plugins.md#hooks-stages) 属性来选择他们运行的 git 钩子类型——这也可以被 `.pre-commit-config.yaml` 中的 [`stages`](./plugins.md#config-stages) 覆盖。如果在这两个地方都没有设置 `stages`，那么将从顶级的 [`default_stages`](./plugins.md#top_level-default_stages) 选项中获取默认值（默认为所有阶段）。默认情况下，工具会为 pre-commit 支持的每个钩子类型启用。
    
    **新功能 3.2.0**：`stages` 的值与钩子名称匹配。以前，`commit`、`push` 和 `merge-commit` 分别对应 `pre-commit`、`pre-push` 和 `pre-merge-commit`。
    
    通过 `stages: [manual]` 设置的 `manual` 阶段是一个特殊阶段，它不会被任何 `git` 钩子自动触发——这在您想要添加一个不自动运行的工具时很有用，但可以使用 `pre-commit run --hook-stage manual [hookid]` 按需运行。
    
    如果您是工具的作者，通常最好提供一个合适的 `stages` 属性。例如，对于一个 linter 或代码格式化工具来说，合理的设置可能是 `stages: [pre-commit, pre-merge-commit, pre-push, manual]`。
    
    要为特定的 git 钩子安装 `pre-commit`，请向 `pre-commit install` 传递 `--hook-type`。这可以多次指定，例如：
    
    ```console
    $ pre-commit install --hook-type pre-commit --hook-type pre-push
    pre-commit installed at .git/hooks/pre-commit
    pre-commit installed at .git/hooks/pre-push
    ```
    
    此外，可以通过设置顶级的 [`default_install_hook_types`](./plugins.md#top_level-default_install_hook_types) 来指定一组默认的 git 钩子类型进行安装。
    
    例如：
    
    ```yaml
    default_install_hook_types: [pre-commit, pre-push, commit-msg]
    ```
    
    ```console
    $ pre-commit  install
    pre-commit installed at .git/hooks/pre-commit
    pre-commit installed at .git/hooks/pre-push
    pre-commit installed at .git/hooks/commit-msg
    ```
    

=== "英文"

    pre-commit supports many different types of `git` hooks (not just
    `pre-commit`!).
    
    Providers of hooks can select which git hooks they run on by setting the
    [`stages`](#hooks-stages) property in `.pre-commit-hooks.yaml` -- this can
    also be overridden by setting [`stages`](#config-stages) in
    `.pre-commit-config.yaml`.  If `stages` is not set in either of those places
    the default value will be pulled from the top-level
    [`default_stages`](#top_level-default_stages) option (which defaults to _all_
    stages).  By default, tools are enabled for [every hook type](#supported-git-hooks)
    that pre-commit supports.
    
    _new in 3.2.0_: The values of `stages` match the hook names.  Previously,
    `commit`, `push`, and `merge-commit` matched `pre-commit`, `pre-push`, and
    `pre-merge-commit` respectively.
    
    The `manual` stage (via `stages: [manual]`) is a special stage which will not
    be automatically triggered by any `git` hook -- this is useful if you want to
    add a tool which is not automatically run, but is run on demand using
    `pre-commit run --hook-stage manual [hookid]`.
    
    If you are authoring a tool, it is usually a good idea to provide an appropriate
    `stages` property.  For example a reasonable setting for a linter or code
    formatter would be `stages: [pre-commit, pre-merge-commit, pre-push, manual]`.
    
    To install `pre-commit` for particular git hooks, pass `--hook-type` to
    `pre-commit install`.  This can be specified multiple times such as:
    
    ```console
    $ pre-commit install --hook-type pre-commit --hook-type pre-push
    pre-commit installed at .git/hooks/pre-commit
    pre-commit installed at .git/hooks/pre-push
    ```
    
    Additionally, one can specify a default set of git hook types to be installed
    for by setting the top-level [`default_install_hook_types`](#top_level-default_install_hook_types).
    
    For example:
    
    ```yaml
    default_install_hook_types: [pre-commit, pre-push, commit-msg]
    ```
    
    ```console
    $ pre-commit  install
    pre-commit installed at .git/hooks/pre-commit
    pre-commit installed at .git/hooks/pre-push
    pre-commit installed at .git/hooks/commit-msg
    ```
    

## 支持的git钩子

**Supported git hooks**

=== "中文"

=== "英文"

- [commit-msg](#commit-msg)
- [post-checkout](#post-checkout)
- [post-commit](#post-commit)
- [post-merge](#post-merge)
- [post-rewrite](#post-rewrite)
- [pre-commit](#pre-commit)
- [pre-merge-commit](#pre-merge-commit)
- [pre-push](#pre-push)
- [pre-rebase](#pre-rebase)
- [prepare-commit-msg](#prepare-commit-msg)

### commit-msg

=== "中文"
    
    [git commit-msg 文档](https://git-scm.com/docs/githooks#_commit_msg) 

    `commit-msg` 钩子将传递一个文件名参数——此文件包含要验证的当前提交消息的内容。如果退出码非零，提交将被中止。


=== "英文"

    [git commit-msg docs](https://git-scm.com/docs/githooks#_commit_msg)
    
    `commit-msg` hooks will be passed a single filename -- this file contains the
    current contents of the commit message to be validated.  The commit will be
    aborted if there is a nonzero exit code.

### post-checkout

=== "中文"

    **新功能 2.2.0**

    [git post-checkout 文档](https://git-scm.com/docs/githooks#_post_checkout) 
    
    `post-checkout` 钩子在 `checkout` 操作之后运行，可以用来设置或管理仓库中的状态。
    
    `post-checkout` 钩子不操作文件，所以它们必须设置为 `always_run: true`，否则它们将总是被跳过。
    
    环境变量：

    - `PRE_COMMIT_FROM_REF`: `post-checkout` git 钩子的第一个参数
    - `PRE_COMMIT_TO_REF`: `post-checkout` git 钩子的第二个参数
    - `PRE_COMMIT_CHECKOUT_TYPE`: `post-checkout` git 钩子的第三个参数
    

=== "英文"

    _new in 2.2.0_
    
    [git post-checkout docs](https://git-scm.com/docs/githooks#_post_checkout)
    
    post-checkout hooks run *after* a `checkout` has occurred and can be used to
    set up or manage state in the repository.
    
    `post-checkout` hooks do not operate on files so they must be set as
    `always_run: true` or they will always be skipped.
    
    environment variables:
    - `PRE_COMMIT_FROM_REF`: the first argument to the `post-checkout` git hook
    - `PRE_COMMIT_TO_REF`:  the second argument to the `post-checkout` git hook
    - `PRE_COMMIT_CHECKOUT_TYPE`: the third argument to the `post-checkout` git hook

### post-commit

=== "中文"

    **新功能 2.4.0**

    [git post-commit 文档](https://git-scm.com/docs/githooks#_post_commit) 
    
    `post-commit` 钩子在提交已经成功之后运行，因此它不能用来阻止提交发生。
    
    `post-commit` 钩子不操作文件，所以它们必须设置为 `always_run: true`，否则它们将总是被跳过。
    

=== "英文"

    _new in 2.4.0_
    
    [git post-commit docs](https://git-scm.com/docs/githooks#_post_commit)
    
    `post-commit` runs after the commit has already succeeded so it cannot be used
    to prevent the commit from happening.
    
    `post-commit` hooks do not operate on files so they must be set as
    `always_run: true` or they will always be skipped.

### post-merge

=== "中文"

    **新功能 2.11.0**

    [git post-merge 文档](https://git-scm.com/docs/githooks#_post_merge) 
    
    `post-merge` 在 `git merge` 操作成功之后运行。
    
    `post-merge` 钩子不操作文件，所以它们必须设置为 `always_run: true`，否则它们将总是被跳过。
    
    环境变量：

    - `PRE_COMMIT_IS_SQUASH_MERGE`: `post-merge` git 钩子的第一个参数。
    

=== "英文"

    _new in 2.11.0_
    
    [git post-merge docs](https://git-scm.com/docs/githooks#_post_merge)
    
    `post-merge` runs after a successful `git merge`.
    
    `post-merge` hooks do not operate on files so they must be set as
    `always_run: true` or they will always be skipped.
    
    environment variables:
    - `PRE_COMMIT_IS_SQUASH_MERGE`: the first argument to the `post-merge` git hook.

### post-rewrite

=== "中文"

    _new in 2.15.0_

    [git post-rewrite 文档](https://git-scm.com/docs/githooks#_post_rewrite) 
    
    `post-rewrite` 在像 `git commit --amend` 或 `git rebase` 这样修改历史记录的 git 命令之后运行。
    
    `post-rewrite` 钩子不会操作文件，所以它们必须设置为 `always_run: true`，否则它们将总是被跳过。
    
    环境变量：
    
    - `PRE_COMMIT_REWRITE_COMMAND`: `post-rewrite` git 钩子的第一个参数。

=== "英文"

    _new in 2.15.0_
    
    [git post-rewrite docs](https://git-scm.com/docs/githooks#_post_rewrite)
    
    `post-rewrite` runs after a git command which modifies history such as
    `git commit --amend` or `git rebase`.
    
    `post-rewrite` hooks do not operate on files so they must be set as
    `always_run: true` or they will always be skipped.
    
    environment variables:
    
    - `PRE_COMMIT_REWRITE_COMMAND`: the first argument to the `post-rewrite` git hook.

### pre-commit

=== "中文"
    
    [git pre-commit 文档](https://git-scm.com/docs/githooks#_pre_commit) 
    
    `pre-commit` 在提交最终确定之前被触发，以允许对正在提交的代码进行检查。如果在提交过程中对未暂存的更改运行钩子，可能会导致提交期间出现误报和漏报。pre-commit 仅在暂存的文件内容上运行，通过在运行钩子时临时存储未暂存的更改。

=== "英文"

    [git pre-commit docs](https://git-scm.com/docs/githooks#_pre_commit)
    
    `pre-commit` is triggered before the commit is finalized to allow checks on the
    code being committed.  Running hooks on unstaged changes can lead to both
    false-positives and false-negatives during committing.  pre-commit only runs
    on the staged contents of files by temporarily stashing the unstaged changes
    while running hooks.

### pre-merge-commit

=== "中文"


    [git pre-merge-commit 文档](https://git-scm.com/docs/githooks#_pre_merge_commit) 
    
    `pre-merge-commit` 在合并成功后但创建合并提交之前触发。这个钩子在合并的所有暂存文件上运行。
    
    注意，你需要使用至少 git 2.24 版本才能使用这个钩子。


=== "英文"

    [git pre-merge-commit docs](https://git-scm.com/docs/githooks#_pre_merge_commit)
    
    `pre-merge-commit` fires after a merge succeeds but before the merge commit is
    created.  This hook runs on all staged files from the merge.
    
    Note that you need to be using at least git 2.24 for this hook.

### pre-push

=== "中文"


    [git pre-push 文档](https://git-scm.com/docs/githooks#_pre_push) 
    
    `pre-push` 在执行 `git push` 时被触发。
    
    环境变量：
    - `PRE_COMMIT_FROM_REF`: 正在被推送到的修订版。
    - `PRE_COMMIT_TO_REF`: 正在被推送到远程的本地修订版。
    - `PRE_COMMIT_REMOTE_NAME`: 正在推送到的远程名称（例如 `origin`）。
    - `PRE_COMMIT_REMOTE_URL`: 正在推送到的远程的 URL（例如 `git@github.com:pre-commit/pre-commit`）。
    - `PRE_COMMIT_REMOTE_BRANCH`: 我们正在推送到的远程分支的名称（例如 `refs/heads/target-branch`）。
    - `PRE_COMMIT_LOCAL_BRANCH`: 正在推送到远程的本地分支的名称（例如 `HEAD`）。

=== "英文"

    [git pre-push docs](https://git-scm.com/docs/githooks#_pre_push)
    
    `pre-push` is triggered on `git push`.
    
    environment variables:
    - `PRE_COMMIT_FROM_REF`: the revision that is being pushed to.
    - `PRE_COMMIT_TO_REF`: the local revision that is being pushed to the remote.
    - `PRE_COMMIT_REMOTE_NAME`: which remote is being pushed to (for example `origin`)
    - `PRE_COMMIT_REMOTE_URL`: the url of the remote that is being pushed to (for
      example `git@github.com:pre-commit/pre-commit`)
    - `PRE_COMMIT_REMOTE_BRANCH`: the name of the remote branch to which we are
       pushing (for example `refs/heads/target-branch`)
    - `PRE_COMMIT_LOCAL_BRANCH`: the name of the local branch that is being pushed
      to the remote (for example `HEAD`)

### pre-rebase

=== "中文"

    在版本 3.2.0 中引入的新特性：
    
    [git pre-rebase 文档](https://git-scm.com/docs/githooks#_pre_rebase) 
    
    `pre-rebase` 钩子在执行变基操作之前被触发。如果钩子执行失败，可以取消变基操作。
    
    `pre-rebase` 钩子不操作文件，因此它们必须设置为 `always_run: true`，否则它们将总是被跳过。
    
    环境变量：
    - `PRE_COMMIT_PRE_REBASE_UPSTREAM`：`pre-rebase` Git 钩子的第一个参数
    - `PRE_COMMIT_PRE_REBASE_BRANCH`：`pre-rebase` Git 钩子的第二个参数。
    

=== "英文"

    _new in 3.2.0_
    
    [git pre-rebase docs](https://git-scm.com/docs/githooks#_pre_rebase)
    
    `pre-rebase` is triggered before a rebase occurs.  A hook failure can cancel a
    rebase from occurring.
    
    `pre-rebase` hooks do not operate on files so they must be set as
    `always_run: true` or they will always be skipped.
    
    environment variables:
    - `PRE_COMMIT_PRE_REBASE_UPSTREAM`: the first argument to the `pre-rebase` git hook
    - `PRE_COMMIT_PRE_REBASE_BRANCH`: the second argument to the `pre-rebase` git hook.

### prepare-commit-msg

=== "中文"
    
    `prepare-commit-msg` 钩子在提交信息编辑器启动之前被调用，它将接收一个文件名参数——这个文件可能是空的，或者它可能包含来自 `-m` 参数或其他模板的提交信息。`prepare-commit-msg` 钩子可以修改这个文件的内容，从而改变将要提交的内容。钩子可能需要检查 `GIT_EDITOR=:`，因为这表示不会启动编辑器。如果钩子以非零状态退出，则会中止提交。

    环境变量：

    - `PRE_COMMIT_COMMIT_MSG_SOURCE`：`prepare-commit-msg` Git 钩子的第二个参数
    - `PRE_COMMIT_COMMIT_OBJECT_NAME`：`prepare-commit-msg` Git 钩子的第三个参数
    
=== "英文"

    [git prepare-commit-msg docs](https://git-scm.com/docs/githooks#_prepare_commit_msg)
    
    `prepare-commit-msg` hooks will be passed a single filename -- this file may
    be empty or it could contain the commit message from `-m` or from other
    templates.  `prepare-commit-msg` hooks can modify the contents of this file to
    change what will be committed.  A hook may want to check for `GIT_EDITOR=:` as
    this indicates that no editor will be launched.  If a hook exits nonzero, the
    commit will be aborted.
    
    environment variables:
    - `PRE_COMMIT_COMMIT_MSG_SOURCE`: the second argument to the
      `prepare-commit-msg` git hook
    - `PRE_COMMIT_COMMIT_OBJECT_NAME`: the third argument to the
      `prepare-commit-msg` git hook

## 给钩子传递参数

**Passing arguments to hooks**

=== "中文"

    在某些情况下，钩子（hooks）需要参数才能正确运行。你可以通过在 `.pre-commit-config.yaml` 文件中的 [`args`](./plugins.md#config-args) 属性里指定参数来传递静态参数，如下所示：
    
    ```yaml
    -   repo: https://github.com/PyCQA/flake8 
        rev: 4.0.1
        hooks:
        -   id: flake8
            args: [--max-line-length=131]
    ```
    
    这将会把 `--max-line-length=131` 参数传递给 `flake8` 工具。


=== "英文"

    Sometimes hooks require arguments to run correctly. You can pass static
    arguments by specifying the [`args`](./plugins.md#config-args) property in your `.pre-commit-config.yaml`
    as follows:
    
    ```yaml
    -   repo: https://github.com/PyCQA/flake8
        rev: 4.0.1
        hooks:
        -   id: flake8
            args: [--max-line-length=131]
    ```
    
    This will pass `--max-line-length=131` to `flake8`.

### 钩子中的参数模式

**Arguments pattern in hooks**

=== "中文"

    如果您正在编写自己的自定义钩子，则您的钩子应该预期接收 [`args`](./plugins.md#config-args) 参数值，然后是一组已暂存的文件列表。

    例如，假设有一个 `.pre-commit-config.yaml` 文件：
    
    ```yaml
    -   repo: https://github.com/path/to/your/hook/repo 
        rev: badf00ddeadbeef
        hooks:
        -   id: my-hook-script-id
            args: [--myarg1=1, --myarg1=2]
    ```
    
    当您下次运行 `pre-commit` 时，将调用您的脚本：
    
    ```
    path/to/script-or-system-exe --myarg1=1 --myarg1=2 dir/file1 dir/file2 file3
    ```
    
    如果 [`args`](./plugins.md#config-args) 属性为空或未定义，将调用您的脚本：
    
    ```
    path/to/script-or-system-exe dir/file1 dir/file2 file3
    ```
    
    在创建本地钩子时，没有必要将命令参数放入 [`args`](./plugins.md#config-args) 中，因为没有什么东西可以覆盖它们——相反，直接把参数直接放入钩子的 [`entry`](./new-hooks.md#hooks-entry) 中。
    
    例如：
    
    ```yaml
    -   repo: local
        hooks:
        -   id: check-requirements
            name: 检查需求文件
            language: system
            entry: python -m scripts.check_requirements --compare
            files: ^requirements.*\.txt$
    ```
    

=== "英文"

    If you are writing your own custom hook, your hook should expect to receive
    the [`args`](./plugins.md#config-args) value and then a list of staged files.
    
    For example, assuming a `.pre-commit-config.yaml`:
    
    ```yaml
    -   repo: https://github.com/path/to/your/hook/repo
        rev: badf00ddeadbeef
        hooks:
        -   id: my-hook-script-id
            args: [--myarg1=1, --myarg1=2]
    ```
    
    When you next run `pre-commit`, your script will be called:
    
    ```
    path/to/script-or-system-exe --myarg1=1 --myarg1=2 dir/file1 dir/file2 file3
    ```
    
    If the [`args`](./plugins.md#config-args) property is empty or not defined, your script will be called:
    
    ```
    path/to/script-or-system-exe dir/file1 dir/file2 file3
    ```
    
    When creating local hooks, there's no reason to put command arguments
    into [`args`](./plugins.md#config-args) as there is nothing which can override them --
    instead put your arguments directly in the hook [`entry`](./new-hooks.md#hooks-entry).
    
    For example:
    
    ```yaml
    -   repo: local
        hooks:
        -   id: check-requirements
            name: check requirements files
            language: system
            entry: python -m scripts.check_requirements --compare
            files: ^requirements.*\.txt$
    ```

## 本地仓库钩子

**Repository local hooks**

=== "中文"

    仓库本地钩子在以下情况下很有用：

    - 当脚本与仓库紧密相关，并且将钩子脚本与仓库一起分发是有意义的。
    - 钩子需要的状态仅存在于仓库构建产物中（例如，用于 pylint 的应用虚拟环境）。
    - 某个 linter 的官方仓库没有 pre-commit 元数据。

    您可以通过指定 [`repo`](./plugins.md#repos-repo) 为哨兵值 `local` 来配置仓库本地钩子。

    本地钩子可以使用支持 [`additional_dependencies`](./plugins.md#config-additional_dependencies) 的任何语言，或者使用 [`docker_image`](./new-hooks.md#docker_image) / [`fail`](./new-hooks.md#fail) / [`pygrep`](./new-hooks.md#pygrep) / [`script`](./new-hooks.md#script) / [`system`](./new-hooks.md#system)。
    这使您能够安装以前需要一个简单的镜像仓库的东西。

    一个 `local` 钩子必须定义 [`id`](./new-hooks.md#hooks-id), [`name`](./new-hooks.md#hooks-name), [`language`](./new-hooks.md#hooks-language), [`entry`](./new-hooks.md#hooks-entry), 和 [`files`](./new-hooks.md#hooks-files) / [`types`](./new-hooks.md#hooks-types) 如在 [创建新钩子](./new-hooks.md) 下所指定。

    这里有一个带有一些 `local` 钩子的示例配置：

    ```yaml
    -   repo: local
        hooks:
        -   id: pylint
            name: pylint
            entry: pylint
            language: system
            types: [python]
            require_serial: true
        -   id: check-x
            name: 检查 X
            entry: ./bin/check-x.sh
            language: script
            files: \.x$
        -   id: scss-lint
            name: scss-lint
            entry: scss-lint
            language: ruby
            language_version: 2.1.5
            types: [scss]
            additional_dependencies: ['scss_lint:0.52.0']
    ```


=== "英文"

    Repository-local hooks are useful when:
    
    - The scripts are tightly coupled to the repository and it makes sense to
      distribute the hook scripts with the repository.
    - Hooks require state that is only present in a built artifact of your
      repository (such as your app's virtualenv for pylint).
    - The official repository for a linter doesn't have the pre-commit metadata.
    
    You can configure repository-local hooks by specifying the [`repo`](./plugins.md#repos-repo) as the
    sentinel `local`.
    
    local hooks can use any language which supports [`additional_dependencies`](./plugins.md#config-additional_dependencies)
    or [`docker_image`](./new-hooks.md#docker_image) / [`fail`](./new-hooks.md#fail) / [`pygrep`](./new-hooks.md#pygrep) / [`script`](./new-hooks.md#script) / [`system`](./new-hooks.md#system).
    This enables you to install things which previously would require a trivial
    mirror repository.
    
    A `local` hook must define [`id`](#hooks-id), [`name`](#hooks-name), [`language`](#hooks-language),
    [`entry`](./new-hooks.md#hooks-entry), and [`files`](#hooks-files) / [`types`](#hooks-types)
    as specified under [Creating new hooks](#new-hooks).
    
    Here's an example configuration with a few `local` hooks:
    
    ```yaml
    -   repo: local
        hooks:
        -   id: pylint
            name: pylint
            entry: pylint
            language: system
            types: [python]
            require_serial: true
        -   id: check-x
            name: Check X
            entry: ./bin/check-x.sh
            language: script
            files: \.x$
        -   id: scss-lint
            name: scss-lint
            entry: scss-lint
            language: ruby
            language_version: 2.1.5
            types: [scss]
            additional_dependencies: ['scss_lint:0.52.0']
    ```

## 内置钩子

**meta hooks**

=== "中文"

    `pre-commit` 提供了几个钩子，这些钩子对于检查 pre-commit 配置本身非常有用。这些钩子可以使用 `repo: meta` 来启用。

    ```yaml
    -   repo: meta
        hooks:
        -   id: ...
    ```
    
    当前可用的 `meta` 钩子：
    
    | 配置项                                                                                                 | 描述                                                   |
    | ------------------------------------------------------------------------------------------------------ | ------------------------------------------------------ |
    | [`check-hooks-apply`](#meta-check_hooks_apply)<span id="meta-check_hooks_apply"></span>                | 确保配置的钩子至少适用于仓库中的一个文件。             |
    | [`check-useless-excludes`](#meta-check_useless_excludes)<span id="meta-check_useless_excludes"></span> | 确保 `exclude` 指令适用于仓库中的任何一个文件。        |
    | [`identity`](#meta-identity)<span id="meta-identity"></span>                                           | 一个简单的钩子，它打印传给它的所有参数，对调试很有用。 |

=== "英文"

    `pre-commit` provides several hooks which are useful for checking the
    pre-commit configuration itself.  These can be enabled using `repo: meta`.
    
    ```yaml
    -   repo: meta
        hooks:
        -   id: ...
    ```
    
    The currently available `meta` hooks:
    
    ```table
    =r=
        =c= [`check-hooks-apply`](_#meta-check_hooks_apply)
        =c= ensures that the configured hooks apply to at least one file in the
            repository.
    =r=
        =c= [`check-useless-excludes`](_#meta-check_useless_excludes)
        =c= ensures that `exclude` directives apply to _any_ file in the
            repository.
    =r=
        =c= [`identity`](_#meta-identity)
        =c= a simple hook which prints all arguments passed to it, useful for
            debugging.
    ```

## 自动启用存储库的预提交

**automatically enabling pre-commit on repositories**

=== "中文"

    `pre-commit init-templatedir` 可用于设置 `git` 的 `init.templateDir` 选项的骨架。这意味着任何新克隆的仓库将自动设置钩子，无需运行 `pre-commit install`。
    
    要配置，请首先设置 `git` 的 `init.templateDir`——在这个例子中，我使用 `~/.git-template` 作为我的模板目录。
    
    ```console
    $ git config --global init.templateDir ~/.git-template
    $ pre-commit init-templatedir ~/.git-template
    pre-commit installed at /home/asottile/.git-template/hooks/pre-commit
    ```
    
    现在，每当您克隆一个启用了 pre-commit 的仓库时，钩子已经被设置好了！
    
    ```pre-commit
    $ git clone -q git@github.com:asottile/pyupgrade
    $ cd pyupgrade
    $ git commit --allow-empty -m 'Hello world!'
    Check docstring is first.............................(no files to check)Skipped
    Check Yaml...........................................(no files to check)Skipped
    Debug Statements (Python)............................(no files to check)Skipped
    ...
    ```
    
    `init-templatedir` 使用了 `pre-commit install` 的 `--allow-missing-config` 选项，因此没有配置的仓库将被跳过：
    
    ```console
    $ git init sample
    Initialized empty Git repository in /tmp/sample/.git/
    $ cd sample
    $ git commit --allow-empty -m 'Initial commit'
    `.pre-commit-config.yaml` config file not found. Skipping `pre-commit`.
    [main (root-commit) d1b39c1] Initial commit
    ```
    
    要仍然需要选择加入，但提示用户设置 pre-commit，使用如下的模板钩子（例如在 `~/.git-template/hooks/pre-commit` 中）。
    
    ```bash
    #!/usr/bin/env bash
    if [ -f .pre-commit-config.yaml ]; then
        echo 'pre-commit configuration detected, but `pre-commit install` was never run' 1>&2
        exit 1
    fi
    ```
    
    这样，如果忘记运行 `pre-commit install`，在提交时会产生错误：
    
    ```console
    $ git clone -q https://github.com/asottile/pyupgrade 
    $ cd pyupgrade/
    $ git commit -m 'foo'
    pre-commit configuration detected, but `pre-commit install` was never run
    ```

=== "英文"

    `pre-commit init-templatedir` can be used to set up a skeleton for `git`'s
    `init.templateDir` option.  This means that any newly cloned repository will
    automatically have the hooks set up without the need to run
    `pre-commit install`.
    
    To configure, first set `git`'s `init.templateDir` -- in this example I'm
    using `~/.git-template` as my template directory.
    
    ```console
    $ git config --global init.templateDir ~/.git-template
    $ pre-commit init-templatedir ~/.git-template
    pre-commit installed at /home/asottile/.git-template/hooks/pre-commit
    ```
    
    Now whenever you clone a pre-commit enabled repo, the hooks will already be
    set up!
    
    ```pre-commit
    $ git clone -q git@github.com:asottile/pyupgrade
    $ cd pyupgrade
    $ git commit --allow-empty -m 'Hello world!'
    Check docstring is first.............................(no files to check)Skipped
    Check Yaml...........................................(no files to check)Skipped
    Debug Statements (Python)............................(no files to check)Skipped
    ...
    ```
    
    `init-templatedir` uses the `--allow-missing-config` option from
    `pre-commit install` so repos without a config will be skipped:
    
    ```console
    $ git init sample
    Initialized empty Git repository in /tmp/sample/.git/
    $ cd sample
    $ git commit --allow-empty -m 'Initial commit'
    `.pre-commit-config.yaml` config file not found. Skipping `pre-commit`.
    [main (root-commit) d1b39c1] Initial commit
    ```
    
    To still require opt-in, but prompt the user to set up pre-commit use a
    template hook as follows (for example in `~/.git-template/hooks/pre-commit`).
    
    ```bash
    #!/usr/bin/env bash
    if [ -f .pre-commit-config.yaml ]; then
        echo 'pre-commit configuration detected, but `pre-commit install` was never run' 1>&2
        exit 1
    fi
    ```
    
    With this, a forgotten `pre-commit install` produces an error on commit:
    
    ```console
    $ git clone -q https://github.com/asottile/pyupgrade
    $ cd pyupgrade/
    $ git commit -m 'foo'
    pre-commit configuration detected, but `pre-commit install` was never run
    ```

## 根据文件类型过滤

**Filtering files with types**

=== "中文"

    使用 `types` 进行过滤相比传统的使用 `files` 过滤有多个优势：
    
    - 不需要容易出错的正则表达式。
    - 可以通过文件的 shebang（即使是没有扩展名的文件）进行匹配。
    - 可以轻松忽略符号链接/子模块。
    
    `types` 是按钩子指定为标签数组。这些标签是通过 [identify](https://github.com/pre-commit/identify) 库的一组启发式方法发现的。选择 `identify` 是因为它是一个小型的、可移植的、纯 Python 库。
    
    以下是一些 `identify` 常见的标签：
    
    - `file`
    - `symlink`
    - `directory` - 在 pre-commit 的上下文中，这将是子模块
    - `executable` - 文件是否设置了可执行位
    - `text` - 文件是否看起来像文本文件
    - `binary` - 文件是否看起来像二进制文件
    - [按扩展名/命名约定的标签](https://github.com/pre-commit/identify/blob/main/identify/extensions.py)
    - [按 shebang (`#!`) 的标签](https://github.com/pre-commit/identify/blob/main/identify/interpreters.py)
    
    要发现任何文件在磁盘上的类型，您可以使用 `identify` 的命令行界面：
    
    ```console
    $ identify-cli setup.py
    ["file", "non-executable", "python", "text"]
    $ identify-cli some-random-file
    ["file", "non-executable", "text"]
    $ identify-cli --filename-only some-random-file; echo $?
    1
    ```
    
    如果您使用的文件扩展名不受支持，请[提交一个拉取请求](https://github.com/pre-commit/identify)！
    
    `types`、`types_or` 和 `files` 在过滤时会与 `AND` 一起评估。`types` 内的标签也使用 `AND` 评估。
    
    _2.9.0 版本新功能_：`types_or` 内的标签使用 `OR` 评估。
    
    例如：
    
    ```yaml
        files: ^foo/
        types: [file, python]
    ```
    
    将匹配文件 `foo/1.py`，但不会匹配 `setup.py`。
    
    另一个例子：
    
    ```yaml
        files: ^foo/
        types_or: [javascript, jsx, ts, tsx]
    ```
    
    将匹配 `foo/bar.js` / `foo/bar.jsx` / `foo/bar.ts` / `foo/bar.tsx` 中的任何一个，但不会匹配 `baz.js`。
    
    如果您想匹配在使用现有钩子时未包含在 `type` 中的文件路径，则需要回退到仅使用 `files` 匹配，通过覆盖 `types` 设置。以下是使用 `check-json` 对非 JSON 文件进行匹配的示例：
    
    ```yaml
        -   id: check-json
            types: [file]  # 覆盖 `types: [json]`
            files: \.(json|myext)$
    ```
    
    文件也可以通过 shebang 匹配。使用 `types: python` 时，以 `#!/usr/bin/env python3` 开始的 `exe` 也将被匹配。
    
    与 `files` 和 `exclude` 一样，如果需要，也可以使用 `exclude_types` 排除类型。
    
=== "英文"

    Filtering with `types` provides several advantages over traditional filtering
    with `files`.
    
    - no error-prone regular expressions
    - files can be matched by their shebang (even when extensionless)
    - symlinks / submodules can be easily ignored
    
    `types` is specified per hook as an array of tags.  The tags are discovered
    through a set of heuristics by the
    [identify](https://github.com/pre-commit/identify) library.  `identify` was
    chosen as it is a small portable pure python library.
    
    Some of the common tags you'll find from identify:
    
    - `file`
    - `symlink`
    - `directory` - in the context of pre-commit this will be a submodule
    - `executable` - whether the file has the executable bit set
    - `text` - whether the file looks like a text file
    - `binary` - whether the file looks like a binary file
    - [tags by extension / naming convention](https://github.com/pre-commit/identify/blob/main/identify/extensions.py)
    - [tags by shebang (`#!`)](https://github.com/pre-commit/identify/blob/main/identify/interpreters.py)
    
    To discover the type of any file on disk, you can use `identify`'s cli:
    
    ```console
    $ identify-cli setup.py
    ["file", "non-executable", "python", "text"]
    $ identify-cli some-random-file
    ["file", "non-executable", "text"]
    $ identify-cli --filename-only some-random-file; echo $?
    1
    ```
    
    If a file extension you use is not supported, please
    [submit a pull request](https://github.com/pre-commit/identify)!
    
    `types`, `types_or`, and `files` are evaluated together with `AND` when
    filtering.  Tags within `types` are also evaluated using `AND`.
    
    _new in 2.9.0_: Tags within `types_or` are evaluated using `OR`.
    
    For example:
    
    ```yaml
        files: ^foo/
        types: [file, python]
    ```
    
    will match a file `foo/1.py` but will not match `setup.py`.
    
    Another example:
    
    ```yaml
        files: ^foo/
        types_or: [javascript, jsx, ts, tsx]
    ```
    
    will match any of `foo/bar.js` / `foo/bar.jsx` / `foo/bar.ts` / `foo/bar.tsx`
    but not `baz.js`.
    
    If you want to match a file path that isn't included in a `type` when using an
    existing hook you'll need to revert back to `files` only matching by overriding
    the `types` setting.  Here's an example of using `check-json` against non-json
    files:
    
    ```yaml
        -   id: check-json
            types: [file]  # override `types: [json]`
            files: \.(json|myext)$
    ```
    
    Files can also be matched by shebang.  With `types: python`, an `exe` starting
    with `#!/usr/bin/env python3` will also be matched.
    
    As with `files` and `exclude`, you can also exclude types if necessary using
    `exclude_types`.

## 正则表达式

**Regular expressions**

=== "中文"

    模式 `files` 和 `exclude` 使用的是 Python 的
    [正则表达式](https://docs.python.org/3/library/re.html#regular-expression-syntax)，
    并通过 [`re.search`](https://docs.python.org/3/library/re.html#re.search) 进行匹配。
    
    因此，您可以使用 Python 正则表达式支持的任何特性。
    
    如果您发现由于排除/包含的项过多，正则表达式变得复杂，可以考虑使用
    [详细模式](https://docs.python.org/3/library/re.html#re.VERBOSE)。可以通过 YAML 的多行字面量和
    `(?x)` 正则标志来启用此模式。
    
    ```yaml
    # ...
        -   id: my-hook
            exclude: |
                (?x)^(
                    path/to/file1.py|
                    path/to/file2.py|
                    path/to/file3.py
                )$
    ```

=== "英文"

    The patterns for `files` and `exclude` are python
    [regular expressions](https://docs.python.org/3/library/re.html#regular-expression-syntax)
    and are matched with [`re.search`](https://docs.python.org/3/library/re.html#re.search).
    
    As such, you can use any of the features that python regexes support.
    
    If you find that your regular expression is becoming unwieldy due to a long
    list of excluded / included things, you may find a
    [verbose](https://docs.python.org/3/library/re.html#re.VERBOSE) regular
    expression useful.  One can enable this with yaml's multiline literals and
    the `(?x)` regex flag.
    
    ```yaml
    # ...
        -   id: my-hook
            exclude: |
                (?x)^(
                    path/to/file1.py|
                    path/to/file2.py|
                    path/to/file3.py
                )$
    ```
    
## 覆盖语言版本

**Overriding language version**

=== "中文"

    有时您只想在特定版本的语言上运行钩子。对于每种语言，它们默认使用系统安装的语言（例如，如果我正在运行 `python3.7` 且钩子指定 `python`，则 pre-commit 会使用 `python3.7` 运行钩子）。有时您可能不想使用默认的系统安装版本，可以通过设置 [`language_version`](./plugins.md#config-language_version) 逐钩子覆盖此设置。
    
    ```yaml
    -   repo: https://github.com/pre-commit/mirrors-scss-lint
        rev: v0.54.0
        hooks:
        -   id: scss-lint
            language_version: 2.1.5
    ```
    
    这告诉 pre-commit 使用 Ruby `2.1.5` 来运行 `scss-lint` 钩子。
    
    以下是特定语言的有效值：

    - python: 使用您系统中安装的任何 Python 解释器。该参数的值作为 `-p` 传递给 `virtualenv`。
        - 在 Windows 上，
          [pep394](https://www.python.org/dev/peps/pep-0394/) 名称将被转换为 py 启动器调用以实现可移植性。因此，即使在 Windows 上，也应继续使用 `python3` (`py -3`) 或 `python3.6` (`py -3.6`)。
    - node: 参见 [nodeenv](https://github.com/ekalinin/nodeenv#advanced)。
    - ruby: 参见 [ruby-build](https://github.com/sstephenson/ruby-build/tree/master/share/ruby-build)。
    - _在 2.21.0 中新增_ rust: `language_version` 被传递给 `rustup`。
    - _在 3.0.0 中新增_ golang: 使用 [go.dev/dl](https://go.dev/dl/) 上的版本，例如 `1.19.5`。
    
    您可以在配置的 [顶层](./plugins.md#pre-commit-configyaml---顶级配置) 设置 [`default_language_version`](./plugins.md#top_level-default_language_version) 来控制所有钩子的语言默认版本。
    
    ```yaml
    default_language_version:
        # 强制所有未指定的 Python 钩子运行 python3
        python: python3
        # 强制所有未指定的 Ruby 钩子运行 Ruby 2.1.5
        ruby: 2.1.5
    ```

=== "英文"
    
    Sometimes you only want to run the hooks on a specific version of the
    language. For each language, they default to using the system installed
    language (So for example if I’m running `python3.7` and a hook specifies
    `python`, pre-commit will run the hook using `python3.7`). Sometimes you
    don’t want the default system installed version so you can override this on a
    per-hook basis by setting the [`language_version`](#config-language_version).
    
    ```yaml
    -   repo: https://github.com/pre-commit/mirrors-scss-lint
        rev: v0.54.0
        hooks:
        -   id: scss-lint
            language_version: 2.1.5
    ```
   
    This tells pre-commit to use ruby `2.1.5` to run the `scss-lint` hook.
    
    Valid values for specific languages are listed below:
    - python: Whatever system installed python interpreters you have. The value of
      this argument is passed as the `-p` to `virtualenv`.
        - on windows the
          [pep394](https://www.python.org/dev/peps/pep-0394/) name will be
          translated into a py launcher call for portability.  So continue to use
          names like `python3` (`py -3`) or `python3.6` (`py -3.6`) even on
          windows.
    - node: See [nodeenv](https://github.com/ekalinin/nodeenv#advanced).
    - ruby: See [ruby-build](https://github.com/sstephenson/ruby-build/tree/master/share/ruby-build).
    - _new in 2.21.0_ rust: `language_version` is passed to `rustup`
    - _new in 3.0.0_ golang: use the versions on [go.dev/dl](https://go.dev/dl/) such as `1.19.5`
    
    you can set [`default_language_version`](#top_level-default_language_version)
    at the [top level](#pre-commit-configyaml---top-level) in your configuration to
    control the default versions across all hooks of a language.
    
    ```yaml
    default_language_version:
        # force all unspecified python hooks to run python3
        python: python3
        # force all unspecified ruby hooks to run ruby 2.1.5
        ruby: 2.1.5
    ```

## 标记你的存储库

**badging your repository**

=== "中文"

    您可以在您的仓库中添加一个徽章，向贡献者/用户展示您使用 pre-commit！
    
    [![pre-commit](https://img.shields.io/badge/pre--commit-enabled-brightgreen?logo=pre-commit)](https://github.com/pre-commit/pre-commit)
    
    - Markdown:
    
      ```md#copyable
      [![pre-commit](https://img.shields.io/badge/pre--commit-enabled-brightgreen?logo=pre-commit)](https://github.com/pre-commit/pre-commit)
      ```
    
    - HTML:
    
      ```html#copyable
      <a href="https://github.com/pre-commit/pre-commit"><img src="https://img.shields.io/badge/pre--commit-enabled-brightgreen?logo=pre-commit" alt="pre-commit" style="max-width:100%;"></a>
      ```
    
    - reStructuredText:
    
      ```rst#copyable
      .. image:: https://img.shields.io/badge/pre--commit-enabled-brightgreen?logo=pre-commit
         :target: https://github.com/pre-commit/pre-commit
         :alt: pre-commit
      ```
    
    - AsciiDoc:
    
      ```#copyable
      image:https://img.shields.io/badge/pre--commit-enabled-brightgreen?logo=pre-commit[pre-commit, link=https://github.com/pre-commit/pre-commit]
      ```
    
=== "英文"

    you can add a badge to your repository to show your contributors / users that
    you use pre-commit!
    
    [![pre-commit](https://img.shields.io/badge/pre--commit-enabled-brightgreen?logo=pre-commit)](https://github.com/pre-commit/pre-commit)
    
    - Markdown:
    
      ```md#copyable
      [![pre-commit](https://img.shields.io/badge/pre--commit-enabled-brightgreen?logo=pre-commit)](https://github.com/pre-commit/pre-commit)
      ```
    
    - HTML:
    
      ```html#copyable
      <a href="https://github.com/pre-commit/pre-commit"><img src="https://img.shields.io/badge/pre--commit-enabled-brightgreen?logo=pre-commit" alt="pre-commit" style="max-width:100%;"></a>
      ```
    
    - reStructuredText:
    
      ```rst#copyable
      .. image:: https://img.shields.io/badge/pre--commit-enabled-brightgreen?logo=pre-commit
         :target: https://github.com/pre-commit/pre-commit
         :alt: pre-commit
      ```
    
    - AsciiDoc:
    
      ```#copyable
      image:https://img.shields.io/badge/pre--commit-enabled-brightgreen?logo=pre-commit[pre-commit, link=https://github.com/pre-commit/pre-commit]
      ```

## 在持续集成中的使用

**Usage in continuous integration**

=== "中文"

    pre-commit 还可以作为持续集成的工具。例如，将 `pre-commit run --all-files` 添加为 CI 步骤可以确保所有内容保持最佳状态。要仅检查已更改的文件（这可能更快），可以使用类似于 `pre-commit run --from-ref origin/HEAD --to-ref HEAD` 的命令。

=== "英文"

    pre-commit can also be used as a tool for continuous integration.  For
    instance, adding `pre-commit run --all-files` as a CI step will ensure
    everything stays in tip-top shape.  To check only files which have changed,
    which may be faster, use something like
    `pre-commit run --from-ref origin/HEAD --to-ref HEAD`

## 管理 CI 缓存

**Managing CI Caches**

=== "中文"

    默认情况下，`pre-commit` 将其仓库存储在 `~/.cache/pre-commit` 中——这可以通过两种方式进行配置：
    
    - `PRE_COMMIT_HOME`：如果设置了该环境变量，pre-commit 将使用该位置。
    - `XDG_CACHE_HOME`：如果设置了该环境变量，pre-commit 将使用 `$XDG_CACHE_HOME/pre-commit`，遵循 [XDG 基础目录规范]。
    
    [XDG 基础目录规范]: https://specifications.freedesktop.org/basedir-spec/basedir-spec-latest.html

=== "英文"

    `pre-commit` by default places its repository store in `~/.cache/pre-commit`
    -- this can be configured in two ways:
    
    - `PRE_COMMIT_HOME`: if set, pre-commit will use that location instead.
    - `XDG_CACHE_HOME`: if set, pre-commit will use `$XDG_CACHE_HOME/pre-commit`
      following the [XDG Base Directory Specification].
    
    [XDG Base Directory Specification]: https://specifications.freedesktop.org/basedir-spec/basedir-spec-latest.html

### pre-commit.ci 样例

**pre-commit.ci example**

=== "中文"

    在 [pre-commit.ci] 中运行无需额外配置！
    
    pre-commit.ci 还有以下优点：
    
    - 它比其他免费的 CI 解决方案更快
    - 它会自动修复拉取请求
    - 它会定期自动更新您的配置
    
    [![pre-commit.ci 速度比较](https://raw.githubusercontent.com/pre-commit-ci-demo/demo/main/img/2020-12-15_noop.svg)](https://github.com/pre-commit-ci-demo/demo#results)
    
    [pre-commit.ci]: https://pre-commit.ci

=== "英文"

    no additional configuration is needed to run in [pre-commit.ci]!
    
    pre-commit.ci also has the following benefits:
    
    - it's faster than other free CI solutions
    - it will autofix pull requests
    - it will periodically autoupdate your configuration
    
    [![pre-commit.ci speed comparison](https://raw.githubusercontent.com/pre-commit-ci-demo/demo/main/img/2020-12-15_noop.svg)](https://github.com/pre-commit-ci-demo/demo#results)
    
    [pre-commit.ci]: https://pre-commit.ci

### appveyor 样例

**appveyor example**

=== "中文"

    ```yaml
    cache:
    - '%USERPROFILE%\.cache\pre-commit'
    ```

=== "英文"

    ```yaml
    cache:
    - '%USERPROFILE%\.cache\pre-commit'
    ```

### azure pipelines 样例

**azure pipelines example**

=== "中文"

    注意：Azure Pipelines 使用不可变缓存，因此 Python 版本和 `.pre-commit-config.yaml` 哈希必须包含在缓存键中。有关仓库模板，请参见 [asottile@job--pre-commit.yml]。
    
    [asottile@job--pre-commit.yml]: https://github.com/asottile/azure-pipeline-templates/blob/main/job--pre-commit.yml
    
    ```yaml
    jobs:
    - job: precommit
    
      # ...
    
      variables:
        PRE_COMMIT_HOME: $(Pipeline.Workspace)/pre-commit-cache
    
      steps:
    
      # ...
    
      - script: echo "##vso[task.setvariable variable=PY]$(python -VV)"
      - task: CacheBeta@0
        inputs:
          key: pre-commit | .pre-commit-config.yaml | "$(PY)"
          path: $(PRE_COMMIT_HOME)
    ```

=== "英文"

    note: azure pipelines uses immutable caches so the python version and
    `.pre-commit-config.yaml` hash must be included in the cache key.  for a
    repository template, see [asottile@job--pre-commit.yml].
    
    [asottile@job--pre-commit.yml]: https://github.com/asottile/azure-pipeline-templates/blob/main/job--pre-commit.yml
    
    ```yaml
    jobs:
    - job: precommit
    
      # ...
    
      variables:
        PRE_COMMIT_HOME: $(Pipeline.Workspace)/pre-commit-cache
    
      steps:
    
      # ...
    
      - script: echo "##vso[task.setvariable variable=PY]$(python -VV)"
      - task: CacheBeta@0
        inputs:
          key: pre-commit | .pre-commit-config.yaml | "$(PY)"
          path: $(PRE_COMMIT_HOME)
    ```

### circleci 样例

**circleci example**

=== "中文"

    像 [Azure Pipelines](#azure-pipelines-example) 一样，CircleCI 也使用不可变缓存：
    
    ```yaml
      steps:
      - run:
        command: |
          cp .pre-commit-config.yaml pre-commit-cache-key.txt
          python --version --version >> pre-commit-cache-key.txt
      - restore_cache:
        keys:
        - v1-pc-cache-{{ checksum "pre-commit-cache-key.txt" }}
    
      # ...
    
      - save_cache:
        key: v1-pc-cache-{{ checksum "pre-commit-cache-key.txt" }}
        paths:
          - ~/.cache/pre-commit
    ```
    
    （来源：[@chriselion]）
    
    [@chriselion]: https://github.com/Unity-Technologies/ml-agents/pull/3094/files#diff-1d37e48f9ceff6d8030570cd36286a61

=== "英文"

    like [azure pipelines](#azure-pipelines-example), circleci also uses immutable
    caches:
    
    ```yaml
      steps:
      - run:
        command: |
          cp .pre-commit-config.yaml pre-commit-cache-key.txt
          python --version --version >> pre-commit-cache-key.txt
      - restore_cache:
        keys:
        - v1-pc-cache-{{ checksum "pre-commit-cache-key.txt" }}
    
      # ...
    
      - save_cache:
        key: v1-pc-cache-{{ checksum "pre-commit-cache-key.txt" }}
        paths:
          - ~/.cache/pre-commit
    ```
    
    (source: [@chriselion])
    
    [@chriselion]: https://github.com/Unity-Technologies/ml-agents/pull/3094/files#diff-1d37e48f9ceff6d8030570cd36286a61

### github actions 样例

**github actions example**

=== "中文"

    **查看 [官方 pre-commit GitHub 动作]**
    
    [官方 pre-commit GitHub 动作]: https://github.com/pre-commit/action
    
    与 [Azure Pipelines](#azure-pipelines-example) 类似，GitHub Actions 也使用不可变缓存：
    
    ```yaml
        - name: set PY
          run: echo "PY=$(python -VV | sha256sum | cut -d' ' -f1)" >> $GITHUB_ENV
        - uses: actions/cache@v3
          with:
            path: ~/.cache/pre-commit
            key: pre-commit|${{ env.PY }}|${{ hashFiles('.pre-commit-config.yaml') }}
    ```

=== "英文"

    **see the [official pre-commit github action]**
    
    [official pre-commit github action]: https://github.com/pre-commit/action
    
    like [azure pipelines](#azure-pipelines-example), github actions also uses
    immutable caches:
    
    ```yaml
        - name: set PY
          run: echo "PY=$(python -VV | sha256sum | cut -d' ' -f1)" >> $GITHUB_ENV
        - uses: actions/cache@v3
          with:
            path: ~/.cache/pre-commit
            key: pre-commit|${{ env.PY }}|${{ hashFiles('.pre-commit-config.yaml') }}
    ```

### gitlab CI 样例

**gitlab CI example**

=== "中文"

    查看 [GitLab 缓存最佳实践](https://docs.gitlab.com/ee/ci/caching/#good-caching-practices) 以优化缓存范围。
    
    ```yaml
    my_job:
      variables:
        PRE_COMMIT_HOME: ${CI_PROJECT_DIR}/.cache/pre-commit
      cache:
        paths:
          - ${PRE_COMMIT_HOME}
    ```
    
    pre-commit 的缓存需要在不同构建之间保持在一个固定位置。当在 GitLab 上使用 k8s 运行器时，这并不是默认设置。如果遇到错误 `InvalidManifestError`，请在 `[[runner]]` 配置中将 `builds_dir` 设置为某个静态值，例如 `builds_dir = "/builds"`。

=== "英文"

    See the [Gitlab caching best practices](https://docs.gitlab.com/ee/ci/caching/#good-caching-practices) to fine tune the cache scope.
    
    ```yaml
    my_job:
      variables:
        PRE_COMMIT_HOME: ${CI_PROJECT_DIR}/.cache/pre-commit
      cache:
        paths:
          - ${PRE_COMMIT_HOME}
    ```
    
    pre-commit's cache requires to be served from a constant location between the different builds. This isn't the default when using k8s runners
    on GitLab. In case you face the error `InvalidManifestError`, set `builds_dir` to something static e.g `builds_dir = "/builds"` in your `[[runner]]` config

### travis-ci 样例

**travis-ci example**

=== "中文"
    
    ```yaml
    cache:
      directories:
      - $HOME/.cache/pre-commit
    ```

=== "英文"
    
    ```yaml
    cache:
      directories:
      - $HOME/.cache/pre-commit
    ```

## 与 tox 一起使用

**Usage with tox**

=== "中文"

    [tox](https://tox.readthedocs.io/) 对于配置测试/CI 工具（如 pre-commit）非常有用。`tox>=2` 的一个特点是它会清除环境变量，从而使测试更加可重复。在某些情况下，pre-commit 需要一些环境变量，因此必须允许它们通过。
    
    在通过 SSH 克隆仓库时（`repo: git@github.com:...`），`git` 需要 `SSH_AUTH_SOCK` 变量，否则会失败：
    
    ```pre-commit
    [INFO] 正在为 git@github.com:pre-commit/pre-commit-hooks 初始化环境。
    发生意外错误：CalledProcessError: command: ('/usr/bin/git', 'fetch', 'origin', '--tags')
    返回代码：128
    预期返回代码：0
    标准输出：（无）
    标准错误：
        git@github.com: 权限被拒绝（公钥）。
        fatal: 无法从远程仓库读取。
    
        请确保您拥有正确的访问权限
        并且仓库存在。
    
    请查看位于 /home/asottile/.cache/pre-commit/pre-commit.log 的日志。
    ```
    
    在您的 tox 测试环境中添加以下内容：
    
    ```ini
    [testenv]
    passenv = SSH_AUTH_SOCK
    ```
    
    同样，在通过 HTTP/HTTPS 克隆仓库时（`repo: https://github.com:...`），您可能在公司 HTTP(S) 代理服务器后面，此时 `git` 需要设置 `http_proxy`、`https_proxy` 和 `no_proxy` 变量，否则克隆可能会失败：
    
    ```ini
    [testenv]
    passenv = http_proxy https_proxy no_proxy
    ```

=== "英文"

    [tox](https://tox.readthedocs.io/) is useful for configuring test / CI tools
    such as pre-commit.  One feature of `tox>=2` is it will clear environment
    variables such that tests are more reproducible.  Under some conditions,
    pre-commit requires a few environment variables and so they must be
    allowed to be passed through.
    
    When cloning repos over ssh (`repo: git@github.com:...`), `git` requires the
    `SSH_AUTH_SOCK` variable and will otherwise fail:
    
    ```pre-commit
    [INFO] Initializing environment for git@github.com:pre-commit/pre-commit-hooks.
    An unexpected error has occurred: CalledProcessError: command: ('/usr/bin/git', 'fetch', 'origin', '--tags')
    return code: 128
    expected return code: 0
    stdout: (none)
    stderr:
        git@github.com: Permission denied (publickey).
        fatal: Could not read from remote repository.
    
        Please make sure you have the correct access rights
        and the repository exists.
    
    Check the log at /home/asottile/.cache/pre-commit/pre-commit.log
    ```
    
    Add the following to your tox testenv:
    
    ```ini
    [testenv]
    passenv = SSH_AUTH_SOCK
    ```
    
    Likewise, when cloning repos over http / https
    (`repo: https://github.com:...`), you might be working behind a corporate
    http(s) proxy server, in which case `git` requires the `http_proxy`,
    `https_proxy` and `no_proxy` variables to be set, or the clone may fail:
    
    ```ini
    [testenv]
    passenv = http_proxy https_proxy no_proxy
    ```

## 使用存储库的最新版本

**Using the latest version for a repository**

=== "中文"

    `pre-commit` 配置旨在提供可重复和快速的体验，因此故意不为钩子仓库提供“未锁定的最新版本”功能。
    
    相反，`pre-commit` 提供工具，使您能够轻松升级到最新版本，使用 [`pre-commit autoupdate`](./cli.md#pre-commit-autoupdate)。如果您需要钩子的绝对最新版本（而不是最新的标签版本），请将 `--bleeding-edge` 参数传递给 `autoupdate`。
    
    `pre-commit` 假设 [`rev`](./plugins.md#repos-rev) 的值是不可变的引用（如标签或 SHA），并将基于此进行缓存。不支持将分支名称（或 `HEAD`）用作 [`rev`](./plugins.md#repos-rev) 的值，这仅表示在钩子安装时该可变引用的状态（并且 *不会* 自动更新）。

=== "英文"

    `pre-commit` configuration aims to give a repeatable and fast experience and
    therefore intentionally doesn't provide facilities for "unpinned latest
    version" for hook repositories.
    
    Instead, `pre-commit` provides tools to make it easy to upgrade to the
    latest versions with [`pre-commit autoupdate`](#pre-commit-autoupdate).  If
    you need the absolute latest version of a hook (instead of the latest tagged
    version), pass the `--bleeding-edge` parameter to `autoupdate`.
    
    `pre-commit` assumes that the value of [`rev`](./plugins.md#repos-rev) is an immutable ref (such as a
    tag or SHA) and will cache based on that.  Using a branch name (or `HEAD`) for
    the value of [`rev`](./plugins.md#repos-rev) is not supported and will only represent the state of
    that mutable ref at the time of hook installation (and will *NOT* update
    automatically).
    