---
title: 用 R 发邮件
date: 2019-12-01
slug: r-email
subtitle:
disable_mathjax: true
---

最近需要发送大量的邮件，遂写了个用 R 发邮件的脚本。我的目标是希望输入“学校名称”，“申请岗位”，“邮件主题”，“邮件地址”等信息，能自动生成我需要的 pdf 附件和邮件正文，再将它们按照给定的邮件主题发到指定邮箱。

我用了`RDCOMClient`包。它提供了 R 和本地 outlook 的接口，适合学校邮箱^[如果是 gmail, 也可以直接用 `gmailr` 包。]。`RDCOMClient` 现在还不能通过 cran 安装，需要指定地址：

```
install.packages("RDCOMClient", repos = "http://www.omegahat.net/R")
```

注意 outlook 邮件内容设置为 plain text 格式，而不是 HTML.

##  `RDCOMClient` 基本操作

```r
library(RDCOMClient)

# Set email variables
mail_address = "to@address.com"
subject = "email subject"
body = "
Dear Alice,

This is a test.

Regards,
Bob 
"

# interface to Outlook
OutApp <- COMCreate("Outlook.Application")
## create Email 
Email = OutApp$CreateItem(0)
## configure email parameter 
Email[["To"]] = mail_address
Email[["subject"]] = subject
Email[["body"]] = body

# Attach file(s) to the message
Email[["attachments"]]$Add("C:\\Users\\file1.pdf")
Email[["attachments"]]$Add("C:\\Users\\file2.pdf")

## send it
Email$Send()

# Close Outlook, clear the message
rm(OutApp, Email)
```

用 for loop 发送多封 one-to-one 邮件。

```r
email_list = list(
  "to1@address.com",
  "to2@address.com",
  "to3@address.com")


subject_list = list(
  "subject1",
  "subject2",
  "subject3"
)

body_list = list(
  body1,
  body2,
  body3
)

N = length(body_list)

for (i in 1:N) {
  ## init com api
  OutApp <- COMCreate("Outlook.Application")
  ## create an email named `outMail`
  outMail = OutApp$CreateItem(0)
  ## configure  email parameter 
  outMail[["To"]] = email_list[[i]]
  outMail[["subject"]] = subject_list[[i]]
  outMail[["body"]] = body_list[[i]]

  ## send it                     
  outMail$Send()
}

# Close Outlook, clear the message
rm(OutApp, outMail)
```

## 自动生成 pdf 文件和邮件正文

给定申请学校和职位等，生成对应的邮件内容以及 pdf 附件 (如 cover letter).

### 邮件正文

邮件正文就是一个 string, 我们可以用“回车键”或 `\n` 来换行。下例直接用回车键换行。

给定 `school_name` 和 `Position` 两个输入，它们的类型是 `str`. 一个简单的邮件如下：

```r
Position = "Assistant Professor"
school_name = "Harvard University"

# initial body
body_vector = c(
"Dear Members of the Search Committee,

I am writing to express my interest in the position of ", 
Position, 
" at ", 
school_name,
".

Attached are the application files. 

Yours sincerely, 
Your Name 

PhD Candidate in Physics
Massachusetts Institute of Technology
Phone: 0000-0000
Email: my@address.com
Website: https://www.myweb.com/")

body = paste(body_vector, collapse = '') 
# Show body
cat(body)
```

以下是生成的邮件正文。

```
Dear Members of the Search Committee,

I am writing to express my interest in the position of Assistant Professor at Harvard University.

Attached are the application files. 

Yours sincerely, 
Your Name 

PhD Candidate in Physics
Massachusetts Institute of Technology
Phone: 0000-0000
Email: my@address.com
Website: https://www.myweb.com/
```

### pdf 文件

我们用 pdflatex 来生成 pdf 文件。这里默认读者具备基本的 LaTeX 知识，会用 `\newcommand` 定义命令。

以 cover letter 为例。在 `cover.tex` 模板中定义 `\School`, `\Position`, `\University` 等变量，对应我们的输入。

```tex
\newcommand\School{IAS}
\newcommand\University{Princeton University}
\newcommand\Position{Assistant Professor}
```

确认 `cover.tex` 能生成对应的 `cover.pdf`。我们直接修改对应的 `\School` 等变量，再重新生成 pdf 即可。

```r 
# read file
tex_file = readLines("cover.tex",-1)
# Suppose the command is at Line 17
tex_file[17] = paste(
  c("\\newcommand\\School{", school_name,"}"), 
  collapse = '')
tex_file[18] = paste(
  c("\\newcommand\\University{", university_name,"}"), 
  collapse = '')
tex_file[19] = paste(
  c("\\newcommand\\Position{", Position,"}"), 
  collapse = '')    
# apply the changes to cover.tex   
writeLines(tex_file,"cover.tex")

# compile cover.tex 
system(paste0("pdflatex ", "cover.tex")) 
system(paste0("pdflatex ", "cover.tex")) 
```

## 封装

给定输入的 csv file, 其中 Header 包含 {Univeristy, School, Position, email_address, email_subject},
定义相应的函数完成上述操作。这里我就不赘述了。

如果对 R 和 LaTeX 有一定了解，掌握以上操作应该不超过 1 小时。如果需要发送的邮件超过 50 封，以上操作还是比手动发送省时一些，而且不容易出错。

最麻烦的还是一开始的“数据清理”，即从各学校的招聘信息，整理出干净的 csv 表格。这一步应该可以用爬虫解决，但我对爬虫不太熟，就直接手动处理了。毕竟需要邮件申请的学校数目是 500 而不是 50 。 