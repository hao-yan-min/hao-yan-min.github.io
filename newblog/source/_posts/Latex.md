---
title: Latex的使用
date: 2023-05-01 23:12:45
tags:
- Latex
- 论文排版工具
categories:
- Latex
---





## Latex环境和命令

$latex{}$ 的命令以\开头，而环境是指用以令一些效果在局部生效，或是生成特殊的文档元素。LATEX 环境

的用法为一对命令 \begin 和 \end

```la
\begin{⟨environment name⟩}[⟨optional arguments⟩]{⟨mandatory arguments⟩}
…
\end{⟨environment name⟩}
```

其中 *⟨**environment name**⟩* 为环境名，\begin 和 \end 中填写的环境名应当一致。类似命令，

{*⟨**mandatory arguments**⟩*} 和 [*⟨**optional arguments**⟩*] 为环境所需的必选和可选参数。LATEX 环

境可能需要一个或多个必选/可选参数，也可能完全不需要参数。部分环境允许嵌套使用。

### Latex源代码结构

LATEX 源代码以一个 \documentclass 命令作为开头，它指定了文档使用的**文档类**。docu

ment 环境当中的内容是文档正文。

```la
\documentclass[⟨options⟩]{⟨class-name⟩}
```



在 \documentclass 和 \begin{document} 之间的位置称为**导言区**。在导言区中常会使用

\usepackage 命令调用**宏包**，还会进行文档的全局设置。

![image-20230424224310764](/images/image-20230424224310764.png)

### Latex排版文字

使用latex排版中文的时候需要引入ctex包，代码如下：

```lat
\usepackage{ctex}
```

## 空格和分段

latex中每行开头的空格无效，一个或多个空格被视为一个空格。可以使用\par命令让文字分段。文末的一个换行符被视为一个空格，但多个换行符被视为一个换行符（对中文无效）

### 注释

latex使用%进行注释，该字符到行末的所有文字都被忽略

### 字体的设置

Latex中设置正文字体的时候使用 \setmainfont命令。例如，如果要设置字体为宋体，命令为\setmainfont{Simsun}

在\documentclass[11pt,a4paper]{article}命令中可以设置正文字体大小和使用的纸张大小。

### 行间距的设置

Latex中通过引入宏包

```latex
\usepackage{setspace} % 加载 setspace 宏包
\setstretch{1.5} % 设置行距为1.5倍
```

通过这种方式设置行间距。



### 设置目录

通过引入包hyperref然后在文档内容中加入目录，具体代码如下：

```latex
\usepackage{hyperref} % 引入超链接包

\begin{document}

\newpage % 新页
\tableofcontents % 生成目录
```

这样生成的目录可以使用超链接且独占一页。



## 排版



### 段落

可以使用\section{}标签生成一级段落，\subsection{}生成二级段落。 \subsubsection{} 生成三级段落。

### 列表

使用

```latex
\begin{enumerate}
\item 表项一
\item 表项二
···
\end{enumerate}
```

生成有序列表，使用

```latex
\begin{itemize}
\item 表项一
\item 表项二
···
\end{itemize}
```

生成无序列表。如果要将标签数字用括号括起，使用

```latex
\usepackage{enumitem}
\begin{enumerate}[label=(\arabic*)]
    \item 第一项内容
    \item 第二项内容
    \item 第三项内容
\end{enumerate}
```

其中*表示使用括号



## 公式

如何需要添加一个公式段落，可以使用

```latex
\begin{equation}\label{eq:eq1}
f(x)=a^x+b
\end{equation}
```

其中label标签中定义了公式的引用key，在文章的其他部分可以使用``\ref{eq:eq1}``来进行引用。



常用的公式符号如下：

| 符号                     | 含义                       |
| ------------------------ | -------------------------- |
| =                        | 赋值                       |
| &                        | 对齐符号                   |
| +                        | 加法                       |
| -                        | 减法                       |
| *                        | 乘法                       |
| /                        | 除法                       |
| %                        | 取模                       |
| ^                        | 指数                       |
| ()                       | 括号，用于改变优先级       |
| {}                       | 大括号，用于组合数学表达式 |
| []                       | 中括号，用于表示向量或矩阵 |
| \_                       | 下划线，用于表示下标       |
| \^{}                     | 脚注，用于表示上标         |
| \sqrt{}                  | 开方                       |
| \frac{}{}                | 分数                       |
| \sum                     | 求和                       |
| \int                     | 积分                       |
| \lim                     | 极限                       |
| \infty                   | 无穷                       |
| \pm                      | 正负号                     |
| \neq                     | 不等于号                   |
| \leq                     | 小于等于号                 |
| \geq                     | 大于等于号                 |
| \mathop{\max}\limits_{x} | $\mathop{\max}\limits_{x}$ |

## 图片

如果要插入图片，首先要引入graphicx包，具体的代码如下：

```latex
\usepackage{graphicx}




\begin{figure}
\centering

\includegraphics[width=0.5\textwidth]{Figure1.png}
\caption{DeepId模型结构图} \label{fig:Figure1}
\end{figure}
```

caption为图片题注，label为引用标签，\centering为设置图片居中

$y^i_{j,k}=\mathop{\max}\limits_{0\le m,n<s}\{x^i_{j\cdot s+m,k\cdot s+n}\}$

$y_j=max\big(0,\sum\limits_ix^1_i\cdot w^1_{i,j}+\sum\limits_{i}x^2_i\cdot w^2_{i,j}+b_j\big)$

$y_i=\frac {exp(y_i)} {\sum^n_{i=1}exp(y_j)}$



## 文献的引用

在$Latex$中，通常可以使用BibTeX文件对文件进行引用。BibTex是一个单独的文献引用数据库，其基本的格式是：

```json
@article{smith01,
  author = "John Smith",
  title = "A new approach to computer vision",
  journal = "IEEE Transactions on Pattern Analysis and Machine Intelligence",
  volume = "23",
  number = "12",
  pages = "1331--1335",
  year = "2001"
}

```

这个例子中的文献条目包括作者、标题、期刊名称、卷、期、页码和年份等信息。

之后可以在LaTeX文档中使用\cite命令引用文献，例如：

```latex
According to Smith \cite{smith01}, computer vision is a challenging field.

```

之后在LaTeX文档中需要生成文献参考表的位置指定BibTeX文件的位置和样式，例如：

```latex
\bibliography{mybibfile}
\bibliographystyle{plain}
```

最后，先使用xelatex编译一遍源文件，然后使用BibTex编译一遍源文件，然后再用xelatex编译一遍源文件就可以生成正确的pdf文件了。
