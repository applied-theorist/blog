---
title: "Rmarkdown 贴士"
date: 2019-03-31
categories:
- Chinese 
tags: 
- Rmarkdown
---

这学期“经验产业经济学”的课程作业都是用 Rmarkdown 写的。记录几个常用的 Tips.

## 全局设置

```r
​```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE,message = FALSE)
​```
```
命令 `message = FALSE` 可以在报告中不显示 warnings 等信息.

## 显示 R 代码但不运行

```r
​```{r 35, eval = FALSE}
result <- optim(par = theta, fn = NLLS_objective_A3, df_share = df_share_smooth, method = 'Nelder-Mead')
save(result, file =here("result.RData"))
​```
```
命令 `eval = FALSE` 选择不运行代码。减少报告生成时间。

## 其他命令

-	`include=FALSE` 不在文档中显示该代码块


