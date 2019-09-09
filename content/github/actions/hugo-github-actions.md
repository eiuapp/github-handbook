---
date: 2019-06-27T15:15:15+08:00
title: "github actions"
weight: 1140
keywords: 
description: "github actions"
---

hugo 与 github actions 

## 查找路径

https://github.com/marketplace?utf8=%E2%9C%93&type=actions&query=hugo

https://github.com/marketplace/actions/hugo-gh-pages-action

https://github.com/mattbailey/actions-hugo

https://github.com/mattbailey/

https://github.com/dadlan/dadlan.com

https://github.com/dadlan/dadlan.com/blob/master/.github/workflows/push.yaml

## 操作

结合一下 https://github.com/khanhicetea/gh-actions-hugo-deploy-gh-pages 提到的 `Create Deploy Key` 进行操作

### Create Deploy Key

1. Generate deploy key ssh-keygen -t rsa -f hugo -q -N ""
2. Then go to "Settings > Deploy Keys" of repository
3. Add your public key within "Allow write access" option.
4. Copy your private deploy key to GIT_DEPLOY_KEY secret in "Settings > Secrets"

