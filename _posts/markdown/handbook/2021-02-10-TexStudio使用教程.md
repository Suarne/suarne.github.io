---
title:  TexStudio使用教程
author: Wan Shicheng
date: 2021-02-10 10:47:31 +0800
categories: [HandBook, Latex]
tags: [latex]
math: true
---

写在前面：因为自己需要使用TexStudio，所以收集相关方面的教程。**所有的记录都来自互联网**，在参考部分均给出链接。安装教程[在这里](https://blog.csdn.net/aijava1/article/details/99282235)（单推使用CTEX+TexStudio模式，毕竟其他科目要写中文论文的时候这个也可以弄）LaTeX是所想即所得，Word是所见即所得。所以你要告诉Latex你要做什么，然后它才会去处理你给出的命令。

# 通常框架：

```latex
% 导言区
\documentclass{article}

\usepackage{宏包}

\title{文章标题}

\author{作者}

\date{日期}

% 正文区
\begin{document} % 开始正文

\tableofcontents % 目录（可以没有）

\maketitle % 显示标题

\section{一级标题}
\subsection{二级标题}
\subsubsection{三级标题}

\paragraph{} % 段落

\par % 表示该段结束，通常放句尾

\newpage % 另起一页

\end{document} % 结束正文
```

注意事项：

+ 带有下标的符号需要保持下标直体（非斜体）：$T_j \, \times$    $T_{\rm j} \, \checkmark$，可以用 `{\rm ...}` 将需要变为直体的部分括起来，如刚才的正确示范 Latex表达式 为 `T_{\rm j}`
+ 加粗显示文字：`\textbf{文字内容}` 【\bf{文字内容} 也可以达到加粗字体的效果，但texStudio不支持该表达式，使用后会使得后续文字全部加粗】
+ 斜体显示文字：`\textit{文字内容}`
+ 双引号：``文字内容”
+ 当需要文字顶格时，通过 `\noindent` 达到文字顶格效果
+ 使用 `\ \hspace*{\fill} \par` 可以勉强达到空一行的效果（下面会介绍更好的）

其它：

```latex
\usepackage{indentfirst} % 设置段落首行缩进
\usepackage{setspace} % 修改行间距需要调用的包
\usepackage[UTF8]{ctex} % 中文补充包，放在格式下面。

\thispagestyle{} % 用于生成该页的页眉页脚格式，有四个可选项；1，empty:将页眉页脚都设置成空白；2，plain:显示页眉页脚；3，headings:只显示页眉；4，myheadings:配合\markboth或\markright命令进行自定义。

\setcounter{page}{x} % 将此页页码重新设置为x。

\pagenumbering{} % 设置页码格式，arabic：阿拉伯数字；roman：罗马数字；alph：英文字母。要是把他们的首字母改成大写，那么数字格式会变成大写格式。

\usepackage[a4paper,left=10mm,right=10mm,top=15mm,bottom=15mm]{geometry} % 设置页面的环境,a4纸张大小，左右上下边距信息

~\\ % 添加一个空行
```

有必要说一下换行这档子事 ￣へ￣：

+ 在word里的”硬换行“（一个向下的箭头）等同于Latex中 ”`\\`“ 这个换行符，意思是让文字换行但还是属于同一个段落

+ 在word里的”软换行“（一个转弯的箭头）等同于Latex中 ”`\par`“ 这个段落标记或者直接空一行，意思是之后的文字就是另外一个段落。但是请注意，多个空行或`\par`连续只会被算作一个

+ Latex不像word那样有“段前、段后、行间距”等参数，只能通过设置“段间距、行间距”来控制排版，还可以像下面这样实现空行：

  ```latex
  \newline
  \vspace{2cm}
  \\[2\baselikeskip]
  \vspace{2\baselineskip}  % 2倍当前行距，这个行距是用\linespread调整过的行距
  \vspace{\fill}           % 间距一撑到底，下面的文字相当于“底部对齐”
  ```

+ 

# 摘要：

```latex
\begin{abstract} % 摘要开始
Your abstract.\\ % “\\”放在段尾，表示另起一行的意思
% “\par”在段首，表示另起一行，“\textbf{}”,花括号内的内容加粗显示
\par\textbf{Keywords: } Keyword1; Keyword2; Keyword3 
\end{abstract} % 摘要结束
```

# 公式：

```latex
\begin{equation} % “equation”表示会对插入公式进行自动编号
 % 需使用到大量的Latex语法表述数学公式、符号
\end{equation}

\begin{align} % 可以编辑多行公式，换行使用“\\”实现

\end{align}
```

在定义部分最常使用到公式书写，对于一整段的定义描述，以及相关性质、定理等内容，使用

```latex
%在引言区（头部）可以先做如下定义，对parameter进行自我定制
\newtheorem{definition}{\bf\it Definition \quad}[section] % 第一个{}中写parameter，第二个{}是显示的样式，第三个[]表示编号，section意味着会按照每章节自动编号，默认是全文连续编号
\usepackage{amsthm} % 如果要使用proof语句就必须要引入这个包
\newtheorem*{parameter}{parameter} % 表示不编号

\begin{parameter} % parameter可以为：definition/theorem/property/lemma/proof/rationale
	内容...
\end{parameter}
```



# 表格：

## 三线表格

```latex
\usepackage{booktabs} % 插入表格时需要调用的包
\usepackage{diagbox}  % 插表格用的宏包
\usepackage{multirow} % 插多行表格用的宏包

\begin{table}[htb] % h：hear，t：top，b：bottom，p：page，下一页。
	\setlength{\abovecaptionskip}{0.0cm} % 控制表名上边距
	\setlength{\belowcaptionskip}{0.2cm} % 控制表名下边距
	\caption{table name}
	\centering
	\label{tab1}
	\begin{tabular}{ccc} % c是居中对齐，l是左对齐
	% 几列就写几个c，表示内容居中写，哪里需要分割线就在哪里加“|”
		\toprule
		a & b & c \\ 
		\midrule
		1 & 2 & 3 \\
		\bottomrule
	\end{tabular}
\end{table}
```

## 合并多行的表格

```latex
\begin{table}
	\caption{Multi}
	\centering
	\label{tab1}
    \begin{tabular}{|c|c|c|c|c|}
        \hline
        \multirow{2}{*}{1} & 11 & 111 & 1111 & 11111 \\      
% 跨行（将两行合并为一行），第一个参数{2}指明跨几行，{*}是必须写的，{1}是指合并的单元格里要填写的内容。接下来就填写几行的内容，记得每行结束后都要加“\\”。
        \cline{2-5}
% \cline{}是指列间要加分割线，{2-5}是指接下来填写的是2-5列的内容。
        2 & 22 & 222 & 2222 & 22222 \\
        \hline
        3 & 33 & 333 & 3333 & 33333 \\
        \hline
    \end{tabular}
\end{table}
```

![合并多行的表格](https://img-blog.csdnimg.cn/20190306211206516.png)

补充：

+ 合并多列只要将表达式 `\multirow` 改成 `\multicolumn` 即可

## 可以分行的表格

```latex
\newcommand{\tabincell}[2]{\begin{tabular}{@{}#1@{}}#2\end{tabular}}  
% 表格自动换行
\begin{table*}[ht] % h：hear，t：top，b：bottom，p：page，下一页。
\caption{Title}
\centering
    \begin{tabular}{|c|c|c|}
        \toprule
        {\bf Method} & {\bf 1} & {\bf 2} \\      
% \bf表示字体加粗
        \hline
        \tabincell{c}{01\\02\\03}  & \tabincell{c}{11\\12}  & \tabincell{c}{21,\\22}  \\
% 需要分行的单元格的语句用\tabincell{c}{所填写第一行内容\\第二行内容···}，可以根据需要换行，也不限定换多少行。
        \hline
        11 & \tabincell{c}{dsads\\deawd} & \tabincell{c}{dewaf\\cewaef} \\
        \bottomrule
    \end{tabular}
\end{table*}
```

![可以分行的表格](https://img-blog.csdnimg.cn/20190306211646356.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzMzMDk0Ng==,size_16,color_FFFFFF,t_70)

## 带有斜线的表格

```latex

\begin{table}[ht] % h：hear，t：top，b：bottom，p：page，下一页。
\caption{example}
\centering
    \begin{tabular}{|c|cc|}
        \hline
        \diagbox{bottom}{top} & A & B \\   
        %斜线命令语句
        \hline
        1 & abc   & def \\
        2 & ghijk & lmn \\
        3 & opq   & rst \\
        \hline
    \end{tabular}
\end{table}
```

![带有斜线的表格](https://img-blog.csdnimg.cn/20190306211849471.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzMzMDk0Ng==,size_16,color_FFFFFF,t_70)

补充：

+ 插入跨栏表格只需要在table属性后面加上星号

  ```latex
  % 注意这里是“table*”而不是“table”
  \begin{table*}
    ...
  \end{table*}
  ```

+ 当表格在文中显示过大或过小，可以添加

  ```latex
  \setlength{\tabcolsep}{7mm}{
  \begin{tabular}{lll}
  	... 
  \end{tabular}}
  ```

+ 当表格在文中显示过宽或过窄，可以添加

  ```latex
  \resizebox{\textwidth}{7mm}{
  \begin{tabular}{cc}
      ...
  \end{tabular}}
  ```

+ 

# 图片：

```latex
\usepackage{graphicx} % 插入图片时需要调用的包
\usepackage[subfigure]{graphfig} % 插多张图片的宏包


\begin{figure}
	\centering % 居中。也可以设置靠左或者靠右。
	\includegraphics[height=4.5cm,width=7.5cm]{SVM_model.pdf}
	% 设置宽高，个人觉得比设置宽高比例好用，SVM_model是要插入的图片名称。
	\caption{This is a SVM model.} % 图片描述。
	\label{Fig1} % 引用图片时的标签。
\end{figure}
```

补充：

+ 插入跨栏图片只需要在figure属性后面加上星号

  ```latex
  % 注意这里是“figure*”而不是“figure”
  \begin{figure*}
    ...
  \end{figure*}
  ```

+ 通常我们写论文插入图片的格式为“eps，pdf，jpg”，这些格式都可以通过matelab编译之后把图片另存为就可以得到

  - 当在latex中插入eps文件时，需要导入以下包

    ```latex
    \usepackage{amsmath, amssymb, amsfonts, graphicx}
    \usepackage{epstopdf}
    ```

    

+ 以下内容均是在latex中对图片的自定义设置

  ```latex
  % 控制图片距离上一行内容的距离，单位可以为cm或pt
  \setlength{\abovecaptionskip}{0cm}
  % 控制图片距离下一行内容的距离，单位可以为cm或pt
  \setlength{\belowcaptionskip}{-0.cm}
  % \scalebox{} 表示对图片的缩放，参数值“1”为原大小
  % trim={left, lower, right, upper} 表示对图像进行裁剪，当图片有白边时很有用，单位为cm或pt
  % width 表示对图像的宽进行设置，单位为cm或pt，特别地，在写双栏论文时可以直接设置参数“0.5\textwidth”，这样图片就不会占用整个篇幅的宽度
  % height 同上
  \scalebox{\includegraphics[trim={}, clip, width=, height=]}{图片的相对路径}
  ```

  

+ 在这里贴出一个链接，是在latex中插图和颜色的[使用指导](http://tex.loria.fr/graph-pack/grf/grf.pdf)。

# 算法：

## 通常框架

```latex
% 这其中algorithm是浮动块，目的是让Latex能像处理图片表格那样处理伪代码块，（必须要有）
% 而algorithmic是负责算法的书写，控制排版等功能（可以用algorithmicx替换）

% ---------编写算法需要用到的导入包-------------
\usepackage{algorithm}
\usepackage{algorithmic}
\usepackage{amsmath} 
\usepackage{xcolor}

% ----重新定义require和ensure命令对应的关键字----
\renewcommand{\algorithmicrequire}{\textbf{Input:}} % 注意这里只是改变了REQUIRE的显示内容，使用还是以\REQUIRE
\renewcommand{\algorithmicensure}{\textbf{Output:}}
% ---(此处将默认的Require/Ensure自定义为Input/Output)---

\begin{algorithm} % 算法开始 
	\caption{Sample algorithm} % 算法的题目 
	\label{alg1} % 算法的标签，可以作为引用标志 
	\begin{algorithmic}[1] % 此处的[1]控制一下算法中的每句前面都有行号 
	\REQUIRE Text:Today is a good day. Variables:$u,v,w$. $G=(V,E)$ % 输入条件(此处的REQUIRE默认关键字为Require，在上面已自定义为Input) 
	\ENSURE Something... % 输出结果(此处的ENSURE默认关键字为Ensure在上面已自定义为Output) 
	
	% if-then-else 
	\IF{some condition is true} 
		\STATE do some processing 
	\ELSIF{some other condition is true} 
		\STATE do some different processing 
	\ELSE 
		\STATE do the default actions 
	\ENDIF 
	
	% for loop 
	\FOR{$i=0$ to $10$} 
		\STATE carry out some processing 
	\ENDFOR 
	\FORALL{$i$ such that $0\leq i\leq 10$} 
		\STATE carry out some processing 
	\ENDFOR 
	
	% while-loop 
	\WHILE{some condition holds} 
		\STATE carry out some processing 
	\ENDWHILE 
	
	% repeat-until loop 
	\REPEAT 
		\STATE carry out some processing 
	\UNTIL{some condition is met} 
	
	% infinite loop 
	\LOOP 
		\STATE this processing will be repeated forever 
	\ENDLOOP

	\RETURN return parameters
	\end{algorithmic} 
\end{algorithm}
```

效果图如下（该图引用自网友[ihoujie](https://blog.csdn.net/u014791046/article/details/51383698?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-6.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-6.nonecase)）：

![伪代码示意图](/assets/img/handbook/TexStudio使用教程/伪代码示意图.png)

# 其它：

```Latex
% 其实宏包algorithmicx和宏包algorithmic是一样的效果，只是一个关键字仅首字母大写，一个全为大写，这一点需要注意
% algorithicx的不同点在于能够对伪代码添加一些额外描述，方便阅读
% ---------编写算法需要用到的导入包-------------
\usepackage{algorithm}
\usepackage{algorithmicx}
\usepackage{algpseudocode} % algpseudocode宏包则是algorithmicx宏包的一部分，它提供了实际用来排版伪代码的环境
\usepackage{amsmath} 
\usepackage{xcolor}

\begin{algorithm}[htb]
	\caption{sample algorithm}
	\label{alg_label}
	\begin{algorithmic}[1]
		\Require input paramters
		\Ensure output
		\Procedure{function name}{paramters}
			\State Balabalabala...
			\Statex Balabalabala... % \Statex可以提供取消行号功能，当某些伪代码过长可以使用该关键字保证排版美观
		\EndProcedure
		\Return result.
	\end{algorithmic}
\end{algorithm}
```

更多例子可以[点这里](https://www.cnblogs.com/tsingke/p/6510343.html)。

要善于使用TexStudio的==**拼写和语法检查**==，有错误的地方只要右键一下就能自动更正。这里是[网友整理的教程](https://blog.csdn.net/yinqingwang/article/details/54583541)，也可以根据需要自行配置。【我在使用的时候候下载的软件版本不大对，高亮功能有问题，且没有找到修复办法，这里贴一份正规下载[网址](https://sourceforge.net/projects/texstudio/)（原TexStudio官网需要架梯子）】

# 引用文献：

推荐使用后缀bib文件管理引用文献

```latex
\usepackage{cite}  % 插引用文献的宏包

\cite{pa1} % 引用单个标签，如[1]
\cite{pa1，pa2，pa3} % 引用多个标签，如[1, 2, 3]

% plian参数表示一种参考文献的排序方式
\bibliographystyle{plain}
% 插入参考文献splncs04为.dst文件，也就是某一种参考文献的模板
\bibliographystyle{splncs04}
% xxx为.bib文件，即下载的Bibtex格式的参考文献
\bibliography{xxx} 
```

**Tip:** 如果在前面已经生成了其它风格的pdf，那么改代码之后直接编译会报错，这个时候只需要删除缓存文件.aux和.bbl，再重新编译一遍就可以解决。当然碰上其它非语法错误的也可以采用这个方法解决。

对于参考文献呈现样式共有以下几个参数：（**投稿时要注意收稿方的格式要求**）

1. plain，按字母的顺序排列，比较次序为作者、年度和标题；
2. unsrt，样式同plain，只是按照引用的先后排序；
3. alpha，用作者名首字母+年份后两位作标号，以字母顺序排序；
4. abbrv，类似plain，将月份全拼改为缩写，更显紧凑；
5. ieeetr，国际电气电子工程师协会期刊样式；
6. acm，美国计算机学会期刊样式；
7. siam，美国工业和应用数学学会期刊样式；
8. apalike，美国心理学学会期刊样式；

# 参考：

> 1. 血雨腥风霜：[Latex使用，插入标题...](https://blog.csdn.net/weixin_41466947/article/details/94719377)
> 2. ihoujie：[latex 插入算法](https://blog.csdn.net/u014791046/article/details/51383698?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-6.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-6.nonecase)
> 3. 张一根：[LaTeX学习系列之---LaTeX的总结](https://www.jianshu.com/p/1d99b3c883a6)
> 4. 碎金：[LaTeX排版入门的正确姿势（LaTeX排版入门教程）](https://www.bilibili.com/video/av88052289?t=2401)
> 5. Liam Huang：[一份其实很短的 LaTeX 入门文档](https://liam.page/2014/09/08/latex-introduction/)
> 6. 无宠不惊过一生：[LATEX中常用的插图和插表格的方法](https://blog.csdn.net/weixin_43330946/article/details/88255366?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-3.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-3.nonecase)
> 7. 刘海洋：《LaTex入门》
> 8. LaTex社区：[社区首页](http://www.latexstudio.net/)
> 9. hzYang ：[【开源项目】LaTeX速查手册](https://zhuanlan.zhihu.com/p/88249368)
> 10. MOON：[LaTex手册](https://www.moonpapers.com/manual/latex/)

