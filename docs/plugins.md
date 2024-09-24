# 添加 pre-commit 插件到你的项目

**Adding pre-commit plugins to your project**

=== "中文"

    安装了 pre-commit 之后，可以通过 `.pre-commit-config.yaml` 配置文件向您的项目添加 pre-commit 插件。
    
    在项目的根目录下添加一个名为 `.pre-commit-config.yaml` 的文件。pre-commit 配置文件描述了安装了哪些仓库和钩子。
    

=== "英文"

    Once you have pre-commit installed, adding pre-commit plugins to your project
    is done with the `.pre-commit-config.yaml` configuration file.
    
    Add a file called `.pre-commit-config.yaml` to the root of your project. The
    pre-commit config file describes what repositories and hooks are installed.

## .pre-commit-config.yaml - 顶级配置

**.pre-commit-config.yaml - top level**

=== "中文"

    以下是 `.pre-commit-config.yaml` 文件中的顶级配置选项的解释：
    
    | 配置项                                                                                                                       | 描述                                                                                                                                                                                                                                                                                                                         |
    | ---------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | [`repos`](#top_level-repos)<span id="top_level-repos"></span>                                                                | 一个包含[仓库映射](#pre-commit-configyaml---仓库配置)的列表。                                                                                                                                                                                                                                                                |
    | [`default_install_hook_types`](#top_level-default_install_hook_types)<span id="top_level-default_install_hook_types"></span> | （可选，默认为 `[pre-commit]`）在运行[`pre-commit install`](./cli.md#pre-commit-install-options)时，默认使用的`--hook-type`列表。<br>_2.18.0 版本新增_                                                                                                                                                                       |
    | [`default_language_version`](#top_level-default_language_version)<span id="top_level-default_language_version"></span>       | （可选，默认为 `{}`）一种语言到默认[`language_version`](#config-language_version)的映射，该版本应该用于该语言。这只会在没有设置[`language_version`](#config-language_version)的个别钩子中覆盖。<br>例如，要为`language: python`钩子使用`python3.7`：<br>```yaml<br>default_language_version:<br>    python: python3.7<br>``` |
    | [`default_stages`](#top_level-default_stages)<span id="top_level-default_stages"></span>                                     | （可选，默认为所有阶段）钩子的[`stages`](#config-stages)属性的全局默认值。这只会在没有设置[`stages`](#config-stages)的个别钩子中覆盖。<br>例如：<br>```yaml<br>default_stages: [pre-commit, pre-push]<br>```                                                                                                                 |
    | [`files`](#top_level-files)<span id="top_level-files"></span>                                                                | （可选，默认为 `''`）全局文件包含模式。                                                                                                                                                                                                                                                                                      |
    | [`exclude`](#top_level-exclude)<span id="top_level-exclude"></span>                                                          | （可选，默认为 `^$`）全局文件排除模式。                                                                                                                                                                                                                                                                                      |
    | [`fail_fast`](#top_level-fail_fast)<span id="top_level-fail_fast"></span>                                                    | （可选，默认为 `false`）设置为 `true` 以在首次失败后停止运行钩子。                                                                                                                                                                                                                                                           |
    | [`minimum_pre_commit_version`](#top_level-minimum_pre_commit_version)<span id="top_level-minimum_pre_commit_version"></span> | （可选，默认为 `'0'`）要求 pre-commit 的最低版本。                                                                                                                                                                                                                                                                           |
    
    一个示例顶级配置：
    
    ```yaml
    exclude: '^$'
    fail_fast: false
    repos:
    -   ...
    ```
    

=== "英文"

    ```table
    =r=
        =c= [`repos`](_#top_level-repos)
        =c= A list of [repository mappings](#pre-commit-configyaml---repos).
    =r=
        =c= [`default_install_hook_types`](_#top_level-default_install_hook_types)
        =c= (optional: default `[pre-commit]`) a list of `--hook-type`s which will
            be used by default when running
            [`pre-commit install`](#pre-commit-install).
    
            _new in 2.18.0_
    =r=
        =c= [`default_language_version`](_#top_level-default_language_version)
        =c= (optional: default `{}`) a mapping from language to the default
            [`language_version`](#config-language_version) that should be used for that language.  This will
            only override individual hooks that do not set [`language_version`](#config-language_version).
    
            For example to use `python3.7` for `language: python` hooks:
    
            ```yaml
            default_language_version:
                python: python3.7
            ```
    =r=
        =c= [`default_stages`](_#top_level-default_stages)
        =c= (optional: default (all stages)) a configuration-wide default for
            the [`stages`](#config-stages) property of hooks.  This will only override individual
            hooks that do not set [`stages`](#config-stages).
    
            For example:
    
            ```yaml
            default_stages: [pre-commit, pre-push]
            ```
    =r=
        =c= [`files`](_#top_level-files)
        =c= (optional: default `''`) global file include pattern.
    =r=
        =c= [`exclude`](_#top_level-exclude)
        =c= (optional: default `^$`) global file exclude pattern.
    =r=
        =c= [`fail_fast`](_#top_level-fail_fast)
        =c= (optional: default `false`) set to `true` to have pre-commit stop
            running hooks after the first failure.
    =r=
        =c= [`minimum_pre_commit_version`](_#top_level-minimum_pre_commit_version)
        =c= (optional: default `'0'`) require a minimum version of pre-commit.
    ```
    
    A sample top-level:
    
    ```yaml
    exclude: '^$'
    fail_fast: false
    repos:
    -   ...
    ```

## .pre-commit-config.yaml - 仓库配置

**.pre-commit-config.yaml - repos**

=== "中文"

    仓库映射告诉 pre-commit 从哪里获取钩子的代码。

    | 配置项                                                | 描述                                                                                                                   |
    | ----------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
    | [`repo`](#repos-repo)<span id="repos-repo"></span>    | 要 `git clone` 的仓库 URL，或者是特殊哨兵值：[`local`](./advanced.md#本地仓库钩子), [`meta`](./advanced.md#内置钩子)。 |
    | [`rev`](#repos-rev)<span id="repos-rev"></span>       | 克隆时使用的修订版本或标签。                                                                                           |
    | [`hooks`](#repos-hooks)<span id="repos-hooks"></span> | 一个包含[钩子映射](#pre-commit-configyaml---钩子配置)的列表。                                                          |
    
    一个示例仓库：
    
    ```yaml
    repos:
    -   repo: https://github.com/pre-commit/pre-commit-hooks 
        rev: v1.2.3
        hooks:
        -   ...
    ```
    

=== "英文"

    The repository mapping tells pre-commit where to get the code for the hook
    from.
    
    ```table
    =r=
        =c= [`repo`](_#repos-repo)
        =c= the repository url to `git clone` from
            or one of the special sentinel values:
            [`local`](#repository-local-hooks),
            [`meta`](#meta-hooks).
    =r=
        =c= [`rev`](_#repos-rev)
        =c= the revision or tag to clone at.
    =r=
        =c= [`hooks`](_#repos-hooks)
        =c= A list of [hook mappings](#pre-commit-configyaml---hooks).
    ```
    
    A sample repository:
    
    ```yaml
    repos:
    -   repo: https://github.com/pre-commit/pre-commit-hooks
        rev: v1.2.3
        hooks:
        -   ...
    ```

## .pre-commit-config.yaml - 钩子配置

**.pre-commit-config.yaml - hooks**

=== "中文"

    钩子映射配置了使用仓库中的哪个钩子，并允许自定义。所有可选的键将从仓库的配置中获得其默认值。

    | 配置项                                                                                                        | 描述                                                                                                               |
    | ------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ |
    | [`id`](#config-id)<span id="config-id"></span>                                                                | 要使用的仓库中的钩子。                                                                                             |
    | [`alias`](#config-alias)<span id="config-alias"></span>                                                       | （可选）允许在使用 `pre-commit run <hookid>` 时使用额外的 ID 引用钩子。                                            |
    | [`name`](#config-name)<span id="config-name"></span>                                                          | （可选）覆盖钩子的名称 - 在钩子执行期间显示。                                                                      |
    | [`language_version`](#config-language_version)<span id="config-language_version"></span>                      | （可选）覆盖钩子的语言版本。参见[覆盖语言版本](./advanced.md#覆盖语言版本)。                                       |
    | [`files`](#config-files)<span id="config-files"></span>                                                       | （可选）覆盖要运行的文件的默认模式。                                                                               |
    | [`exclude`](#config-exclude)<span id="config-exclude"></span>                                                 | （可选）文件排除模式。                                                                                             |
    | [`types`](#config-types)<span id="config-types"></span>                                                       | （可选）覆盖要运行的默认文件类型（AND）。参见[使用类型过滤文件](./advanced.md#根据文件类型过滤)。                  |
    | [`types_or`](#config-types_or)<span id="config-types_or"></span>                                              | （可选）覆盖要运行的默认文件类型（OR）。参见[使用类型过滤文件](./advanced.md#根据文件类型过滤)。_2.9.0 版本新增_。 |
    | [`exclude_types`](#config-exclude_types)<span id="config-exclude_types"></span>                               | （可选）要排除的文件类型。                                                                                         |
    | [`args`](#config-args)<span id="config-args"></span>                                                          | （可选）要传递给钩子的附加参数列表。                                                                               |
    | [`stages`](#config-stages)<span id="config-stages"></span>                                                    | （可选）选择要运行的 git 钩子。参见[限制钩子在特定阶段运行](./advanced.md#限制钩子在某些阶段运行)。                |
    | [`additional_dependencies`](#config-additional_dependencies)<span id="config-additional_dependencies"></span> | （可选）将在运行此钩子的环境里安装的依赖项列表。一个有用的应用是安装钩子如 `eslint` 的插件。                       |
    | [`always_run`](#config-always_run)<span id="config-always_run"></span>                                        | （可选）如果为 `true`，则即使没有匹配的文件，这个钩子也会运行。                                                    |
    | [`verbose`](#config-verbose)<span id="config-verbose"></span>                                                 | （可选）如果为 `true`，则即使钩子通过，也强制打印钩子的输出。                                                      |
    | [`log_file`](#config-log_file)<span id="config-log_file"></span>                                              | （可选）如果存在，当钩子失败或[verbose](#config-verbose)为 `true` 时，钩子的输出也将被写入文件。                   |
    
    一个完整的配置示例：
    
    ```yaml
    repos:
    -   repo: https://github.com/pre-commit/pre-commit-hooks 
        rev: v1.2.3
        hooks:
        -   id: trailing-whitespace
    ```
    
    这个配置表示下载 pre-commit-hooks 项目并运行它的 trailing-whitespace 钩子。
    

=== "英文"

    The hook mapping configures which hook from the repository is used and allows
    for customization.  All optional keys will receive their default from the
    repository's configuration.
    
    ```table
    =r=
        =c= [`id`](_#config-id)
        =c= which hook from the repository to use.
    =r=
        =c= [`alias`](_#config-alias)
        =c= (optional) allows the hook to be referenced using an additional id when
            using `pre-commit run <hookid>`.
    =r=
        =c= [`name`](_#config-name)
        =c= (optional) override the name of the hook - shown during hook execution.
    =r=
        =c= [`language_version`](_#config-language_version)
        =c= (optional) override the language version for the
            hook.  See [Overriding Language Version](#overriding-language-version).
    =r=
        =c= [`files`](_#config-files)
        =c= (optional) override the default pattern for files to run on.
    =r=
        =c= [`exclude`](_#config-exclude)
        =c= (optional) file exclude pattern.
    =r=
        =c= [`types`](_#config-types)
        =c= (optional) override the default file types to run on (AND).  See
            [Filtering files with types](#filtering-files-with-types).
    =r=
        =c= [`types_or`](_#config-types_or)
        =c= (optional) override the default file types to run on (OR).  See
            [Filtering files with types](#filtering-files-with-types).
            _new in 2.9.0_.
    =r=
        =c= [`exclude_types`](_#config-exclude_types)
        =c= (optional) file types to exclude.
    =r=
        =c= [`args`](_#config-args)
        =c= (optional) list of additional parameters to pass to the hook.
    =r=
        =c= [`stages`](_#config-stages)
        =c= (optional) selects which git hook(s) to run for.
            See [Confining hooks to run at certain stages](#confining-hooks-to-run-at-certain-stages).
    =r=
        =c= [`additional_dependencies`](_#config-additional_dependencies)
        =c= (optional) a list of dependencies that will be installed in the
            environment where this hook gets run.  One useful application is to
            install plugins for hooks such as `eslint`.
    =r=
        =c= [`always_run`](_#config-always_run)
        =c= (optional) if `true`, this hook will run even if there are no matching
            files.
    =r=
        =c= [`verbose`](_#config-verbose)
        =c= (optional) if `true`, forces the output of the hook to be printed even when
            the hook passes.
    =r=
        =c= [`log_file`](_#config-log_file)
        =c= (optional) if present, the hook output will additionally be written to
            a file when the hook fails or [verbose](#config-verbose) is `true`.
    ```
    
    One example of a complete configuration:
    
    ```yaml
    repos:
    -   repo: https://github.com/pre-commit/pre-commit-hooks
        rev: v1.2.3
        hooks:
        -   id: trailing-whitespace
    ```
    
    This configuration says to download the pre-commit-hooks project and run its
    trailing-whitespace hook.

## 自动更新钩子

**Updating hooks automatically**

=== "中文"

    您可以运行 [`pre-commit autoupdate`](./advanced.md#使用存储库的最新版本) 来自动将您的钩子更新到最新版本。默认情况下，这会将钩子更新到默认分支上的最新标签。

=== "英文"
    
    You can update your hooks to the latest version automatically by running
    [`pre-commit autoupdate`](./advanced.md#使用存储库的最新版本).  By default, this will
    bring the hooks to the latest tag on the default branch.
