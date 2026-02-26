---
title: errs
date: 2026-02-26 20:20:59
tags: 
  - err
  - Hexo
---
# 遇到的error

{% asset_img err.png %}

{% asset_img failpage.png %}

## themes/butterfly错误

> Error: fatal: No url found for submodule path 'themes/butterfly' in .gitmodules

&emsp;&emsp;***What？*** 这个错误的意思是：GitHub Actions 在构建环境里把你的 butterfly 主题文件夹当成了一个需要单独拉取的“子模块”，但它又找不到这个子模块的下载地址，所以构建就卡住了。

&emsp;&emsp;***Why？*** themes/butterfly 文件夹当初可能是通过 git clone 下载的，它内部自带了一个 .git 文件夹 。当你把这个带有 .git 的文件夹提交到自己的博客仓库时，Git 就会把它识别为一个“子模块”，而不是你仓库本身的一部分。在云端自动构建时，Actions 尝试去拉取这个子模块，但找不到它的配置信息，于是就报错了 。

&emsp;&emsp;***Solution。*** 让主题文件夹“归顺”你的仓库

&emsp;&emsp;要解决这个问题，需要做的是移除主题文件夹中的 .git 痕迹，让它彻底变成你博客仓库的一个普通文件夹 。请在你的本地博客根目录（D:\today\myblog）下，按顺序执行以下命令：

第一步：进入主题目录并删除 Git 痕迹

```bash
cd themes/butterfly
# 删除主题文件夹自带的 .git 文件夹（Linux/macOS 命令）
rm -rf .git
# 如果你用的是 Windows 命令提示符，可以用下面这行代替：
# rmdir /s .git
```

&emsp;&emsp;这一步是核心，rm -rf .git 就是删除主题文件夹里的 Git 独立记录。

第二步：回到博客根目录，从 Git 中移除对主题的错误跟踪

```bash
cd ../../
# 从 Git 的缓存中移除对 themes/butterfly 的“子模块”记录
git rm -r --cached themes/butterfly
```

第三步：重新将主题文件夹添加到 Git 跟踪中

```bash
git add themes/butterfly/
```

&emsp;&emsp;现在，themes/butterfly 会被当作一个普通的文件夹来添加。

第四步：提交并推送更改

```bash
git commit -m "fix: 将 butterfly 主题转换为普通文件夹，移除子模块跟踪"
git push
```

&emsp;&emsp;***原理：*** 这个流程和搜索结果中提到的解决方案完全一致 。简单来说，你通过 rm -rf .git 这个动作，把主题从“有独立身份的客卿”变成了“彻底归顺的你自己的文件”。之后再执行 git rm --cached 和 git add，就是清除 Git 旧的错误记忆，并建立新的正确关系。

&emsp;&emsp;***预期结果：*** 推送之后，再去 GitHub 仓库的 Actions 页面看看，这次关于子模块的错误应该就消失了。构建顺利通过，可正常访问博客，但我在git push时又遇到第二个err。

## GitHub仓库的 Actions 标签页是红色叉号

> 查看日志
> error: unable to rewind rpc post data - try increasing http.postBuffer
> error: RPC failed; curl 65 Recv failure: Connection was reset
> fatal: the remote end hung up unexpectedly
> Everything up-to-date

|错误信息	|含义|
|---|---------|
|error: unable to rewind rpc post data - try increasing http.postBuffer|	Git 的 HTTP 缓冲区大小不足|
|Recv failure: Connection was reset	|网络连接被中断|
|fatal: the remote end hung up unexpectedly|	服务器端连接意外断开|

&emsp;&emsp;***Why？*** 综合来看，这是因为推送数据量（2.31 MiB 不算大）在传输过程中，遇到了网络不稳定，或者 Git 默认的 HTTP 缓冲区（默认 1MB）不够大导致的。这次的推送数据量是 2.31 MiB，超过了 Git 默认的 HTTP 缓冲区大小（1 MiB）。加上网络波动，就触发了连接重置。增加缓冲区后，Git 就能更从容地处理这次上传了。

&emsp;&emsp;***Solution。*** 增加 Git 的 HTTP 缓冲区大小（最推荐，成功率最高）。在命令行中执行以下命令，将缓冲区扩大到 500MB：

```bash
git config --global http.postBuffer 524288000
```

然后重新执行：

```bash
git push
```

&emsp;&emsp;git push时报错。

> fatal: unable to access 'https:\//github.com/Zjm110/Zjm110.github.io.git/': Recv failure: Connection was reset

&emsp;&emsp;***Solution。*** 修改本地仓库远程地址。在博客根目录执行：

```bash
git remote set-url origin git@github.com:Zjm110/Zjm110.github.io.git
```

再次尝试推送：

```bash
$ git push
Everything up-to-date
```

Everything up-to-date提示说明：
+ 本地仓库和远程仓库已经完全一致了，没有任何需要推送的新内容。
+ 之前的网络问题已经解决了。


|状态	|含义|
|-------|------|
|Everything up-to-date	|本地没有要推送的新提交，远程已经是最新|
|Git 操作成功|	没有报错，说明连接正常|

&emsp;&emsp;但GitHub仓库的 Actions 标签页还是红色叉号，查看日志sh: 1: hexo: Permission denied。

------------

&emsp;&emsp;***What？*** 构建环境里 hexo 命令的权限问题。

&emsp;&emsp;***Why？*** 在 GitHub Actions 的云端 Ubuntu 环境中，npx hexo generate 执行时提示权限不足，通常是因为：node_modules 里的 .bin/hexo 可执行文件权限不对或者依赖没有完整安装。

&emsp;&emsp;***Solution。*** 修改 .github/workflows/pages.yml。打开博客根目录下的 .github/workflows/pages.yml 文件，用以下完整配置替换全部内容：

```bash
name: Pages

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Use Node.js 20
        uses: actions/setup-node@v4
        with:
          node-version: "20"
          
      - name: Cache NPM dependencies
        uses: actions/cache@v4
        with:
          path: node_modules
          key: ${{ runner.OS }}-npm-cache-${{ hashFiles('**/package-lock.json') }}
          restore-keys: |
            ${{ runner.OS }}-npm-cache-
            
      - name: Install Dependencies
        run: |
          npm install
          # 关键修复：手动修复 hexo 可执行文件权限
          chmod +x node_modules/.bin/hexo
          
      - name: Build
        run: |
          # 验证 hexo 是否存在且可执行
          ls -la node_modules/.bin/hexo || true
          # 执行生成
          npx hexo generate
          
      - name: Upload Pages artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public

  deploy:
    needs: build
    permissions:
      pages: write
      id-token: write
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

关键修复点说明
|改动	|作用|
|--------|-------|
|chmod +x node_modules/.bin/hexo|	在安装依赖后手动赋予 hexo 可执行权限|
|ls -la node_modules/.bin/hexo \|\| true	|调试用，查看文件权限（不会中断流程）|
|保留 npx hexo generate	|用 npx 确保从本地 node_modules 调用|

操作步骤：
1. 打开 .github/workflows/pages.yml
2. 删除所有旧内容，粘贴上面的新配置
3. 保存文件
4. 在博客根目录执行推送：

```bash
git add .
git commit -m "fix: 修复 hexo 权限问题，优化 Actions 配置"
git push
```

&emsp;&emsp;去 GitHub 仓库的 Actions 页面，观察新的工作流运行。最终成功！