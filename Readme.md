​1. 项目目录结构
bash
my-thesis-book/
myproject
│ 
│  main.tex
│  Readme.md
│  Readme.txt
│  
├─.vscode
│      settings.json
│      
├─backmatter
│      appendix.tex
│      references.tex
│      
├─chapters
│      chapter01.tex
│      chapter02.tex
│      
├─figures
├─frontmatter
│      abstract.tex
│      acknowledgments.tex
│      
├─output
│  │  main.pdf
│  │  main.synctex.gz
│  │  
│  ├─aux_files
│  │      main.aux
│  │      main.fdb_latexmk
│  │      main.fls
│  │      main.log
│  │      main.out
│  │      main.toc
│  │      main.xdv
│  │      
│  └─logs
└─styles
        mybook.sty
        myenv.cfg
​2. 核心文件配置
​**(1) main.tex - 主文档**
latex
% 使用 xelatex 引擎
% !TEX program = xelatex

\documentclass[a4paper, 12pt]{book}

% 加载统一样式
\usepackage{styles/mybook}

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
​**(2) styles/mybook.sty - 统一宏包配置**
latex
% 基础宏包
\NeedsTeXFormat{LaTeX2e}
\ProvidesPackage{mybook}[2024/06/20 Custom Thesis Package]

% 字体配置（中文支持）
\usepackage{fontspec}
\setmainfont{Times New Roman}
\setsansfont{Arial}
\setmonofont{Courier New}
\usepackage{xeCJK}
\setCJKmainfont{SimSun}

% 数学宏包
\usepackage{amsmath, amssymb, amsthm}

% 图表宏包
\usepackage{graphicx, caption, subcaption}
\usepackage{booktabs} 

% 代码环境
\usepackage{listings}
\lstset{basicstyle=\ttfamily, breaklines=true}

% 超链接
\usepackage[hidelinks]{hyperref}

% 自定义环境（从 cfg 加载）
\usepackage{styles/myenv}

% 其他配置
\usepackage[margin=2.5cm]{geometry}
\linespread{1.3}
​**(3) styles/myenv.cfg - 自定义环境**
latex
% 定理类环境
\newtheorem{theorem}{定理}[chapter]
\newtheorem{definition}{定义}[chapter]

% 代码框环境
\lstnewenvironment{code}[1][]%
  {\lstset{language=Python, #1}}%
  {}

% 注释环境
\newcommand{\todo}[1]{{\color{red} [TODO: #1]}}
​3. VS Code 配置
​**(1) .vscode/settings.json**
json
{
  "latex-workshop.latex.autoBuild.run": "onFileChange",
  "latex-workshop.latex.recipe.default": "lastUsed",
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
        "-file-line-error",
        "%DOC%"
      ]
    }
  ],
  "latex-workshop.latex.recipes": [
    {
      "name": "latexmk",
      "tools": ["latexmk"]
    }
  ]
}
​4. 使用流程
​安装 VSCode 插件：

LaTeX Workshop
Code Spell Checker
​编译命令：

bash
# 手动编译
latexmk -xelatex -outdir=./output main.tex

​添加新章节：

tex
% 在 chapters/ 目录下新建 chapter03.tex
\chapter{深度学习基础}
\section{神经网络架构}
...
​5. 关键优势
​模块化管理：章节、样式、资源分离，便于协作和版本控制
​一键编译：通过 latexmk 自动处理交叉引用和 BibTeX
​中文友好：XeLaTeX + xeCJK 完美支持中英混排
​扩展性强：通过 .sty 和 .cfg 可快速添加新宏包或环境
