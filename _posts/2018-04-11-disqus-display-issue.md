---
layout: post
title:  "DISQUS显示问题"
date:   2018-04-11 13:03:00
categories: TECH
tags: Daily DISQUS
---

* content
{:toc}

今早我发现Chrome浏览时评论区显示有背景颜色不协调的问题↓  
![DISQUS](/mdres/posts/2018/discus_display_issue.png)





顺带说一下，这个博客加入了DISQUS评论插件，每一条评论都是在DISQUS平台上管理审核的。
它不同于Wordpress保存在MYSQL数据库里的方式，让评论更加易于管理和日后统计。

正是因为它的小巧简洁让我爱上了它，这也是我选择jekyll的理由之一。
当然这里还要感谢主题原作者[HyG](https://github.com/Gaohaoyang)的分享。

言归正传，这个问题仅仅在Chrome浏览器出现，甚至连古老的IE都能正常显示。于是我琢磨了半天，发现
是我Chrome里面的一个小插件导致了这个问题的产生。这个插件中文名字叫作[高对比度模式](https://chrome.google.com/webstore/detail/high-contrast/djcfdncoelnlbldjfhinnjlhdjlikmph)，
是我用来更改配色方案，使得网页更加易于阅读而安装的，只要我把它禁用了，评论区显示就恢复了正常。
