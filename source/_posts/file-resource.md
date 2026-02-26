---
title: 文章资源文件夹
date: 2026-02-26 19:46:14
tags: 
  - 配置Hexo
  - Hexo
---
# 文章资源文件夹

> 如果我写的文章中有若干图片怎么处理？

> 具体操作：
> ```bash
> # 1.打开 _config.yml，找到 post_asset_folder: false 改成 true，保存
> # 2.创建一篇测试文章
> $ hexo new "build-myblog"
> # 3.编辑文章（文章里插入图片）
> # 4.推送
> git add .
> git commit -m "build hexo"
> git push
> ```
> 等一两分钟，访问博客查看效果。

## 第一步：开启配置

&emsp;&emsp;用记事本打开博客根目录的 _config.yml，找到下面这行配置，将 false 改为 true 。

```yaml
post_asset_folder: true
```

## 第二步：创建文章时自动生成图片文件夹

&emsp;&emsp;以后每次执行 hexo new "文章标题" 创建新文章时，Hexo会自动在 source/_posts 目录下生成两个东西 ：
+ 文章标题\.md（你的文章）
+ 文章标题/（与文章同名的文件夹）——这就是放图片的地方。

{% asset_img show.png %}

## 第三步：存放图片并正确引用

1. 把你文章中用到的所有图片（比如 example.jpg）直接复制到这个同名文件夹里。
2. 在文章中用 Hexo 专属标签 引用图片，而不是普通的 Markdown 语法。

```
{% asset_img example.jpg 这里是图片描述 %}
{% asset_img 你的图片.png 图片描述 %}
{% asset_img 你的图片.png %}
```

> :memo: 为什么不用 ![](文章标题/example.jpg)？因为普通 Markdown 引用在首页、归档页可能显示不出来，而 {% asset_img %} 标签能保证在所有页面正确显示 。

## 四步：推送即生效

&emsp;&emsp;图片和文章都准备好后，正常执行：

```bash
git add .
git commit -m "添加新文章及图片"
git push
```

&emsp;&emsp;GitHub Actions 会自动构建，图片就会出现在你的博客上。

## 博客在图片不显现

可能情况：
1. 文件路径不对
2. 文件名大小写不一致
3. 文件没被 Git 跟踪

排查流程：

第一步：确认文件位置

&emsp;&emsp;假设你的文章叫 我的文章.md，那么图片必须放在：

```bash
D:\today\myblog\source\_posts\我的文章\你的图片.png
```

检查方法：（我用的是Git）在博客根目录打开命令行，执行：

```bash
朱俊梅@Zjm MINGW64 /d/today/myblog (main)
$ dir source/_posts/build-myblog/*.png
source/_posts/build-myblog/img01.png
source/_posts/build-myblog/img02.png
source/_posts/build-myblog/img03.png
source/_posts/build-myblog/img04.png
source/_posts/build-myblog/img05.png
source/_posts/build-myblog/img06.png
source/_posts/build-myblog/img07.png
source/_posts/build-myblog/img08.png
source/_posts/build-myblog/img09.png
source/_posts/build-myblog/img10.png
source/_posts/build-myblog/img11.png
source/_posts/build-myblog/img12.png
source/_posts/build-myblog/img13.png

朱俊梅@Zjm MINGW64 /d/today/myblog (main)
$
```

&emsp;&emsp;能列出文件，说明位置正确。

第二步：确认文章里的引用

&emsp;&emsp;在文章里写：

```
{% asset_img 你的图片.png 图片描述 %}
```

关键点：
+ 直接写文件名，不要加文件夹路径
+ 文件名要完全一致（包括大小写，比如 Image.PNG 和 image.png 是不同的）

第三步：确认文件被 Git 跟踪

&emsp;&emsp;在博客根目录执行：

```bash
git status
```

&emsp;&emsp;看看你的图片文件是不是显示为 untracked（未跟踪）或 modified（已修改）。

&emsp;&emsp;如果是 untracked，需要：

```bash
git add source/_posts/你的文章/你的图片.png
git commit -m "添加图片"
git push
```

第四步：本地预览验证

```bash
hexo clean
hexo s
```

&emsp;&emsp;访问http\://localhost:4000，看看图片在本地能不能显示。