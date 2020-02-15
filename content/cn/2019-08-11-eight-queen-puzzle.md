---
title: 八皇后问题
date: 2019-08-11
slug: eight-queen-puzzle
subtitle:
---


## 缘起

我最早是从一本[网络小说](https://baike.baidu.com/item/%E5%A4%A9%E6%93%8E)中得知“八皇后问题”。那会我还在上初二，距今已超过十年了。小说的情节我早已忘了，唯独对主人公“思考数晚后找出八皇后问题所有的解”这一幕印象很深；书中说当年数学家高斯也没能找全所有的解。我一度对“八皇后问题”很着迷，试着穷举了 `n=5` 和 `n=6` 的情形，但没有找到任何规律。我的尝试仍是基于手算，完全没有想过靠计算机来解决它。彼时学校开设了《信息技术课》，老师教了一些 QBASIC 语言。但因为不是主课，教学和学习兴趣都不高。如果当时我有问信技课的老师，或许会早一点接触到编程的威力。

“八皇后问题”由一位德国国际象棋家于 一八四八年提出：

> 在标准的八八六十四格棋盘上放置八个皇后，使皇后之间两两无法吃子。这样的摆法一共有多少种？

高斯曾在一封给好友的信中猜想这个问题有 72 个解，但不久后所有的 92 个解均被人发现。如果不考虑对称性，“独立的解”应该是 12 个。完整的证明可以追溯到一九二一 年，由此可见对于 `n=8` 其实也不需要计算机。《美国数学月刊》的一篇论文 [The n-Queens Problem](https://sci-hub.tw/10.1080/00029890.1994.11997004)
回顾了人们对这个问题的研究，并猜想对一般的 n-Queens 问题，有
$$
\lim_{n \to \infty} \frac{\log T(n)}{n \log n} = \beta >0
$$
其中 β 是某个正常数，T(n) 是 n-queens 问题解的个数，如 T(8)=92. 找到 T(n) 的解析表达式（如果存在的话）是很难的。二零一七年三位苏格兰数学家[证明了 n-queens 完成问题和 P-NP 问题的联系](https://jair.org/index.php/jair/article/view/11079)，以致有“谣言”说解决了
 n-queens 问题可以拿 Clay 奖。这个说法后来被[官方辟谣了。](http://www.claymath.org/events/news/8-queens-puzzle)

## 计算 T(n)

 我试着用 R 语言求解 n-queens 问题。因为不太熟悉 `list` 类型的操作，总计用了约两小时，为了绕过 `list` 表示路径还特意安装了 [sets 包](https://cran.r-project.org/web/packages/sets/index.html)。 

```r
library(sets) # for functions tuple() and set()

is_double_safe <- function(p_1,p_2){
  if(p_1[1] == p_2[1] | p_1[2] == p_2[2]) return(FALSE)
  dif = p_1 - p_2
  return(abs(dif[1]) != abs(dif[2]))
}

is_many_safe <- function(path, p){
  for (i in path) {
    if(!is_double_safe(unlist(i),p)) return(FALSE)
  }
  return(TRUE)
}

# return the list of possible next positions
get_next_positions <- function(path, N){
  out = c()
  x = length(path) + 1
  for (i in 1:N) {
    if( is_many_safe(path,c(x,i)) ) out = c(out,i)
  }
  if(length(out) == 0) return(list())
  mapply(c, x, out, SIMPLIFY=FALSE)
}

# return the set of possible paths
get_list_of_longer_paths <- function(path, N){
  next_list = get_next_positions(path, N)
  if(length(next_list) ==0) return(list())
  out <- vector("list", length(next_list)) 
  for (i in 1:length(next_list)) {
    out[[i]] = set_union(path, as.tuple(next_list[[i]]))  
  }
  return(out)
}

# tree is a list of paths
get_bigger_tree <- function(path_list, N){
  out = list()
  for (path in path_list) {
    out = c(out, get_list_of_longer_paths(path, N))
  }
  return(out)
}

# solve the problem with input N
show_board <- function(N){
  out = list()
  for (var in seq(1.0,N,1.0)) {
    # sorry for the ugly three outer fcts...
    tree = list(set(tuple(1,var))) # intial the tree
    while (length(tree[[1]]) != N) {
      tree = get_bigger_tree(tree, N)
      if(length(tree) == 0) break 
    }
    out = c(out, tree)
  }
  if(length(out) == 0){
    print("No solution.")
    return()
  }
  out
}

N = 8 # board size
show_board(N)
```

Haskell Wiki 上有专门针对这个问题的[练习和解答](https://wiki.haskell.org/99_questions/90_to_94)。 