---
title: "How I use LaTeX"
date: 2019-03-14
categories: 
- English
tags: 
- LaTeX
- beamer-template
- pdf2htmlEX
---

> Revised on August 2019.

I have been using LaTeX for over six years since 2013. After trials and errors, I have converged to the following setup, which should be most appropriate for the writing of economic theory papers and slides.     

## My local LaTeX setup

### MiKTeX + TeXstudio on Windows

For Windows users, [MiKTeX](https://miktex.org/) is a no-brainer. As for the editor, I mainly use TeXstudio which is bundled with the MiKTeX distribution as of August 2019. I also suggest using [Fira Mono](https://github.com/mozilla/Fira), an open source typeface releases by Mozilla, as the editor font.

### Slides

You can find my Beamer template on [overleaf](https://www.overleaf.com/read/svrzvrjyhkpk). Specifically, one should use the 16:9 ratio rather than the default.

```tex
\documentclass[14pt,aspectratio=169]{beamer}
```

Sometimes I use the [`article` document class to produce slides](/en/2019/08/article-to-slides). The trick is to exploit the `geometry` package.       

### Papers 

The default format settings of `article` and `amsart` classes are well-designed and [should NOT be changed.](/cn/2019/08/typography)

### Packages  

- Use both `hyperref` and `cleveref`.

   ```tex
   \PassOptionsToPackage{hyphens}{url}\usepackage[colorlinks, breaklinks=true,citecolor=black,linkcolor=NavyBlue,pagebackref]{hyperref}
   \usepackage[capitalise,nameinlink,noabbrev,sort]{cleveref}
   ```
- Almost all LaTeX users use the `amsmath` package. One might consider reading at least once the [User's Guide](http://texdoc.net/texmf-dist/doc/latex/amsmath/amsldoc.pdf). It is short and well-documented.

### TikZ

TikZ is *the* tool for LaTeX graphs. I have always wanted to collect all my TikZ graphs together but just did not make it yet. Some useful resources include:


- [Minimal introduction for Tikz](https://cremeronline.com/LaTeX/minimaltikz.pdf)

- Some [economics related diagrams](http://www.texample.net/tikz/examples/area/economics/)

- [Templates](https://sites.google.com/site/kochiuyu/home) by Chiu  

## Use LaTeX with GitHub

I am trying to internalize GitHub to my LaTeX workflow. If you are interested too, this [tutorial](https://github.com/dspinellis/latex-advice) can be a good start. As an aside, it is not a good idea to use `\left` and `\big` as suggested in that tutorial. It's better to compare the effects of `\big`, `\Big` (and `\BBig`...)  manually and pick the favorite one.     

## pdf2htmlEX

I find [pdf2htmlEX](https://github.com/coolwanglu/pdf2htmlEX) worth introducing though it is not really related to LaTeX. Roughly speaking, it turns a pdf file to a html without loss of form. This tool was initially created for use in Unix, but volunteers have modified it and here is a [video](https://www.youtube.com/watch?v=4ASGOzj1Qtw) about how to run `pdf2htmlex.exe` in windows. 

Sales pitch of pdf2htmlEX include: 

- same typography quality as the LaTeXed pdf

- clickable links are remained and a table of contents is provided at the left side