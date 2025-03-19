# LaTeX 毕业设计项目架构说明
## 📂 项目目录结构
myproject/
│
├── .vscode/ # VS Code 配置
│ └── settings.json
│
├── backmatter/ # 后置内容
│ ├── appendix.tex # 附录
│ └── references.tex # 参考文献
│
├── chapters/ # 章节内容
│ ├── chapter01.tex
│ └── chapter02.tex
│
├── figures/ # 图片资源
├── frontmatter/ # 前置内容
│ ├── abstract.tex # 摘要
│ └── acknowledgments.tex # 致谢
│
├── output/ # 编译输出
│ ├── aux_files/ # 中间文件
│ └── logs/ # 日志文件
│
├── styles/ # 样式配置
│ ├── mybook.sty # 主样式
│ └── myenv.cfg # 自定义环境
│
├── main.tex # 主文档入口
├── Readme.md # 项目说明
└── Readme.txt # 备用说明


## 🎛️ 核心文件配置

### 1. 主文档 `main.tex`
```latex
% 使用 xelatex 引擎
% !TEX program = xelatex

\documentclass[a4paper, 12pt]{book}
\usepackage{styles/mybook}  % 加载统一样式

% 文档元信息
\title{毕业设计概念笔记}
\author{Your Name}
\date{\today}

\begin{document}
    \frontmatter
    \include{frontmatter/abstract}
    \include{frontmatter/acknowledgments}
    
    \mainmatter
    \include{chapters/chapter01}
    \include{chapters/chapter02}
    
    \backmatter
    \include{backmatter/appendix}
    \bibliography{backmatter/references}
\end{document}
2. 统一样式 styles/mybook.sty
latex
\NeedsTeXFormat{LaTeX2e}
\ProvidesPackage{mybook}[2024/06/20 Custom Thesis Package]

% 中文字体配置
\usepackage{fontspec}
\setmainfont{Times New Roman}
\setsansfont{Arial}
\setmonofont{Courier New}
\usepackage{xeCJK}
\setCJKmainfont{SimSun}

% 数学公式支持
\usepackage{amsmath, amssymb, amsthm}

% 图表配置
\usepackage{graphicx, caption, subcaption}
\usepackage{booktabs} 

% 代码环境
\usepackage{listings}
\lstset{basicstyle=\ttfamily, breaklines=true}

% 超链接支持
\usepackage[hidelinks]{hyperref}

% 自定义环境
\usepackage{styles/myenv}

% 页面布局
\usepackage[margin=2.5cm]{geometry}
\linespread{1.3}  % 行距设置
3. 自定义环境 styles/myenv.cfg
latex
% 定理类环境
\newtheorem{theorem}{定理}[chapter]
\newtheorem{definition}{定义}[chapter]

% 代码环境
\lstnewenvironment{code}[1][]%
  {\lstset{language=Python, #1}}%
  {}

% 注释标记
\newcommand{\todo}[1]{{\color{red} [TODO: #1]}}
⚙️ VS Code 配置
.vscode/settings.json 配置示例：

json
{
  "latex-workshop.latex.autoBuild.run": "onFileChange",
  "latex-workshop.latex.root": "main.tex",
  "latex-workshop.latex.tools": [
    {
      "name": "latexmk",
      "command": "latexmk",
      "args": [
        "-xelatex",
        "-output-directory=output",
        "-aux-directory=output/aux_files",
        "-g",
        "-synctex=1",
        "-interaction=nonstopmode",
        "%DOC%"
      ]
    }
  ]
}
🚀 使用流程
环境准备
安装 VS Code 插件：
LaTeX Workshop
Code Spell Checker
编译命令
bash
# 手动编译
latexmk -xelatex -outdir=./output main.tex
添加新章节
在 chapters/ 目录新建 .tex 文件（例：chapter03.tex）
在 main.tex 中添加引用：
latex
\input{chapters/chapter03}
✅ 关键优势
​模块化管理：内容、样式、资源分离，支持多人协作
​智能编译：latexmk 自动处理交叉引用和文献引用
​中文友好：XeLaTeX + xeCJK 完美支持中文排版
​高效调试：VS Code 实时编译 + 语法检查
​扩展性强：通过 .sty 文件快速添加新功能