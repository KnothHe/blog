---
title: Markdown 语法支持测试
date: 2019-02-09
updated: 2019-07-18 16:47:33
mathjax: true
---
Markdown 语法支持的测试博文，有关 Markdown 的语法可参考 [Markdown Guide](https://www.markdownguide.org/basic-syntax)

<!-- more -->

## 列表

### 无序列表

- **粗体**
- *斜体*
- ***粗体和斜体***

### 有序列表

1. First item
2. Second item
3. Third item
4. Fourth item

## 引用

> Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

## 代码块及代码高亮

### bash

``` bash
hexo new "My New Post"
```

### C 语言

```c
#include <stdio.h>

int main(void) {
    printf("Hello world\n");
    return 0;
}
```

## 转义标记

比如　`LaTeX`

## 链接

### 有内容的链接

[Markdown Guide](https://www.markdownguide.org/basic-syntax)

### 无内容的链接(URLs 和　Email)

#### URL

<https://knothhe.github.io/blog>

#### Email

<guanglaihe@gmail.com>

## 图片

![This is an image](markdown-syntax-support-test/qiao.jpg)

## 数学公式

### 行内公式

质能方程：$E = mc^2$

### 行间公式

黎曼 $\zeta$ 函数

$$
    \zeta(s) = \sum_{n=1}^{\infty} \frac{1}{n^s}
$$

## 源文件

[地址](https://raw.githubusercontent.com/KnothHe/blog/master/source/_posts/markdown-syntax-support-test.md)

## GitHub Actions 测试

- 测试 GitHub Actions 是否成功。
- 测试手机编辑部署。
