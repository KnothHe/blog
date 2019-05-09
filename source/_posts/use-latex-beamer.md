---
title: LaTeX Beamer 使用
date: 2019-04-13 20:25:25
update: 2019-04-13 20:25:25
tags:
    -  LaTeX
    -  Beamer
categories: 
    - Summary
---

今天用了 Beamer 做一个 Presentation，虽然之前已经尝试过，但是有一些细节的部分忘记了，又花了一点时间在查找各种细节。 Presentation 的初稿现在算是基本弄好了。于是决定把折腾得到的结果记录一下。

以下均是一个 `LaTeX` 门外汉的个人见解，也只是知其然而不知其所以然罢了。

<!-- more -->

## 元信息

可以添加一些作者，日期之类的信息。

```latex
\title{..}
\author{...}
\data{...}
...
```

## 标题页

```latex
\begin{frame}
    \titlepage
\end{frame}
```

## 生成 ToC

```latex
\begin{frame}{Table of Contents}
    \tableofcontents
\end{frame}
```

## frame 标题

在 `frame` 后加上 `{title}` 就行了。建议每个 `frame` 都加上一个标题。

```latex
\begin{frame}{Title of This Frame}
\end{frame}
```

## 中文显示

这个问题在我刚开始使用 `LaTeX` 是还是挺犯难的，用的时间较长了，加上这么多年来 `LaTeX` 的发展，虽然在我刚开始学习时， LaTeX 对非西文字体的支持就已经发展的相当的好了，解决方法已经变得相当的简单了。

最简单的解决方法就是在开头加上 `\usepackage{CJK}`。

并且对于中文等非西文字体的 `.tex` 源文件编译时，最好是采用 `XeLaTeX`， 毕竟原本 `XeTeX/XeLaTeX` 原本的设计目的就是增强对非西文字体的支持。

## Beamer 主题

我比较喜欢的主题时 `CambridgeUS`

```latex
\usetheme{CambridgeUS}
```

挑选 Beamer 内置主题的话，可以参考 [Beamer Theem Matrix](https://hartwork.org/beamer-theme-matrix/) 或者 [Another Beamer Theme Matrix](https://mpetroff.net/files/beamer-theme-matrix/) 

当然，在主题之上还可以修改 `colortheme` 之类的。我挺喜欢正在使用的主题的默认的 `colortheme` 的，也就没改。

## 分章节

和普通的 `LaTeX` 文档类似，可以为 `Beamer` 添加章节信息。章节信息最后会生成目录。

```latex
\section{...}
\subsection{...}
```


## 代码排版

在一个 `frame` 中排版代码也是可以的，借助 `listings` 宏包的解决方法如下：


下面这一部分可定义也可不定义，参考（复制于） [WikiBooks](https://en.wikibooks.org/wiki/LaTeX/Source_Code_Listings)
```latex
\usepackage{color}

\definecolor{mygreen}{rgb}{0,0.6,0}
\definecolor{mygray}{rgb}{0.5,0.5,0.5}
\definecolor{mymauve}{rgb}{0.58,0,0.82}

\lstset{ 
    backgroundcolor=\color{white},   % choose the background color; you must add \usepackage{color} or \usepackage{xcolor}; should come as last argument
    basicstyle=\ttfamily\footnotesize,        % the size of the fonts that are used for the code
    breakatwhitespace=true,         % sets if automatic breaks should only happen at whitespace
    breaklines=true,                 % sets automatic line breaking
    captionpos=b,                    % sets the caption-position to bottom
    commentstyle=\color{mygreen},    % comment style
    deletekeywords={...},            % if you want to delete keywords from the given language
    escapeinside={\%*}{*)},          % if you want to add LaTeX within your code
    extendedchars=true,              % lets you use non-ASCII characters; for 8-bits encodings only, does not work with UTF-8
    frame=single,                   % adds a frame around the code
    keepspaces=false,                 % keeps spaces in text, useful for keeping indentation of code (possibly needs columns=flexible)
    morekeywords={bit, sbit, sfr, sfr16},            % if you want to add more keywords to the set
    keywordstyle=\color{blue},       % keyword style
    language=C,                 % the language of the code
    numbers=left,                    % where to put the line-numbers; possible values are (none, left, right)
    numbersep=5pt,                   % how far the line-numbers are from the code
    numberstyle=\tiny\color{mygray}, % the style that is used for the line-numbers
    rulecolor=\color{black},         % if not set, the frame-color may be changed on line-breaks within not-black text (e.g. comments (green here))
    showspaces=false,                % show spaces everywhere adding particular underscores; it overrides 'showstringspaces'
    showstringspaces=false,          % underline spaces within strings only
    showtabs=false,                  % show tabs within strings adding particular underscores
    %  stepnumber=2,                    % the step between two line-numbers. If it's 1, each line will be numbered
    stringstyle=\color{mymauve},     % string literal style
    tabsize=2,                 % sets default tabsize to 2 spaces
    title=\lstname                   % show the filename of files included with \lstinputlisting; also try caption instead of title
}
```

下面这部分参考 [StackExchange](https://tex.stackexchange.com/questions/36776/latex-error-when-inserting-code-listing-in-lyx)。

就是在需要排版代码的 `frame` 后加上参数 `fragile`。

```latex
\begin{frame}[fragile]
    \begin{lstlisting}[language=...]
        ...
        CODE
        ...
    \end{lstlisting}
\end{frame}
```
