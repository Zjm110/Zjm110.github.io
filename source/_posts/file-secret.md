---
title: 文章加密
date: 2026-03-01 12:43:07
tags: Hexo
password: 123456
abstract: 私密笔记
---
# 文章加密

技术栈：基于Hexo + GitHub Pages
方案：安装文章加密插件（最推荐，免费且简单）

&emsp;&emsp;这是专为 Hexo 设计的加密插件，可以直接给你的某篇文章加上密码锁 。

|方案|  实现方式  |优点|缺点|成本|
|-----|--------------|-----|-------|------|
|文章加密插件|安装 hexo-blog-encrypt 插件|最简单，无需换平台，无缝集成|	加密强度一般，懂技术的人可能绕过|免费|

## 第一步：安装插件

&emsp;&emsp;在博客根目录 D:\today\myblog 打开命令行，执行：

```bash
$ npm install hexo-blog-encrypt --save

added 1 package in 4s

37 packages are looking for funding
  run `npm fund` for details
```

检查插件是否成功安装
```bash
# 在博客根目录打开命令，执行：
$ npm list hexo-blog-encrypt
hexo-site@0.0.0 D:\today\myblog
└── hexo-blog-encrypt@3.1.9
```

&emsp;&emsp;如果能看到 hexo-blog-encrypt@x.x.x 的版本信息，说明安装成功。

## 第二步：配置插件（可选）

打开根目录的 _config.yml，在最底部添加 ：

```yml
# 加密插件配置（参考）
encrypt:
  enable: true
  default_password: 你的默认密码  # 可选，所有加密文章可用此密码
```

----------

```yml
# 加密插件配置（实际配置）
encrypt:
  enable: true
  # 不要设置 default_password！让每篇文章自己决定密码
  abstract: 有东西被加密了, 请输入密码查看.
  message: 您好, 这里需要密码.
```

## 第三步：加密特定文章

&emsp;&emsp;在你想加密的文章 Front-matter 里添加 password 字段 ：

```
---
title: 我的私密日记
date: 2024-01-01
password: 123456  # 设置访问密码
abstract: 这是一篇加密文章，请输入密码查看  # 密码框上方的提示语
message: 请输入密码  # 密码框的提示信息
---
这里是只有输入密码才能看到的正文内容...
```

---------

```
<!-- 实际配置 -->
title: 文章加密
date: 2026-03-01 12:43:07
tags: Hexo
password: 123456
abstract: 私密笔记
```

## 第四步：重新生成并推送

```bash
hexo clean && hexo g
git add .
git commit -m "添加加密文章"
git push
```

&emsp;&emsp;部署后，访问这篇文章时会看到一个密码输入框，只有输入正确密码才能看到内容 。



两个密码各自的作用

|密码类型|	配置位置|	作用|
|------|---------|-----------|
|default_password|	根目录 _config.yml 中	|全局默认密码，一般不推荐设置，设置了会让所有文章都用同一个密码 |
|password: 123456|	单篇文章的 Front-matter 中|	单篇文章的密码，优先级最高，只对该篇文章生效 |

## 密码优先级规则

&emsp;&emsp;根据插件的官方文档，密码的优先级是明确的 ：

```
文章头的 password > 根目录 _config.yml 中配置的 tags 密码 > 默认配置
```

简单来说：
+ 文章头的 password 优先级最高，会覆盖所有全局设置
+ 根目录的配置只是“后备方案”，只有在文章没设密码时才会生效

{% asset_img img01.png %}
{% asset_img img02.png %}