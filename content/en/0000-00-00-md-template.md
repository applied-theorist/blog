---
title: "md Template"
date: 2017-06-30
---


## Markdown or R Markdown

A `.md` post is rendered through [Blackfriday](https://gohugo.io/overview/configuration/), and an `Rmd` document is compiled by [rmarkdown](http://rmarkdown.rstudio.com) and [Pandoc](http://pandoc.org). 

One should use `.Rmd` files for math-intensive or running code embedded posts only. The outputs of `.md` files are cleaner.

Only with `md` one can write a to-do list as below.

- [x] Write a paper.
- [ ] Submit.
- [ ] ...
- [ ] *Publish!*

# Title (`#`)

## Section (`##`)

### Subsection (`###`)

#### Sub-subsection (`####`)

## Footnotes and quotes

Here is a footnote.^[Footnote *text* must **come after** the comma] And here is another quote[^footnote] in a different way. 

[^footnote]: Here is the *text* of the **footnote**.

Here is a quote:

> Nothing is built on stone; all is built in sand. But we must build as if the sand were stone.
> --- *Jorge Luis Borges*

## Code display

```c
#include <stdio.h>

/* Hello */
int main(void){
  printf("Hello, World!");
  return 0;
}
```

```js
function helloWorld () {
  alert("Hello, World!")
}
```

## Tables

A table is centered by default.

| Name              | Markdown            | HTML tag            |
| ----------------- | ------------------- | --------------------|
| *Emphasis*        | `*Emphasis*`        | `<em></em>`         |
| **Strong**        | `**Strong**`        | `<strong></strong>` |
| `code`            | `` `code` ``        | `<code></code>`     |
| ~~Strikethrough~~ | `~~Strikethrough~~` | `<del></del`        |
| <u>Underline</u>  | `<u>underline</u>`  | `<u></u>`           |

## Images

An image (GIF) (automatically centered when it is appropriate):

![Happy Elmo](https://slides.yihui.name/gif/happy-elmo.gif)


