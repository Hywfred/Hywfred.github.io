---
title: butterfly主题页面
tags:
  - butterfly
  - hexo
categories:
  - 技术分享
  - hexo
  - butterfly
keywords: "hexo,theme,butterfly,主题"
description: butterfly 博客主题的主题页面配置。包括page页面和post页面的metadata配置。
top_img: 'https://s1.ax1x.com/2020/09/03/wiGagU.jpg'
comments: true
toc_number: false
cover: 'https://s1.ax1x.com/2020/09/03/wiGagU.jpg'
abbrlink: 2e99775b
date: 2020-09-03 10:28:18
---


## 1. `Front-matter` 
档案最上方以 `---` 分隔的区域，用于指定个别档案的配置。  
> ---
> title: 主题页面 #[必需] 页面标题
> date: #[必需] 页面创建日期
> type: Hexo #[必需] 标签、分类和友情链接三个页面需要配置
> updated: #[可选] 页面更新日期
> comments: #[可选] 显示页面评论模块，默认为 true
> description: #[可选] 页面描述
> keywords: #[可选] 页面关键字
> top_img: #[可选] 页面顶部图片
> mathjax: #[可选] 显示 mathjax，当设置 mathjax 的 per_page: false 时才需要配置，默认为 false
> katex: #[可选] 同上
> aside: #[可选] 显示侧边栏，默认为 true
> aplayer: #[可选] 在需要的页面加载 aplayer 的 js 和 css，参考`音乐`配置
> highlight_shrink: #[可选] 配置代码框是否展开，默认为设置中 highlight_shrink 的配置
> ---


## 2. `Post Front-matter`
档案最上方以 `---` 分隔的区域，用于指定具体博客的配置。
> ---
> title: #【必需】文章標題
> date: #【必需】文章創建日期
> updated: #【可選】文章更新日期
> tags: #【可選】文章標籤
> categories: #【可選】文章分類
> keywords: #【可選】文章關鍵字
> description: #【可選】文章描述
> top_img: #【可選】文章頂部圖片
> comments: #【可選】顯示文章評論模塊 (默認 true)
> cover: #【可選】文章縮略圖 (如果沒有設置 top_img, 文章頁頂部將顯示縮略圖，可設為 false / 圖片地址 / 留空)
> toc: #【可選】顯示文章 TOC (默認為設置中 toc 的 enable 配置)
> toc_number: #【可選】顯示 toc_number (默認為設置中 toc 的 number 配置)
> auto_open: #【可選】是否自動打開 TOC (默認為設置中 toc 的 auto_open 配置)
> copyright: #【可選】顯示文章版權模塊 (默認為設置中 post_copyright 的 enable 配置)
> copyright_author: #【可選】文章版權模塊的文章作者
> copyright_author_href: #【可選】文章版權模塊的文章作者鏈接
> copyright_url: #【可選】文章版權模塊的文章連結鏈接
> copyright_info: #【可選】文章版權模塊的版權聲明文字
> mathjax: #【可選】顯示 mathjax (當設置 mathjax 的 per_page: false 時，才需要配置，默認 false)
> katex: #【可選】顯示 katex (當設置 katex 的 per_page: false 時，才需要配置，默認 false)
> aplayer: #【可選】在需要的頁面加載 aplayer 的 js 和 css, 請參考文章下面的音樂 配置
> highlight_shrink: #【可選】配置代碼框是否展開 (true/false)(默認為設置中 highlight_shrink 的配置)
> ---

## 3. 关于 `butterfly` 详细的配置请移步 [butterfly 配置](https://demo.jerryc.me/posts/4aa8abbe)