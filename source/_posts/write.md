---
title: 写博客
date: 2026-02-26 21:35:58
categories: Hexo
tags: 
---
# 发布博客

## 写文章

```bash
# 文章开头可以添加一些元信息（YAML Front Matter）
---
title: 我的第一篇文章
date: 2024-01-01 10:00:00
categories: 技术分享  # 分类
# tags: [Hexo, 教程]
tags:                # 标签（可多个）
  - 随笔
  - 博客
---
# 这里是正文，用Markdown语法写...
```

## 访问本地博客预览

```bash
$ hexo clean
INFO  Validating config
INFO
  ===================================================================
      #####  #    # ##### ##### ###### #####  ###### #      #   #
      #    # #    #   #     #   #      #    # #      #       # #
      #####  #    #   #     #   #####  #    # #####  #        #
      #    # #    #   #     #   #      #####  #      #        #
      #    # #    #   #     #   #      #   #  #      #        #
      #####   ####    #     #   ###### #    # #      ######   #
                            5.5.4
  ===================================================================
INFO  Deleted database.

朱俊梅@Zjm MINGW64 /d/today/myblog (main)
$ hexo s
INFO  Validating config
INFO
  ===================================================================
      #####  #    # ##### ##### ###### #####  ###### #      #   #
      #    # #    #   #     #   #      #    # #      #       # #
      #####  #    #   #     #   #####  #    # #####  #        #
      #    # #    #   #     #   #      #####  #      #        #
      #    # #    #   #     #   #      #   #  #      #        #
      #####   ####    #     #   ###### #    # #      ######   #
                            5.5.4
  ===================================================================
INFO  Start processing
INFO  Hexo is running at http://localhost:4000/ . Press Ctrl+C to stop.
INFO  Bye!
```

## 访问线上博客

```bash
hexo new "文章标题"    # 写新文章
git add .             # 添加
git commit -m "更新"   # 提交
git push              # 推送（自动部署）
```
-----------

常用命令：

```bash
hexo new "postName" #新建文章
hexo new page "pageName" #新建页面
hexo generate #生成静态页面至public目录
hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
hexo deploy #部署到GitHub
hexo help  # 查看帮助
hexo version  #查看Hexo的版本
```
