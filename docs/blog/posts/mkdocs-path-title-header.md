---
date: 2023-10-07
categories:
  - Usage
slug: post-and-notes-url-title-header-format
#title: title
---

# blog和note的url, title, header的关系说明

## Blog

blog部分post URL是可以自定义的，目前的设置的`post_url_format: "{slug}"`，通过header部分的slug可以自定义slug信息。

## Note

note部分，
不添加title属性，不包含一级标题的话，url显示md文件名，导航显示文件名，标题显示文件名。
不添加title属性，包含一级标题的话，url显示md文件名，导航显示一级标题，标题显示一级标题。
添加title属性，不包含一级标题的话，url显示md文件名，导航显示title，标题显示title，
添加title属性，包含一级标题的话，url显示md文件名，导航显示title，标题显示一级标题。

总结一下，url是跟着md文件名走的，而title控制左边导航栏显示名称，一级标题控制这篇笔记最上面的标题名称，如果确实则用另一个互补，都缺失用md文件名互补。


