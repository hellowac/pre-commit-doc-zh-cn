# 用法

**Usage**

=== "中文"

    运行 `pre-commit install` 将 pre-commit 安装到您的 git 钩子中。现在，每次提交时 pre-commit 都会运行。每次克隆使用 pre-commit 的项目时，运行 `pre-commit install` 应该是您做的第一件事。

    如果您想手动在仓库上运行所有 pre-commit 钩子，请运行 `pre-commit run --all-files`。要运行个别钩子，请使用 `pre-commit run <hook_id>`。
    
    首次在文件上运行 pre-commit 时，它将自动下载、安装并运行钩子。请注意，首次运行钩子可能会比较慢。例如：如果机器上没有安装 node，pre-commit 将会下载并构建 node 的副本。
    
    ```pre-commit
    $ pre-commit install
    pre-commit installed at /home/asottile/workspace/pytest/.git/hooks/pre-commit
    $ git commit -m "Add super awesome feature"
    black....................................................................Passed
    blacken-docs.........................................(no files to check)Skipped
    Trim Trailing Whitespace.................................................Passed
    Fix End of Files.........................................................Passed
    Check Yaml...........................................(no files to check)Skipped
    Debug Statements (Python)................................................Passed
    Flake8...................................................................Passed
    Reorder python imports...................................................Passed
    pyupgrade................................................................Passed
    rst ``code`` is two backticks........................(no files to check)Skipped
    rst..................................................(no files to check)Skipped
    changelog filenames..................................(no files to check)Skipped
    [main 146c6c2c] Add super awesome feature
     1 file changed, 1 insertion(+)
    ```
    

=== "英文"

    Run `pre-commit install` to install pre-commit into your git hooks. pre-commit
    will now run on every commit. Every time you clone a project using pre-commit
    running `pre-commit install` should always be the first thing you do.
    
    If you want to manually run all pre-commit hooks on a repository, run
    `pre-commit run --all-files`. To run individual hooks use
    `pre-commit run <hook_id>`.
    
    The first time pre-commit runs on a file it will automatically download,
    install, and run the hook. Note that running a hook for the first time may be
    slow. For example: If the machine does not have node installed, pre-commit
    will download and build a copy of node.
    
    ```pre-commit
    $ pre-commit install
    pre-commit installed at /home/asottile/workspace/pytest/.git/hooks/pre-commit
    $ git commit -m "Add super awesome feature"
    black....................................................................Passed
    blacken-docs.........................................(no files to check)Skipped
    Trim Trailing Whitespace.................................................Passed
    Fix End of Files.........................................................Passed
    Check Yaml...........................................(no files to check)Skipped
    Debug Statements (Python)................................................Passed
    Flake8...................................................................Passed
    Reorder python imports...................................................Passed
    pyupgrade................................................................Passed
    rst ``code`` is two backticks........................(no files to check)Skipped
    rst..................................................(no files to check)Skipped
    changelog filenames..................................(no files to check)Skipped
    [main 146c6c2c] Add super awesome feature
     1 file changed, 1 insertion(+)
    ```
