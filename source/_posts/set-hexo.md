---
title: 配置Hexo
date: 2026-03-01 13:28:54
categories: Hexo
tags: 
---
# 配置Hexo

Hexo 有两个重要的配置文件：

|配置文件|	位置|	作用|官网|
|---------|-----------|---------|-------|
|站点配置文件	|博客根目录下的 _config.yml|	网站全局配置（网站标题、作者、主题等）|[hexo官网](https://hexo.io/zh-cn/docs/configuration)|
|主题配置文件	|themes/你的主题/_config.yml|	当前主题的样式、功能配置|[butterfly官网](https://butterfly.js.org/posts/4aa8abbe/)|

修改配置文件的正确姿势：
+ 随时可以修改
    用任何文本编辑器（记事本、VS Code 等）打开 .yml 文件，直接修改即可。

+ 修改后需要重启生效

```bash
# 在博客根目录（D：\today\myblog）下执行：
hexo clean   # 清理缓存（推荐，但不是必须）
hexo s       # 重新启动本地服务器
```

修改配置的注意事项

|注意事项|	说明	|示例|
|-------|----------|----------|
|格式要正确|	YAML 文件对空格敏感，冒号后必须有一个空格|	title： 我的博客 title：我的博客|
|缩进要对齐	|不要用 Tab，用两个空格缩进	|同一层级的配置要对齐|
|中文不乱码|	保存时选择 UTF-8 编码|	否则中文会显示乱码|
|备份好原文件	|修改前可以复制一份备份|_config.yml.bak|

```bash
# 1. 在命令行窗口按 Ctrl + C
# 看到提示：终止批处理操作吗（Y/N）？ Y

# 2. 清理缓存
D：\today\myblog> hexo clean
INFO  Deleted database.
INFO  Deleted public folder.

# 3. 重新启动
D：\today\myblog> hexo s
INFO  Start processing
INFO  Hexo is running at http：//localhost：4000. Press Ctrl+C to stop.

# 4. 打开浏览器刷新 http：//localhost：4000
```

> 小技巧：不用每次都重启
> &emsp;&emsp;如果你只是修改了文章内容（Markdown 文件），Hexo 支持热加载，保存后自动刷新页面，不需要重启服务器。但如果你修改的是配置文件（_config.yml），必须重启才能生效。

如果修改后没看到变化，检查这几点：
+ 浏览器缓存：试试无痕模式，或者按 Ctrl + F5 强制刷新
+ 配置文件格式：确认 YAML 格式正确（冒号后要有空格）
+ 是否在根目录：确认当前路径是 D：\today\myblog 而不是其他目录

## 配置根目录下的_config.yml

<img src="./img/imgP3/img01.png" width="800">

```yml
# Site
title: My Code World
subtitle: '记录生活点滴'
description: '这是一个用Hexo搭建的博客'
keywords:
author: Zhu Junmei
language: zh-CN
timezone: 'Asia/Shanghai'
```

保存 _config.yml 文件，重启 Hexo 服务器。

```bash
# 清除缓存
D:\today\myblog>hexo clean
INFO  Validating config
INFO  Deleted database.

# 重新启动
D:\today\myblog>hexo s
INFO  Validating config
INFO  Start processing
INFO  Hexo is running at http://localhost:4000/ . Press Ctrl+C to stop.

```

&emsp;&emsp;***刷新浏览器*** 看到 INFO Hexo is running at http：//localhost：4000. 的提示后，回到浏览器，刷新页面（按 F5 或刷新按钮），就能看到修改后的效果了。

<img src="./img/imgP3/img02.png" width="800">

```bash
# 清除缓存
D:\today\myblog>hexo clean
INFO  Validating config
INFO  Deleted database.

# 重新启动
D:\today\myblog>hexo s
INFO  Validating config
INFO  Start processing
INFO  Hexo is running at http://localhost:4000/ . Press Ctrl+C to stop.
INFO  Catch you later

D:\today\myblog>
```

------------------

修改根目录下的_config.yml：

```yml
# Site
title: My Code World
subtitle: '记录生活点滴'
description: '这是一个用Hexo搭建的博客'
keywords:
author: zhujunmei
language: zh-CN
timezone: 'Asia/Shanghai'
```

```bash
D:\today\myblog>hexo clean
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

D:\today\myblog>hexo g
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
INFO  Files loaded in 1.71 s
INFO  Generated: link/index.html
INFO  Generated: categories/index.html
INFO  Generated: archives/index.html
INFO  Generated: tags/index.html
INFO  Generated: archives/2026/index.html
INFO  Generated: archives/2026/02/index.html
INFO  Generated: index.html
INFO  Generated: img/favicon.ico
INFO  Generated: js/main.js
INFO  Generated: css/index.css
INFO  Generated: css/var.css
INFO  Generated: js/search/local-search.js
INFO  Generated: img/friend_404.gif
INFO  Generated: img/error-page.png
INFO  Generated: img/404.jpg
INFO  Generated: js/utils.js
INFO  Generated: js/tw_cn.js
INFO  Generated: js/search/algolia.js
INFO  Generated: 2026/02/25/hello-world/index.html
INFO  Generated: img/butterfly-icon.png
INFO  20 files generated in 471 ms

D:\today\myblog>hexo s
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
INFO  Good bye

D:\today\myblog>
```

&emsp;&emsp;改完配置重启后，你的新网站标题就应该显示出来了～

## Butterfly 主题

&emsp;&emsp;默认主题比较简洁，Hexo有非常丰富的主题库 ，推荐几个比较受欢迎的 ：
+ NexT：优雅的双栏布局，功能丰富，技术博客首选 
+ Butterfly：现代化设计，支持暗黑模式，颜值很高 
+ Fluid：简洁清爽的Material Design风格 
+ Anzhiyu：可自定义程度高，兼容很多插件

-------

以安装NexT主题为例 ：

```bash
git clone https://github.com/theme-next/hexo-theme-next themes/next
```

&emsp;&emsp;然后打开博客根目录下的 _config.yml 文件（这是站点配置文件），找到 theme 字段，修改为：

```bash
theme: next
```

&emsp;&emsp;保存后，再次运行 hexo s --debug，就能看到新主题了。

### 第一步：下载 Butterfly 主题

&emsp;&emsp;在博客根目录（D：\today\myblog）下，执行以下命令：

```bash
git clone -b master https://github.com/jerryc127/hexo-theme-butterfly.git themes/butterfly
```

&emsp;&emsp;国内加速：如果 GitHub 下载慢，可以用 Gitee 镜像：

```bash
git clone -b master https://gitee.com/immyw/hexo-theme-butterfly.git themes/butterfly
```

### 第二步：安装必要插件

&emsp;&emsp;Butterfly 需要 pug 和 stylus 渲染器

```bash
npm install hexo-renderer-pug hexo-renderer-stylus --save
```

这个命令会安装两个关键组件：
+ hexo-renderer-pug：用于解析Pug模板（就是你看到的那段代码）
+ hexo-renderer-stylus：用于解析Stylus样式

意思是：
+ 安装 Pug 模板引擎 的渲染器（处理你看到的那些 layout.pug 文件）
+ 安装 Stylus 样式语言 的渲染器（处理主题的样式文件）

两者都是为了让 Hexo 能“读懂”Butterfly 主题的源码。

命令中的 stylus 是指 Stylus 样式语言。

|名词|	含义	|作用|
|------|---------|---------|
|Stylus|	一种 CSS 预处理器（类似 Sass、Less）|	用更简洁的语法写样式，最终会被转换成普通的 CSS 文件|
|hexo-renderer-stylus	|Hexo 的 Stylus 渲染插件	|负责把 .styl 文件转换成浏览器能识别的 .css 文件|

------

```bash
D:\today\myblog>npm install hexo-renderer-pug hexo-renderer-stylus --save

added 34 packages in 6s

31 packages are looking for funding
  run `npm fund` for details

D:\today\myblog>npm list hexo-renderer-pug hexo-renderer-stylus
hexo-site@0.0.0 D:\today\myblog
+-- hexo-renderer-pug@3.0.0
`-- hexo-renderer-stylus@3.0.1


D:\today\myblog>
```

> :bulb: 小知识：为什么 --save 很重要？
> &emsp;&emsp;--save 参数的作用是把插件信息记录到 package.json 文件里。这样以后换电脑或者重新安装时，只需要执行 npm install，就能自动安装所有依赖。

### 第三步：启用主题

&emsp;&emsp;用记事本打开博客根目录的 _config.yml，找到 theme 字段，修改为 

```bash
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
# theme: landscape
theme: butterfly
```

### 第四步：创建主题配置文件（重要！）

&emsp;&emsp;这是 Butterfly 推荐的最佳实践，方便以后升级主题

```bash
# 把主题自带的配置文件复制到根目录
copy themes\butterfly\_config.yml _config.butterfly.yml
```

> :memo: 为什么这样做？以后所有主题修改都在 _config.butterfly.yml 里进行，即使更新主题文件，你的个性化配置也不会丢失

### 第五步：创建必要的页面

&emsp;&emsp;Butterfly 需要一些特殊页面才能正常工作。

常见页面的 type 设置一览

|页面	|需要添加的 type	|说明|
|----|---------|------|-------|
|标签页|	type: "tags"	|显示标签云|
|分类页	|type: "categories"	|显示分类列表|
|友链页|	type: "link"	|显示友情链接|
|音乐页|	type: "music"|	显示音乐播放器或歌单|
|相册页|	type: "gallery"	|显示图片集（如果有）|
|关于页	|不需要 type|	普通页面，可自定义内容|

```bash
# 创建标签页
hexo new page tags

# 创建分类页
hexo new page categories

# 创建友情链接页（可选）
hexo new page link
```

&emsp;&emsp;然后分别编辑这些页面，在 Front-matter 中添加 type 字段：

source/tags/index.md：

```bash
---
title: 标签
date: 2026-02-25 15:01:05
type: "tags"
---
```

source/categories/index.md：

```bash
---
title: 分类
date: 2026-02-25 15:01:43
type: "categories"
---
```

source/link/index.md：

```bash
---
title: link
date: 2026-02-25 15:02:11
type: "link"
---
```

source/gallery/index.md：

```bash
---
title: gallery
date: 2026-02-25 18:39:25
type: "gallery"
---
```

source/music/index.md：

```bash
---
title: music
date: 2026-02-25 18:22:57
type: "music"
---
```

source/movies/index.md：

```bash
---
title: movies
date: 2026-02-25 18:23:12
type: "movies"
---
```

source/about/index.md：

```bash
---
title: about
date: 2026-02-25 18:23:26
---
```

### 第六步：启动看看效果

```bash
hexo clean  # 清理缓存
hexo s      # 启动本地服务器
```

&emsp;&emsp;访问 http：//localhost：4000，你应该就能看到漂亮的 Butterfly 主题了！

----------- 

```bash
D:\today\myblog>hexo clean
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

D:\today\myblog>hexo s
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

## 基础配置（可选，但推荐）

&emsp;&emsp;打开 _config.butterfly.yml，可以修改这些基础设置。

### 网站基本信息

```yml
# 导航菜单
nav:
  # Navigation bar logo image
  logo:
  display_title: true
  display_post_title: true
  # Whether to fix navigation bar
  fixed: false

menu:
  首页: / || fas fa-home                    # 主菜单：路径 || 图标
  时间轴: /archives/ || fas fa-archive      # 修正路径
  标签: /tags/ || fas fa-tags
  分类: /categories/ || fas fa-folder-open
  清单||fas fa-list:                         # 子菜单父项：名称||图标:（无空格）
    照片: /gallery/ || fas fa-images         # 子菜单：名称: 路径 || 图标
    音乐: /music/ || fas fa-music
    电影: /movies/ || fas fa-video
  友链: /link/ || fas fa-link
  关于: /about/ || fas fa-heart
```

特别注意：
+ /music/ 前后的斜杠不能少
+ || 前后有空格
+ fas fa-music 是音乐图标代码

### 头像和封面图

```yml
# 头像
avatar:
  img: /img/avatar.jpg  # 图片放在 themes/butterfly/source/img/ 下
  effect: false

# 首页封面图
index_img: /img/background.jpg

# 默认顶部图（当页面没设置时使用）
default_top_img: /img/default.jpg
```

----

```yml
# Image Settings
# --------------------------------------

favicon: /img/favicon.png

avatar:
  img: /img/avatar.jpg
  effect: false

# Disable all banner images
disable_top_img: false

# If the banner of page not setting, it will show the default_top_img
default_top_img: /img/default.jpg

# The banner image of index page
index_img: /img/background.jpg
```

### 副标题打字效果

```yml
subtitle:
  enable: true
  effect: true  # 开启打字效果
  loop: true    # 循环播放
  sub:
    - 记录生活点滴
    - 分享技术心得
    - 欢迎来到我的博客
```

--------

```yml
# The subtitle on homepage
subtitle:
  enable: true
  # Typewriter Effect
  effect: true
  # Customize typed.js
  # https://github.com/mattboldt/typed.js/#customization
  typed_option:
  # Source - Call the third-party service API (Chinese only)
  # It will show the source first, then show the content of sub
  # Choose: false/1/2/3
  # false - disable the function
  # 1 - hitokoto.cn
  # 2 - https://api.aa1.cn/doc/yiyan.html
  # 3 - jinrishici.com
  source: false
  loop: true
  # If you close the typewriter effect, the subtitle will only show the first line of sub
  sub: ['记录生活点滴','分享技术心得','欢迎来到我的博客']
  sub: 
    - 记录生活点滴
    - 分享技术心得
    - 欢迎来到我的博客
```

### 图片大图查看功能

```yml
# 推荐启用 fancybox
fancybox: true
medium_zoom: false
```

按 Ctrl + F 打开搜索框分别搜索fancybox和medium_zoom
去掉前面的#号修改如下：

```yml
# egjs_infinitegrid:
fancybox: true

medium_zoom: false
```

|选项|	效果|	适用场景|
|------|---------|----|
|fancybox： true|	点击图片弹出灯箱，可左右滑动查看多张图片|	图集、多图片文章|
|medium_zoom： true|	点击图片原地放大，简洁轻量|	单张配图文章|

一般推荐用 fancybox，功能更丰富，视觉效果也更好～

### 添加真正的音乐播放器

&emsp;&emsp;如果只是设置 type: "music"，你得到的可能只是一个空白页面或者简单的文字介绍。如果你想要一个真正的音乐播放器（比如网易云歌单），还需要额外配置：

```yml
# Inject the css and script (aplayer/meting)
aplayerInject:
  enable: true
  per_page: true
```

后在 inject 区域添加具体的歌单配置。

-----------


```yml
aside:
  enable: true
  hide: false
  # Show the button to hide the aside in bottom right button
  button: true
  mobile: true
  # Position: left / right
  position: right
  display:
    archive: true
    tag: true
    category: true
  card_author:  # 作者卡片
    enable: true
    name: zhujunmei
    description: 记录生活，分享技术
    ## 社交图标
    button:
      enable: true
      icon: fab fa-github
      text: Follow Me
      link: https://github.com/Zjm110

social:
  # fab fa-gitee: https://gitee.com/Zjm_110 || Gitee || '#c71d23'
  fab fa-github: https://github.com/Zjm110 || Github || '#24292e'
```

启动看看效果

```bash
D:\today\myblog>hexo clean
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
INFO  Deleted public folder.

D:\today\myblog>hexo s
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
INFO  See you again

D:\today\myblog>
```


