# LaTeX Reviewer Response Template

A reusable LaTeX template for academic response letters, with automatic reviewer/comment numbering and references listed beneath each response.

适用于论文返修回复信的 LaTeX 模板。包含修改概览、逐条审稿回复、修改正文摘录与通用致谢。作者、标题和单位使用通用英文词，方便替换。

[查看模板 PDF 预览](response_template.pdf)

## 功能

- 审稿意见使用蓝色斜体，作者回复使用黑色正文，修改正文摘录使用红色。
- 审稿人和意见自动编号；每位审稿人的意见编号从 1 重新开始。
- 默认包含 3 位审稿人，每位各有 2 组空白意见和回复，可按需增删。
- 在每条回复下自动显示该回复引用文献的完整条目。
- 各回复共用一套引用编号，同一条回复中的重复引用合并显示。
- 文末不显示参考文献总表。
- 提供 Summary of Revisions 图框；加入 `fig1.png` 后自动替换为实际图片。
- 保留通用开头、总体评价致谢、结尾和署名区域。

## 文件结构

```text
.
├── response_template.tex   # 主模板
├── ref.bib                 # 空白 BibTeX 文献库
├── response_template.pdf   # 通用模板预览
├── README.md
├── .gitignore
└── .gitattributes
```

## 开始使用

下载项目，或使用 GitHub 的 **Use this template** 创建自己的副本。

1. 打开 `response_template.tex`。
2. 填写论文标题、作者和署名信息。首页与致谢正文中的 `Title`、首页与结尾的 `Authors` 应同步更新。
3. 将每位审稿人的总体评价放入 `generalcomment` 环境。
4. 将具体意见与回复分别填写到 `reviewcomment` 和 `author_response` 环境。
5. 将引用的真实文献条目添加到 `ref.bib`，然后在回复中使用 `\cite{条目键}`。
6. 编译并检查生成的 PDF。

署名区域中的通用字段如下：

| 字段 | 填写内容 |
| --- | --- |
| `Title` | 论文标题 |
| `Authors` | 全部作者 |
| `Affiliation` | 学院、部门或研究机构 |
| `Institution` | 学校或单位 |
| `City` | 城市 |
| `Country` | 国家 |
| `Author` | 通讯作者 |
| `Email` | 联系邮箱 |

## 编译

使用包含所需宏包的 TeX Live 或 MiKTeX。主文件为 `response_template.tex`，使用 pdfLaTeX 编译；有文献引用时需要 BibTeX。

主要文献相关依赖为 `bibentry`、`etoolbox`、`xparse`、`letltxmacro` 与 `elsarticle-num.bst`。其他字体、颜色和版式宏包列于主文件开头。

### 自动编译

在项目文件夹中运行：

```sh
latexmk -pdf response_template.tex
```

### 手动编译

未引用文献时，运行两遍 pdfLaTeX 以更新审稿人交叉引用：

```sh
pdflatex response_template.tex
pdflatex response_template.tex
```

添加文献引用后，按以下顺序编译：

```sh
pdflatex response_template.tex
bibtex response_template
pdflatex response_template.tex
pdflatex response_template.tex
```

`ref.bib` 初始为空。加入引用前，先添加与引用键对应的真实 BibTeX 条目。

## 意见与回复

复制以下结构即可增加一条意见，编号会自动更新：

```latex
% Reviewer 1, Comment 1
\begin{reviewcomment}
The reviewer's comment.
\end{reviewcomment}
\begin{author_response}
Your response.
\end{author_response}
```

保留 `% Reviewer 1, Comment 1` 这类注释便于定位。若增删意见，可以同步调整注释中的数字；PDF 中的编号由计数器生成。

新增审稿人时，复制一个审稿人章节，调整章节编号与标签，例如：

```latex
\ReviewerSection{4}
\label{S4}
```

同时在首页的交叉引用句中加入 `\Cref{S4}`。

## 回复中的参考文献

在 `author_response` 环境内使用标准 `\cite` 命令：

```latex
\begin{author_response}
We have clarified the discussion of the related work~\cite{reference_key}.
\end{author_response}
```

将 `reference_key` 换成 `ref.bib` 中真实条目的键。也可一次引用多篇文献：

```latex
\cite{key1,key2}
```

每条回复结束时会自动生成 **References cited in this response:** 列表，无需手工重复写文献信息。没有引用的回复不显示该列表。此功能收集 `author_response` 内的标准 `\cite` 调用。

主文件末尾的 `\writeresponsebibliography` 只为 BibTeX 准备引用编号，不会输出文末参考文献总表；请保留它以及相应的加载命令。

### 按出版社要求调整参考文献格式

模板默认使用 **`elsarticle-num.bst`**。你可以将其替换为目标出版社或期刊提供的 `.bst` 文件，以满足不同的参考文献格式要求。也可以复制并修改 `elsarticle-num.bst`，另存为自定义样式，调整作者姓名、期刊名称、卷期页码、标点等输出格式。

更换样式的方法：

1. 将目标 `.bst` 文件放在主文件同目录，或安装到 TeX 发行版中。
2. 在 `response_template.tex` 末尾找到默认设置：

   ```latex
   \bibliographystyle{elsarticle-num}
   ```

   将其中的样式名称替换为目标文件名，省略 `.bst` 后缀。例如，使用 `publisher-style.bst` 时改为：

   ```latex
   \bibliographystyle{publisher-style}
   ```

3. 重新执行完整的 pdfLaTeX → BibTeX → pdfLaTeX → pdfLaTeX 编译流程，更新各回复下方的文献格式。

当前模板使用数字编号引用，应选用兼容的数值型 BibTeX 样式；如需作者—年份格式，还需相应调整引用与编号逻辑。

## 展示修改后的正文

在回复内嵌入 `revisedtext` 环境，可显示红色修改摘录：

```latex
\begin{author_response}
Your response.
\begin{revisedtext}
Revised manuscript text.
\end{revisedtext}
\end{author_response}
```

## 修改概览图

将图片命名为 `fig1.png`，放在主文件同目录即可显示。没有图片时，模板显示带有 `Figure` 字样的通用图框。

使用其他文件名或格式时，调整图环境中的文件检测与图片路径。`Summary of Revisions` 下方可自行补充实际修改的概述。

## 编译问题

- **引用显示为问号**：确认引用键存在于 `ref.bib` 中，并执行完整的 pdfLaTeX → BibTeX → pdfLaTeX → pdfLaTeX 流程。
- **找不到 `elsarticle-num.bst`**：安装 TeX 发行版中的 `elsarticle` 组件，或统一修改为已安装的数值型 BibTeX 样式。
- **审稿人交叉引用未更新**：再编译一次 pdfLaTeX。
- **首次使用没有文献列表**：空白模板不引用任何文献；填写真实引用并完成 BibTeX 编译后才会显示局部列表。

## 项目范围

此项目提供通用回复信模板与示例版式。具体审稿意见、论文数据和个人联系方式由使用者填写。模板预览展示默认通用内容。

欢迎通过 Pull Requests 改进排版与文献处理。

如果你喜欢这个模板，欢迎用发财的小手点一个 ⭐ Star，感谢支持！

模板使用过程中有任何问题，都欢迎在 [Issues](https://github.com/galaxyxora-cloud/latex-reviewer-response-template/issues) 中提问。
