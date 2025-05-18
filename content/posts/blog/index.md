+++
date = '2025-05-18T11:24:36+08:00'
draft = false
title = 'Blog 配置'
tags = ['Blog']
categories = ['Misc']
+++


## 2. hugo 和 theme 关系
hugo 提供渲染功能，theme 功能是进行更好的展示（？）。配置 blog 样式时先看 theme 有无提供说明，然后再看 hugo reference  
以 taxonomies 为例，使用 hugo 原生语法定制有哪些分类，但是展示效果则取决于 theme

## 2. blog 细节
### 2.1. home page
整体简洁，上面是基本介绍，例如 zhihu link，下面是一行一行的文章概述  
右上角是 archive 和 tag 的链接。
### 2.2. 文章管理
source content 和渲染后路径是对应的，默认一般是：
```txt
.
└── content
    └── about
    |   └── index.md  // <- https://example.org/about/
    ├── posts
    |   ├── firstpost.md   // <- https://example.org/posts/firstpost/
    |   ├── happy
    |   |   └── ness.md  // <- https://example.org/posts/happy/ness/
    |   └── secondpost.md  // <- https://example.org/posts/secondpost/
    └── quote
        ├── first.md       // <- https://example.org/quote/first/
        └── second.md      // <- https://example.org/quote/second/
```

**taxonomies**  
hugo 默认支持两种分类，即 tag 和 category：
```toml
[taxonomies]
  category = 'categories'
  tag = 'tags'
```
举个例子：
```text
+++
categories = ['c1','c2']
tags = ['t1','t2']
+++
```

### 2.3. 换行和段落
markdown 回车默认是不换行的，需要显示在行尾打两个空格。如果需要新段落（即隔的更开），则还要输入回车。  
举个例子：
```text
这是一行，
这里没有换行
```
这是一行，
这里没有换行。  

----

```text
这是一行，<空格><空格>
这是新的一行
```
这是一行，  
这是新的一行。  

----

```text
这是一行，<空格><空格>
 <回车>
这是新的段落
```
这是一行，  

这是新的段落
