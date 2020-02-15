---
title: Convert LaTeX article to slides
date: 2019-08-18
slug: article-to-slides
subtitle:
--- 

It is a pleasure to teach, but not always so when preparing teaching materials. As for teaching materials, slides are usually necessary; Sometimes the instructor need prepare lecture notes as well. If the instructor prepares lecture notes first, then she might not be as willing to produce the slides as it becomes drudgery to convert `article` to `beamer` documentclass. Since many macros/commands in `article` do not work in `beamer`, it would be great *not* to change the documentclass. Actually, that is not only possible but quite easy.

## Use `article` documentclass for slides
           
Just three steps.

1. Change the fonts.

	```tex
	\usepackage{cmbright} 
	```

	One should choose a sans-serif font for slides. 

2. `geometry` package.

	```
	\usepackage[ 
	papersize={12.8cm,9.6cm}, % pre size copied from beamer
	hmargin=1cm,%
	vmargin=0.5cm,%
	head=1cm,% might be changed later
	headsep=0pt,%
	foot=1cm% might be changed later
	]{geometry}
	```
	
3. Add a title page.
	```tex
	\vspace*{\fill}
	\begingroup
	  \centering
	   \Huge Title
	
	   \large Subtitle
	   
	   \large Name
	   
	   \large Time

	\endgroup
	\vspace*{\fill}
	```  

## Uncomment `fancyhdr` and some formatting

The three steps should be enough in most cases. Since I also have used the `fancyhdr` package, I should comment them as below.    	

```tex
%\usepackage{fancyhdr} % headers and footers
%
% blabla
%
% headers and footers
%\pagestyle{fancy} 
%\fancyhf{}
%\rhead{}
%\lhead{}
%\cfoot{\thepage}
```

We finish the slides by some extra formatting:

- Delete unnecessary contents. For example, remarks and proofs.

- Use ~~\newpage~~ `\clearpage` like `\frame{}` in beamer.

- If needed, use `\\~\\` to add an extra line.

- Of course, beamer is still recommended if you have time. [Here](https://www.overleaf.com/read/svrzvrjyhkpk) is my template using pdflatex. 
