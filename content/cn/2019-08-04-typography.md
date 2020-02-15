---
title: 十分钟课程：排版与字体
date: 2019-08-04T20:00:00
slug: typography
subtitle:
disable_mathjax: true
disable_highlight: true
---

读了一本关于排版的书，《实战字体排印学》 *(Practical Typography).* 全书对我有用的部分基本都在第一章的[十分钟课程](https://practicaltypography.com/typography-in-ten-minutes.html)里。以下是对这十分钟的总结。排名分先后，越靠前越重要。

## 排版与字型要点

1. 字型大小 (Point size) ：字号有多大。

    - 纸质文字应选 **10--12 point** point. 这也是 LaTeX 中 `article` 类的可选字号范围。
    - 网络文字应选 **15--25 pixels**, 具体最佳大小取决于所用的字体。  
	
1. 行距 (Line spacing) 应是所选字号的 120--145%. 对于 LaTeX 的 `article` 类，12pt 的默认行距是 14.5pt, [11pt 的默认行距是 13.6pt](https://tex.stackexchange.com/questions/425404/what-is-the-default-space-between-lines-in-latex). 由于我常用的 palatino 字体需要大一点的行距，我会把行距变成 145%.
    
	```
	\renewcommand{\baselinestretch}{1.2}
	```
	
    我不用 word, 但作者认为 word 默认的 single line 和 one-half line 都是很糟糕的选择。CSS 中，则用 `line-height` property 设置行距。

1. 行宽 (Line length) 应在 45--90 characters，即字母表重复 2--3 遍。

    - 对于纸质文章，这意味着页边距 (margin) 大于 1 英寸 (2.54cm)。LaTeX `article` 类的默认页边距很好看，并且会根据字号调整。不用修改。
	
1. 最后是字体选择。

	- LaTeX 默认的字体可以配套使用 `lmodern` 包。它本身是很经典的字体。除此之外还可以考虑
	    - charter 字体。
		
			```
	        \usepackage[charter,cal]{mathdesign}
			```
	    - Palatino 字体。我很喜欢这个字体，因为它有漂亮的斜体和 small caps. 唯一的缺点是周围太多人都在用它。
		
		    ```
		    \usepackage{newpxtext,newpxmath}
			```

## 总结

直接用 LaTeX 默认的 `article` 类或 `amsart` 类就好，最多改改字体，别另外折腾。
	


  	
	