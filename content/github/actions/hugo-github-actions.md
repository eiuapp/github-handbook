---
date: 2019-06-27T15:15:15+08:00
title: "github actions"
weight: 1140
keywords: 
description: "github actions"
---

hugo 与 github actions 



## 使用 hugo-build-action 解决

- https://github.com/marketplace?utf8=%E2%9C%93&type=actions&query=hugo
- https://github.com/marketplace/actions/hugo-build-action  找到 github 
- https://github.com/peaceiris/actions-hugo 

## 操作

结合一下 https://github.com/khanhicetea/gh-actions-hugo-deploy-gh-pages 提到的 `Create Deploy Key` 进行操作

### Create Deploy Key

Official Documents is [here](https://developer.github.com/v3/guides/managing-deploy-keys/#deploy-keys)

install setup is [here](https://developer.github.com/v3/guides/managing-deploy-keys/#setup-2)

Here is my Steps:

1. Generate deploy key ssh-keygen -t rsa -f hugo -q -N ""
2. Then go to "Settings > Deploy Keys" of repository
3. Add your public key within "Allow write access" option.
4. Copy your private deploy key to GIT_DEPLOY_KEY secret in "Settings > Secrets"

### `.github/workflows/push.yml`

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
      with:
        submodules: true

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

## (可跳过)查找路径

https://github.com/marketplace?utf8=%E2%9C%93&type=actions&query=hugo

https://github.com/marketplace/actions/hugo-gh-pages-action

https://github.com/mattbailey/actions-hugo

https://github.com/mattbailey/

https://github.com/dadlan/dadlan.com

https://github.com/dadlan/dadlan.com/blob/master/.github/workflows/push.yaml


## build前下载themes

```bash
Build 2s
##[error]Docker run failed with exit code 255
Run mattbailey/actions-hugo@v0.57.2
/usr/bin/docker run --name df7dc4ad10577cef04c17b47013079333c4ac_cbd433 --label 0df7dc --workdir /github/workspace --rm -e GITHUB_TOKEN -e INPUT_ARGS -e HOME -e GITHUB_REF -e GITHUB_SHA -e GITHUB_REPOSITORY -e GITHUB_ACTOR -e GITHUB_WORKFLOW -e GITHUB_HEAD_REF -e GITHUB_BASE_REF -e GITHUB_EVENT_NAME -e GITHUB_WORKSPACE -e GITHUB_ACTION -e GITHUB_EVENT_PATH -e RUNNER_OS -e RUNNER_TOOL_CACHE -e RUNNER_TEMP -e RUNNER_WORKSPACE -v "/var/run/docker.sock":"/var/run/docker.sock" -v "/home/runner/work/_temp/_github_home":"/github/home" -v "/home/runner/work/_temp/_github_workflow":"/github/workflow" -v "/home/runner/work/github-handbook/github-handbook":"/github/workspace" 0df7dc:4ad10577cef04c17b47013079333c4ac --gc --minify
Generating site
hugo: /usr/lib/libstdc++.so.6: no version information available (required by hugo)
hugo: /usr/lib/libstdc++.so.6: no version information available (required by hugo)
hugo: /usr/lib/libstdc++.so.6: no version information available (required by hugo)
hugo: /usr/lib/libstdc++.so.6: no version information available (required by hugo)
hugo: /usr/lib/libstdc++.so.6: no version information available (required by hugo)
hugo: /usr/lib/libstdc++.so.6: no version information available (required by hugo)
hugo: /usr/lib/libstdc++.so.6: no version information available (required by hugo)
hugo: /usr/lib/libstdc++.so.6: no version information available (required by hugo)
hugo: /usr/lib/libstdc++.so.6: no version information available (required by hugo)
Error: module "hugo-theme-learn" not found; either add it as a Hugo Module or store it in "/github/workspace/themes".: module does not exist
##[error]Docker run failed with exit code 255
```

如上，提示，`module "hugo-theme-learn" not found`, 说明这个工程依赖 "hugo-theme-learn" 没有下载。

在 .travis.yml file 中是放在了 `script` 中的。

