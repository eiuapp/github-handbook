---
date: 2019-06-27T15:15:15+08:00
title: "github actions build stage error"
weight: 1141
keywords: 
description: "github actions"
---

github actions build stage error

## hugo-gh-pages-action 会报错

### cloning 后报错

``` 
Building sites … WARN 2019/09/09 08:14:01 In the next Hugo version (0.58.0) we will change how $home.Pages behaves. If you want to list all regular pages, replace .Pages or .Data.Pages with .Site.RegularPages in your home page template.
WARN 2019/09/09 08:14:01 .File.UniqueID on zero object. Wrap it in if or with: {{ with .File }}{{ .UniqueID }}{{ end }}

                   | EN  
+------------------+----+
   Pages            | 23  
  Paginator pages  |  0  
  Non-page files   |  0  
  Static files     | 80  
  Processed images |  0  
  Aliases          |  0  
  Sitemaps         |  1  
  Cleaned          |  0  

Total in 58 ms
Setting up git
cloning
error: unknown option `---END'
usage: git clone [<options>] [--] <repo> [<dir>]

    -v, --verbose         be more verbose
    -q, --quiet           be more quiet
```

一般情况，正确时，如下：

```
Setting up git
cloning
Cloning into '/tmp/gh-pages'...
copying
'public/404.html' -> '/tmp/gh-pages/404.html'
'public/CNAME' -> '/tmp/gh-pages/CNAME'
```

然后，对作者提了个 [issue](https://github.com/mattbailey/actions-hugo/issues/1)

然后，明白了一个思路：

1. 在 `.github/workflows/push.yaml` 中指明了 `uses: mattbailey/actions-hugo@v0.57.2`
2. 那么 actions 内部 会 运行`https://github.com/mattbailey/actions-hugo` 这里面的 `[Dockerfile](https://github.com/mattbailey/actions-hugo/blob/master/Dockerfile)`
3. `Dockerfile` 中的 `entrypoint.sh` 运行到 第15行 打印了上面的日志中的 `cloning`
4. `Dockerfile` 中的 `entrypoint.sh` 运行到 第16行 报错了。

那报错的思路，就是2个：

1. 仓库本身，没有 `gh-pages` 分支。这个我这里已有此分支。
2. Did you add a DEPLOY_TOKEN_MATTBAILEY to your repo's secrets?

但是，1，2，这2个可能性，我都可以排除掉的。

现在我只能看一看，有没有其它的`actions-hugo`了

## 使用 hugo-build-action 解决

- https://github.com/marketplace?utf8=%E2%9C%93&type=actions&query=hugo
- https://github.com/marketplace/actions/hugo-build-action  找到 github 
- https://github.com/peaceiris/actions-hugo 

直接把这README.md 中的 `.github/workflows/gh-pages.yml` 示例弄过来了，替换成 `.github/workflows/push.yml`

具体如下：

```yml
name: github pages

on:
  push:
    branches:
    - master

jobs:
  build-deploy:
    runs-on: ubuntu-18.04
    steps:
    - uses: actions/checkout@master

    - name: build
      uses: peaceiris/actions-hugo@v0.58.1
      with:
        args: --gc --minify --cleanDestinationDir

    - name: deploy
      uses: peaceiris/actions-gh-pages@v2.2.0
      env:
        ACTIONS_DEPLOY_KEY: ${{ secrets.DEPLOY_TOKEN_MATTBAILEY }}
        PUBLISH_BRANCH: gh-pages
        PUBLISH_DIR: ./public
```

成功了。

**另外** 这个 https://github.com/peaceiris/actions-gh-pages 里面有，包括

- Static Site Generators with Node.js！
next.js, nuxt.js, gatsby, hexo, gitbook, vuepress, react-static, gridsome, etc.

- Static Site Generators with Python！
pelican, MkDocs, sphinx, etc.