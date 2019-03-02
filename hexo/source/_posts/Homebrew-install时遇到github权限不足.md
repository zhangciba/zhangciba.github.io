---
title: Homebrew install时遇到github权限不足
date: 2016-04-29 16:29:57
categories: Mac OSX 
tags: Homebrew
---

在使用brew工具时，报了以下错误,github API 访问的权限限制
```
Error:GitHub API rate limit exceeded for [xxxx]. (But here's the good news: Authenticated requests get a higher rate limit. Check out the documentation for more details.)
Try again in 5 minutes 54 seconds, or create an personal access token:
https://github.com/settings/tokens
and then set the token as: HOMEBREW_GITHUB_API_TOKEN
 
```

首先打开  https://github.com/settings/tokens/new

登陆后，输入Homebrew，生成token

设置环境变量HOMEBREW_GITHUB_API_TOKEN：

将HOMEBREW_GITHUB_API_TOKEN变量设置为获取到的token,在终端输入:
```
export HOMEBREW_GITHUB_API_TOKEN=XXXXXXXXXX

```

