# 简介

**Introduction**

=== "中文"

    Git 钩子脚本在提交代码审查之前识别简单问题非常有用。我们在每次提交时运行钩子，自动指出代码中的问题，例如缺少分号、尾随空格和调试语句。在代码审查之前指出这些问题，使得代码审查者可以专注于更改的架构，而不浪费时间处理琐碎的风格问题。

    随着我们创建更多库和项目，我们意识到在项目之间共享预提交钩子是痛苦的。我们从一个项目复制粘贴笨重的 Bash 脚本，并且必须手动更改钩子以适应不同的项目结构。
    
    我们相信，您应该始终使用最佳的行业标准代码检查工具。一些最好的代码检查工具是用您在项目中未使用或未安装的语言编写的。例如，scss-lint 是一个为 SCSS 编写的 Ruby 代码检查工具。如果您在 Node.js 中编写项目，您应该能够在不添加 Gemfile 或了解如何安装 scss-lint 的情况下使用 scss-lint 作为预提交钩子。
    
    我们构建了 pre-commit 来解决钩子问题。它是一个多语言的预提交钩子包管理器。您可以指定要使用的钩子列表，而 pre-commit 会管理在每次提交之前安装和执行任何用任何语言编写的钩子。pre-commit 设计时特别考虑到不需要根权限。如果您的开发人员没有安装 Node，但修改了 JavaScript 文件，pre-commit 会自动处理下载和构建 Node，以在没有根权限的情况下运行 eslint。

=== "英文"

    Git hook scripts are useful for identifying simple issues before submission to code review.  We run our hooks on every commit to automatically point out issues in code such as missing semicolons, trailing whitespace, and debug statements.  By pointing these issues out before code review, this allows a code reviewer to focus on the architecture of a change while not wasting time with trivial style nitpicks.
    
    As we created more libraries and projects we recognized that sharing our pre-commit hooks across projects is painful. We copied and pasted unwieldy bash scripts from project to project and had to manually change the hooks to work for different project structures.
    
    We believe that you should always use the best industry standard linters. Some of the best linters are written in languages that you do not use in your project or have installed on your machine. For example scss-lint is a linter for SCSS written in Ruby. If you’re writing a project in node you should be able to use scss-lint as a pre-commit hook without adding a Gemfile to your project or understanding how to get scss-lint installed.
    
    We built pre-commit to solve our hook issues. It is a multi-language package manager for pre-commit hooks. You specify a list of hooks you want and pre-commit manages the installation and execution of any hook written in any language before every commit. pre-commit is specifically designed to not require root access. If one of your developers doesn’t have node installed but modifies a JavaScript file, pre-commit automatically handles downloading and building node to run eslint without root.
