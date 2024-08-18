---
title: Markdown 语法
author: cotes
date: 2019-08-07 11:23:00 +0800
categories: [Blogging, Tutorial]
tags: [markdown]
render_with_liquid: false
---

## 代码

### 反引号

要将单词或短语表示为代码，请将其包裹在反引号 (`) 中

```md
`code`
```
效果：`code`

### 转义反引号

可以通过将单词或短语包裹在双反引号(``)中。

```md
``use `code` in markdown``
```

效果：``use `code` in markdown``

## 表格

Markdown 制作表格使用 | 来分隔不同的单元格，使用 - 来分隔表头和其他行。

```md
| 表头 | 表头 |
| ---  | ---  |
| 单元格  | 单元格  |
| 单元格  | 单元格  |
```

设置表格对齐：
+ `-:` 设置内容和标题栏居右对齐
+ `:-` 设置内容和标题栏居左对齐
+ `:-:` 设置内容和标题栏居中对齐

## 高级用法

### 反斜杠转义

# *参考来源*

