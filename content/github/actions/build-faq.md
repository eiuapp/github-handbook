---
date: 2019-06-27T15:15:15+08:00
title: "github actions build stage error"
weight: 1141
keywords: 
description: "github actions"
---

github actions build stage error

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

