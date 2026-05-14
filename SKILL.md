---
name: cumcm-paper-architect
description: >
  将零散的赛题描述、代码逻辑和运行结果转化为规范的数学建模竞赛 LaTeX 论文。
  当用户提供赛题PDF/截图、Python/MATLAB 求解代码、建模笔记、Excel数据或运行结果时，
  使用此技能自动生成符合 CUMCM(国赛)/MCM-ICM(美赛)/华东杯/亚太杯/深圳杯格式标准的 .tex 论文文件。
  核心能力包括：从代码逆向推导数学模型、提取目标函数和约束条件、生成符号说明和模型假设、
  构建完整的论文结构（摘要→问题重述→问题分析→模型建立与求解→模型检验→参考文献→附录）。
  触发场景：用户提到"写论文"、"数模论文"、"建模论文"、"latex论文"、"国赛论文"、
  "华东杯"、"美赛"、"MCM"、"ICM"、"亚太杯"、"帮我写论文"、"根据代码写论文"、
  "根据赛题写论文"、"把代码转成论文"、"生成latex论文"时触发。
  即使用户只是简单说"帮我写数模论文"、"整理成论文"、"搞一篇论文"，
  或者只提供了赛题+代码+结果而没有明确说要写论文，也请使用此技能。
  用户提到遗传算法、蒙特卡洛、模拟退火、粒子群、神经网络等算法并说要写成论文时也应触发。
---

# 数模论文智能架构师

## 工作流

```
输入（题目+代码+笔记+结果） → 解析材料 → 完备性检查 → 数学形式化 → LaTeX 生成
```

按顺序执行，材料不全在第二步主动追问，不要跳过。用户确认"先写后补"则标注 `[待补充]` 继续。

---

## 第一步：解析材料

读取用户提供的所有材料。代码逆向建模要点：识别**目标函数**（minimize/maximize/loss/obj）、**约束条件**（等式/不等式/边界）、**算法参数**（种群大小、交叉率、迭代次数等）、**输入输出变量**。不认识的算法先搜索再写。

## 第二步：完备性检查

生成前检查：每个子问题有模型/结果？有灵敏度分析？有收敛性分析（随机/迭代算法）？有模型检验（误差/残差/交叉验证）？有图表数据？缺失则追问，用户说先写则标 `[待补充]`。

## 第三步：数学形式化

### 问题分析
从建模者视角拆解子问题关系（递进/并列），说明模型选择动机。建议配流程图。

### 模型假设
**必须用文字段落（不能用表格），每条带编号独立成段。** 假设类型：已知数据保真、仅考虑主要条件、平稳性规定、简化假设、参数规定。每条必须与后续模型直接相关，不写空洞套话。

示例：
```latex
\textbf{假设 1：}题目给出的车辆成本数据和排货信息准确可靠，不存在录入异常值。
\textbf{假设 2：}公路运输过程中除行驶里程和装卸时间外，不考虑交通拥堵等不确定因素。
```

### 符号说明
**紧跟模型假设之后、模型建立之前，作为第4章，绝对不能放附录。** 用三线表，只写主要全局变量，优先希腊字母。

```latex
\begin{table}[h]
\centering\caption{本文的主要符号说明}\label{tab:symbols}
\begin{tabular}{cll}\toprule
符号 & 含义 & 单位 \\\midrule
$\alpha$ & 非直线系数 & --- \\
$v$ & 平均行驶速度 & km/h \\
\bottomrule\end{tabular}
\end{table}
```

---

## 第四步：LaTeX 论文生成

### 论文结构

```
第一页：论文题目 + 摘要 + 关键词
第二页：目录
第三页起：正文（页码从1开始）
1. 问题重述 → 2. 问题分析 → 3. 模型假设 → 4. 符号说明
5~N. 各问题模型的建立与求解（每问题含：数据预处理、模型建立、求解、结果分析）
模型检验与评价 → 参考文献（GB/T 7714） → 附录
```

**目录要求：** 目录必须包含所有正文章节 + 参考文献 + 所有附录条目。附录在目录中的标题必须与正文 `\section*` 标题完全一致。

**参考文献要求：** 正文中用 `\cite{key}` 引用，显示为 `[1]`。格式：期刊`[J]`、书籍`[M]`、学位论文`[D]`、会议`[C]`、专利`[P]`、标准`[S]`、报告`[R]`、电子文献`[EB/OL]`。

### 各章节写作要点

**摘要（最重要，与题目同页）：** 开头段写背景+问题+方法；每问题一段写"针对问题X，建立XXX模型，求解得到XXX结果"；结尾写总结评价。关键词顶格左对齐（`\noindent\textbf{关键词：}...`）。

**问题重述：** 用自己的话改写题目，不复制原文。说明情景、列出关键数据、提出明确问题。半页到一页。

**问题分析：** 承上启下，反映建模认识程度。每问题一小节，简明扼要不放结论，配流程图辅助说明。

**模型建立与求解（每个问题的章节）：** 统一包含四部分——

1. **数据预处理**：描述数据来源、清洗逻辑，用文字说明
2. **模型建立**：写建模动机→逐步推导→公式（编号+符号解释）→约束含义。**每个公式前后必须有文字**：前说明为什么引入，后解释各部分含义。模型必须与代码逻辑一致
3. **模型求解**：描述算法步骤、参数设置。可配流程图或伪代码
4. **结果分析**：必须有表格+文字解读+图表引用，不是只放数字。解读数据含义和发现

### 伪代码规范

**必须放在 `\begin{algorithm}[h]...\end{algorithm}` 内的 `\begin{algorithmic}...\end{algorithmic}` 环境中。** 使用 `\For{...}...\EndFor`、`\To`（大写T小写o，**不是 `\TO`**——未定义会报错）、`\State`、`\If{...}...\EndIf`。绝对禁止在正文或 equation 里直接用这些宏。

```latex
\begin{algorithm}[h]
\caption{遗传算法求解流程}\label{alg:ga}
\begin{algorithmic}[1]
\State 初始化种群 $P_0$，大小 $N=50$，迭代 $T=200$
\For{$t = 1$ \To $T$}
    \State 计算适应度 $f(x_i)$
    \State 轮盘赌选择
    \State 交叉（$p_c=0.8$）、变异（$p_m=0.02$）
\EndFor
\State 输出最优解 $x^*$
\end{algorithmic}
\end{algorithm}
```

### 图表规范

每章节必须配图。无图表文件则插入 `\textbf{[图X待插入：描述]}` 占位符。格式：

```latex
\begin{figure}[h]
\centering\includegraphics[width=0.85\textwidth]{figures/文件名.png}
\caption{图表标题}\label{fig:标签}
\end{figure}
```

图表标题居中，全文连续编号。正文中必须引用（"如图X所示"、"表X表明"）。

### LaTeX 文档模板

**必须包含以下完整结构：**

```latex
\documentclass[12pt,a4paper]{article}
\usepackage[UTF8]{ctex}
\usepackage{amsmath,amssymb,amsfonts}
\usepackage{geometry}
\usepackage{graphicx}
\usepackage{booktabs}
\usepackage{algorithm}
\usepackage{algpseudocode}
\usepackage{hyperref}
\usepackage{cite}
\usepackage{array}
\usepackage{multirow}
\usepackage{xcolor}
\usepackage{float}
\usepackage{fancyhdr}
\usepackage{titlesec}
\usepackage[justification=centering]{caption}
\usepackage{setspace}
\usepackage{listings}
\lstset{basicstyle=\small\ttfamily, frame=single, breaklines=true, numbers=left, numberstyle=\tiny}

\geometry{left=2.5cm,right=2.5cm,top=3cm,bottom=2.5cm}
\pagestyle{fancy}\fancyhf{}\cfoot{\thepage}
\renewcommand{\headrulewidth}{0pt}

% 标题格式：仅一级标题居中，二三级左对齐
\titleformat{\section}[block]
  {\centering\fontsize{14pt}{16pt}\selectfont\heiti}{\thesection}{1em}{}
\titleformat{\subsection}[block]
  {\fontsize{12pt}{14pt}\selectfont\heiti}{\thesubsection}{1em}{}
\titleformat{\subsubsection}[block]
  {\fontsize{12pt}{14pt}\selectfont\heiti}{\thesubsubsection}{1em}{}
\titleformat{name=\section,numberless}[block]
  {\centering\fontsize{14pt}{16pt}\selectfont\heiti}{}{0em}{}

% 公式/图表编号按章节，图表标题居中
\renewcommand{\theequation}{\arabic{section}.\arabic{equation}}
\numberwithin{equation}{section}
\numberwithin{figure}{section}
\numberwithin{table}{section}
\renewcommand{\baselinestretch}{1.0}
\captionsetup[figure]{justification=centering}
\captionsetup[table]{justification=centering}
\captionsetup[algorithm]{justification=centering}

\title{\fontsize{16pt}{18pt}\selectfont\heiti 论文题目}
\author{}\date{}

\begin{document}
\maketitle
\thispagestyle{empty}
% 摘要和关键词在此处插入（与题目同页）
\newpage
\tableofcontents
\thispagestyle{empty}
\newpage
\setcounter{page}{1}
\fontsize{12pt}{12pt}\selectfont

% 正文各章节...

% 参考文献
\section{参考文献}
\begin{thebibliography}{99}
\bibitem{ref1} 参考文献条目（GB/T 7714 格式）
\end{thebibliography}

% 附录
\appendix
\renewcommand{\thesection}{附录\Alph{section}}
% 附录 A：文件名 —— 标题与 TOC 中条目必须完全一致
\section*{附录A\quad 文件名}
\addcontentsline{toc}{section}{附录A\quad 文件名}
\begin{minipage}{\textwidth}
\lstset{basicstyle=\small\ttfamily, frame=single, breaklines=true}
% 代码或图表内容
\end{minipage}

% 如有多个附录，重复以上 \section* + \addcontentsline + minipage 结构
% 确保 \addcontentsline 的文本与 \section* 的文本完全相同

\end{document}
```

### 参考文献与附录

**参考文献：** 用 `\usepackage{cite}`，正文中用 `\cite{key}` 引用，显示为 `[1]` 上标形式。格式遵循 GB/T 7714：

```latex
\begin{thebibliography}{99}
\bibitem{key1} 作者. 题名[J]. 期刊名, 年, 卷(期): 页码.
\bibitem{key2} 作者. 书名[M]. 出版地: 出版社, 出版年.
\end{thebibliography}
```
期刊 `[J]`、书籍 `[M]`、学位论文 `[D]`、会议 `[C]`、专利 `[P]`、标准 `[S]`、报告 `[R]`、电子文献 `[EB/OL]`。置于模型检验之后、附录之前。

**附录：** 每个附录文件用独立 section，标题格式 `附录 A：文件名` 或 `附录 A：程序代码`。用 `minipage` 包裹 `verbatim` 或 `listings` 显示代码。

```latex
\begin{minipage}{\textwidth}
\lstset{basicstyle=\small\ttfamily, frame=single, breaklines=true}
\lstinputlisting[language=Matlab]{code/main.m}
\end{minipage}
```

**TOC 中必须包含附录条目**，且附录标题与正文完全一致。用 `\addcontentsline{toc}{section}{附录A\quad 文件名}` 确保一致性。

---

## 自检清单

生成完成后检查：
- [ ] 一级标题居中，二三级左对齐
- [ ] 题目三号黑体，正文小四宋体单倍行距
- [ ] 摘要与题目同页（第一页），关键词左对齐
- [ ] 目录第二页，正文第三页起，页码从1开始脚注居中
- [ ] 公式编号 (章节.序号)，图表连续编号，正文有引用
- [ ] 伪代码全在 algorithmic 环境，用 `\To` 非 `\TO`
- [ ] 模型假设文字段落（非表格），符号说明在模型之前（非附录）
- [ ] 每问题章节含：数据预处理+模型文字说明+求解文字+结果分析+图表
- [ ] 模型与代码逻辑一致，摘要数值与运行结果一致
- [ ] 参考文献 GB/T 7714 格式，正文有 `\cite` 引用
- [ ] 附录 minipage 包裹代码/图表，TOC 中附录条目与正文标题一致

---

## 输出

输出完整 `.tex` 文件，应可直接用 `xelatex` 编译。用户提供参赛编号或题目编号时自动填入封面。
