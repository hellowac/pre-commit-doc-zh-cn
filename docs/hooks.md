# 支持的钩子

**Supported hooks**

## 精选钩子

**featured hooks**

=== "中文"

    这里有一些精选的仓库，提供 pre-commit 集成。

    这些仓库相当受欢迎，并且通常在大多数设置中运行良好！
    
    _此列表并不旨在详尽无遗_
    
    由 pre-commit 团队提供：

    - [pre-commit/pre-commit-hooks](https://github.com/pre-commit/pre-commit-hooks)：一小部分与语言无关的钩子，普遍有用！
    - [pre-commit/pygrep-hooks](https://github.com/pre-commit/pygrep-hooks)：一些快速的基于正则表达式的钩子，用于快速语法检查
    - [pre-commit/sync-pre-commit-deps](https://github.com/pre-commit/sync-pre-commit-deps)：基于其他已安装钩子同步 pre-commit 钩子依赖项
    - [pre-commit/mirrors-*](https://github.com/orgs/pre-commit/repositories?language=&q=%22mirrors-%22+archived%3AFalse&sort=)：一些流行工具的 pre-commit 镜像
    
    针对 Python 项目：

    - [asottile/pyupgrade](https://github.com/asottile/pyupgrade)：自动升级语言的新版本语法
    - [asottile/(others)](https://sourcegraph.com/search?q=context:global+file:%5E%5C.pre-commit-hooks%5C.yaml%24+repo:%5Egithub.com/asottile/)：pre-commit 创建者的其他几个仓库
    - [psf/black](https://github.com/psf/black)：不妥协的 Python 代码格式化工具
    - [hhatto/autopep8](https://github.com/hhatto/autopep8)：自动修复 PEP8 违规
    - [astral-sh/ruff-pre-commit](https://github.com/astral-sh/ruff-pre-commit)：Python 的 ruff linter 和格式化工具
    - [google/yapf](https://github.com/google/yapf)：高度可配置的 Python 格式化工具
    - [PyCQA/flake8](https://github.com/PyCQA/flake8)：Python 的 linting 框架
    - [PyCQA/isort](https://github.com/PyCQA/isort)：Python 的导入排序工具
    - [PyCQA/(others)](https://sourcegraph.com/search?q=context:global+file:%5E%5C.pre-commit-hooks%5C.yaml%24+repo:%5Egithub.com/PyCQA/)：其他一些 Python 代码质量工具
    - [adamchainz/django-upgrade](https://github.com/adamchainz/django-upgrade)：自动升级您的 Django 项目代码
    
    针对 Shell 脚本：

    - [shellcheck-py/shellcheck-py]: 对您的脚本运行 shellcheck
    - [openstack/bashate]: 对 Bash 程序进行代码风格检查

    针对 Web：

    - [biomejs/pre-commit]: 用 Rust 编写的快速格式化工具
    - [standard/standard](https://github.com/standard/standard): 代码检查和修复工具
    - [shssoichiro/oxipng]: 优化 PNG 文件

    针对配置文件：

    - [python-jsonschema/check-jsonschema]: 使用 jsonschema 检查常见配置
    - [rhysd/actionlint]: 检查 GitHub Actions 工作流文件
    - [google/yamlfmt]: YAML 文件格式化工具
    - [adrienverge/yamllint]: YAML 文件检查工具

    针对文本 / 文档 / 散文：

    - [crate-ci/typos](https://github.com/crate-ci/typos): 查找和修复常见的排版错误
    - [thlorenz/doctoc]: 在 Markdown 文件中生成目录
    - [amperser/proselint](https://github.com/amperser/proselint): 散文检查工具
    - [markdownlint/markdownlint]: Markdown 检查工具
    - [codespell-project/codespell](https://github.com/codespell-project/codespell): 检查代码中的常见拼写错误

    针对提交信息检查：

    - [jorisroovers/gitlint]
    - [commitizen-tools/commitizen]

    针对秘密扫描 / 安全：

    - [gitleaks/gitleaks]
    - [trufflesecurity/truffleHog]
    - [thoughtworks/talisman]

    针对其他编程语言：

    - [realm/SwiftLint]: 强制执行 Swift 风格和约定
    - [nicklockwood/SwiftFormat]: Swift 的格式化工具
    - [AleksaC/terraform-py]: 格式化和验证 Terraform 语法
    - [rubocop/rubocop]: Ruby 的静态分析和格式化
    - [bufbuild/buf]: Protocol Buffers 工具
    - [sqlfluff/sqlfluff]: SQL 的模块化检查和自动格式化工具
    - [aws-cloudformation/cfn-lint]: AWS CloudFormation 检查工具
    - [google/go-jsonnet]: jsonnet 的检查和格式化工具
    - [JohnnyMorganz/StyLua]: 有个性化的 Lua 代码格式化工具
    - [Koihik/LuaFormatter]: Lua 代码的格式化工具
    - [mrtazz/checkmake]: Makefile 语法检查工具


=== "英文"

    here are a few hand-picked repositories which provide pre-commit integrations.
    
    these are fairly popular and are generally known to work well in most setups!
    
    _this list is not intended to be exhaustive_
    
    provided by the pre-commit team:

    - [pre-commit/pre-commit-hooks]: a handful of language-agnostic hooks which
      are universally useful!
    - [pre-commit/pygrep-hooks]: a few quick regex-based hooks for a handful of
      quick syntax checks
    - [pre-commit/sync-pre-commit-deps]: sync pre-commit hook dependencies based
      on other installed hooks
    - [pre-commit/mirrors-*]: pre-commit mirrors of a handful of popular tools
    
    for python projects:

    - [asottile/pyupgrade]: automatically upgrade syntax for newer versions of the
      language
    - [asottile/(others)]: a few other repos by the pre-commit creator
    - [psf/black]: The uncompromising Python code formatter
    - [hhatto/autopep8]: automatically fixes PEP8 violations
    - [astral-sh/ruff-pre-commit]: the ruff linter and formatter for python
    - [google/yapf]: a highly configurable python formatter
    - [PyCQA/flake8]: a linter framework for python
    - [PyCQA/isort]: an import sorter for python
    - [PyCQA/(others)]: a few other python code quality tools
    - [adamchainz/django-upgrade]: automatically upgrade your Django project code

    for shell scripts:

    - [shellcheck-py/shellcheck-py]: runs shellcheck on your scripts
    - [openstack/bashate]: code style enforcement for bash programs
    
    for the web:

    - [biomejs/pre-commit]: a fast formatter / fixer written in rust
    - [standard/standard]: linter / fixer
    - [shssoichiro/oxipng]: optimize png files
    
    for configuration files:

    - [python-jsonschema/check-jsonschema]: check many common configurations with jsonschema
    - [rhysd/actionlint]: lint your GitHub Actions workflow files
    - [google/yamlfmt]: a formatter for yaml files
    - [adrienverge/yamllint]: a linter for YAML files
    
    for text / docs / prose:

    - [crate-ci/typos]: find and fix common typographical errors
    - [thlorenz/doctoc]: generate a table-of-contents in markdown files
    - [amperser/proselint]: A linter for prose.
    - [markdownlint/markdownlint]: a Markdown lint tool
    - [codespell-project/codespell]: check code for common misspellings
    
    for linting commit messages:

    - [jorisroovers/gitlint]
    - [commitizen-tools/commitizen]
    
    for secret scanning / security:

    - [gitleaks/gitleaks]
    - [trufflesecurity/truffleHog]
    - [thoughtworks/talisman]
    
    for other programming languages:

    - [realm/SwiftLint]: enforce Swift style and conventions
    - [nicklockwood/SwiftFormat]: a formatter for Swift
    - [AleksaC/terraform-py]: format and validate terraform syntax
    - [rubocop/rubocop]: static analysis and formatting for Ruby
    - [bufbuild/buf]: tooling for Protocol Buffers
    - [sqlfluff/sqlfluff]: a modular linter and auto formatter for SQL
    - [aws-cloudformation/cfn-lint]: aws CloudFormation linter
    - [google/go-jsonnet]: linter / formatter for jsonnet
    - [JohnnyMorganz/StyLua]: an opinionated Lua code formatter
    - [Koihik/LuaFormatter]: a formatter for Lua code
    - [mrtazz/checkmake]: linter for Makefile syntax

[pre-commit/pre-commit-hooks]: https://github.com/pre-commit/pre-commit-hooks
[pre-commit/pygrep-hooks]: https://github.com/pre-commit/pygrep-hooks
[pre-commit/sync-pre-commit-deps]: https://github.com/pre-commit/sync-pre-commit-deps
[pre-commit/mirrors-*]: https://github.com/orgs/pre-commit/repositories?language=&q=%22mirrors-%22+archived%3AFalse&sort=
[asottile/pyupgrade]: https://github.com/asottile/pyupgrade
[asottile/(others)]: https://sourcegraph.com/search?q=context:global+file:%5E%5C.pre-commit-hooks%5C.yaml%24+repo:%5Egithub.com/asottile/
[psf/black]: https://github.com/psf/black
[hhatto/autopep8]: https://github.com/hhatto/autopep8
[astral-sh/ruff-pre-commit]: https://github.com/astral-sh/ruff-pre-commit
[google/yapf]: https://github.com/google/yapf
[PyCQA/flake8]: https://github.com/PyCQA/flake8
[PyCQA/isort]: https://github.com/PyCQA/isort
[PyCQA/(others)]: https://sourcegraph.com/search?q=context:global+file:%5E%5C.pre-commit-hooks%5C.yaml%24+repo:%5Egithub.com/PyCQA/
[adamchainz/django-upgrade]: https://github.com/adamchainz/django-upgrade
[realm/SwiftLint]: https://github.com/realm/SwiftLint
[nicklockwood/SwiftFormat]: https://github.com/nicklockwood/SwiftFormat
[AleksaC/terraform-py]: https://github.com/AleksaC/terraform-py
[rubocop/rubocop]: https://github.com/rubocop/rubocop
[bufbuild/buf]: https://github.com/bufbuild/buf
[sqlfluff/sqlfluff]: https://github.com/sqlfluff/sqlfluff
[aws-cloudformation/cfn-lint]: https://github.com/aws-cloudformation/cfn-lint
[google/go-jsonnet]: https://github.com/google/go-jsonnet
[JohnnyMorganz/StyLua]: https://github.com/JohnnyMorganz/StyLua
[Koihik/LuaFormatter]: https://github.com/Koihik/LuaFormatter
[mrtazz/checkmake]: https://github.com/mrtazz/checkmake

[gitleaks/gitleaks]: https://github.com/gitleaks/gitleaks
[trufflesecurity/truffleHog]: https://github.com/trufflesecurity/truffleHog
[thoughtworks/talisman]: https://github.com/thoughtworks/talisman

[crate-ci/typos]: https://github.com/crate-ci/typos
[thlorenz/doctoc]: https://github.com/thlorenz/doctoc
[amperser/proselint]: https://github.com/amperser/proselint
[markdownlint/markdownlint]: https://github.com/markdownlint/markdownlint
[codespell-project/codespell]: https://github.com/codespell-project/codespell
[jorisroovers/gitlint]: https://github.com/jorisroovers/gitlint
[commitizen-tools/commitizen]: https://github.com/commitizen-tools/commitizen

[python-jsonschema/check-jsonschema]: https://github.com/python-jsonschema/check-jsonschema
[rhysd/actionlint]: https://github.com/rhysd/actionlint
[google/yamlfmt]: https://github.com/google/yamlfmt
[adrienverge/yamllint]: https://github.com/adrienverge/yamllint

[biomejs/pre-commit]: https://github.com/biomejs/pre-commit
[standard/standard]: https://github.com/standard/standard
[shssoichiro/oxipng]: https://github.com/shssoichiro/oxipng

[shellcheck-py/shellcheck-py]: https://github.com/shellcheck-py/shellcheck-py
[openstack/bashate]: https://github.com/openstack/bashate

## 寻找钩子

**finding hooks**

=== "中文"

建议使用您喜欢的搜索工具查找可在项目中使用的现有钩子。

例如，这里有一些您可能觉得有用的搜索，使用 [sourcegraph]：

- 在 Python 文件上运行的钩子：[`file:^\.pre-commit-hooks\.yaml$ "types: [python]"`](https://sourcegraph.com/search?q=context:global+file:^\.pre-commit-hooks\.yaml%24+%22types:+[python]%22)
- 在 Shell 文件上运行的钩子：[`file:^\.pre-commit-hooks\.yaml$ "types: [shell]"`](https://sourcegraph.com/search?q=context:global+file:^\.pre-commit-hooks\.yaml%24+"types:+[shell]")
- 流行项目中的 pre-commit 配置：[`file:^\.pre-commit-config\.yaml$`](https://sourcegraph.com/search?q=context:global+file:^\.pre-commit-hooks\.yaml%24+"types:+[shell]")

[sourcegraph]: https://sourcegraph.com/search

您也可以发现 [GitHub 的搜索] 可能有用，尽管其查询和排序功能相当有限，并且需要登录：

- 提供钩子的仓库：[`path:.pre-commit-hooks.yaml language:YAML`](https://github.com/search?q=path%3A.pre-commit-hooks.yaml+language%3AYAML&type=code&l=YAML)

[GitHub 的搜索]: https://github.com/search

=== "英文"

  it's recommended to use your favorite searching tool to find existing hooks to
  use in your project.
  
  for example, here's some searches you may find useful using [sourcegraph]:
  
  - hooks which run on python files: [`file:^\.pre-commit-hooks\.yaml$ "types: [python]"`](https://sourcegraph.com/search?q=context:global+file:^\.pre-commit-hooks\.yaml%24+%22types:+[python]%22)
  - hooks which run on shell files: [`file:^\.pre-commit-hooks\.yaml$ "types: [shell]"`](https://sourcegraph.com/search?q=context:global+file:^\.pre-commit-hooks\.yaml%24+"types:+[shell]")
  - pre-commit configurations in popular projects: [`file:^\.pre-commit-config\.yaml$`](https://sourcegraph.com/search?q=context:global+file:^\.pre-commit-hooks\.yaml%24+"types:+[shell]")
  
  [sourcegraph]: https://sourcegraph.com/search
  
  you may also find [github's search] useful as well, though its querying and
  sorting capabilities are quite limited plus it requires a login:
  
  - repositories providing hooks: [`path:.pre-commit-hooks.yaml language:YAML`](https://github.com/search?q=path%3A.pre-commit-hooks.yaml+language%3AYAML&type=code&l=YAML)
  
  [github's search]: https://github.com/search
  
  
## 添加到此页面

**adding to this page**

=== "中文"

    此页面的先前版本是一个钩子列表，维护所列工具的质量非常繁琐。
    
    **此页面并不旨在详尽无遗**
    
    您可以发送 [拉取请求] 来扩展此列表，但有一些要求您 *必须* 遵循，否则您的 PR 将被关闭而不作评论：
    
    - 工具必须已经相当受欢迎（>500 星）
    - 工具必须使用受管理的语言（不允许使用 `system` / `script` / `docker` 钩子）
    - 工具必须操作文件
    
    [拉取请求]: https://github.com/pre-commit/pre-commit.com/blob/main/sections/hooks.md

=== "英文"

    the previous iteration of this page was a laundry list of hooks and maintaining
    quality of the listed tools was cumbersome.
    
    **this page is not intended to be exhaustive**
    
    you may send [a pull request] to expand this list however there are a few
    requirements you *must* follow or your PR will be closed without comment:
    
    - the tool must already be fairly popular (>500 stars)
    - the tool must use a managed language (no `system` / `script` / `docker` hooks)
    - the tool must operate on files
    
    [a pull request]: https://github.com/pre-commit/pre-commit.com/blob/main/sections/hooks.md
