---
title: LaTeX 实现简单的条件编译
date: 2019-09-23 08:10:52
update: 2019-09-23 08:10:52
tags:
    - LaTeX
categories:
    - Tools
---

我想让 LaTeX 支持的条件编译的功能是：

根据我是否定义某个具体的宏变量，决定是否编译某一片段。

通过简单搜索之后，了解到一个简单的宏包 `ifthen` 即可实现此功能，现将实现方法记录如下。

<!-- more -->

TLDR:

1. 引入 `ifthen` 宏包。
2. 使用 `\ifthenelse{\isundefine{\themarco}}{〈then clause〉}{〈else clause〉` 命令
3. 通过 `xelatex "\def\themarco{1} "\input{filename.tex}"` 在编译时定义宏。

## 引入 `ifthen` 宏包

```tex
\usepackage{ifthen}
```

## 使用 `\ifthenelse{}{}{}`

根据文档，　`\ifthenelse{〈test〉}{〈then clause〉}{〈else clause〉`　的定义如下：

如果 test 为真，则包括 `<then clause>`，否则包括 `<else clause>`。

`<test>` boolean 表达式可以以 `\and`、`\or`、`\not` 连接，此处我只需要测试一个宏是否定义，则不需要。

文档中还写到，可以使用 `\isundefined{}` 来测试一个具体的宏是否未定义。虽然我更想找到一个宏可以判断一个变量是否被定义，但是，好吧，这个宏也能完成我需要的功能。

所以，完成的语句如下：

```tex
\ifthenelse{\isundefined{\themacro}}{
    <then clause>
}{
    <else clause>
}
```

如果　`\themacro` 未定以，则执行（包括） `<then clause>`，否则执行 `<else clause>`。

至此，在 `tex` 文档中需要做的工作已经完成。接下来我们需要知道的是如何使用命令行编译时定义某个具体的宏。幸好，这个问题也很好解决。

## 在使用编译命令时定义具体的宏

以 `xelatex` 为例，我们可以直接输入以下命令编译指定文件。

```tex
xelatex filename.tex
```

同时如果输入参数不是文件名，而是字符串的话，同样可以编译。即 `xelatex "string"` 同样可以通过编译并生成文件，默认的文件名是 `texput.pdf`。目前，我仍然不知道 `xelatex` 如何指定输出文件名。

于是，我们可以通过以下命令定义我们需要的宏。

```tex
xelatex "\def\thenmarco{1} \input{filename.tex}"
```

至此，我所需要的条件编译的功能得到满足。

## 参考文献

1. [ifthen 宏包文档](http://mirrors.ctan.org/macros/latex/base/ifthen.pdf)
2. [如何通过命令行定义 LaTeX 变量的 SO 回答](https://tex.stackexchange.com/a/1495)