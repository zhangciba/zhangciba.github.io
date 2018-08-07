---
title: MacOSX避免每次都用ssh-add添加SSH秘钥
date: 2016-04-06 08:50:39
tags: 
 - SSH
 - SSH密钥
 - SSH公钥
categories: ssh
---

&#160; &#160; &#160; &#160;ssh-add 这个命令并不会永久性地记住你所使用的私钥。实际上，它的作用只是把你指定的私钥添加到 ssh-agent 所管理的一个 session 当中。而 ssh-agent 是一个用于存储私钥的临时性的 session 服务，也就是说当你重启之后，ssh-agent 服务也就重置了。

&#160; &#160; &#160; &#160;下面以我日常使用的 Mac OS X 为例。Mac 系统内置了一个 Keychain 的服务及其管理程序，可以方便的帮你管理各种秘钥，其中包括 ssh 秘钥。ssh-add 默认将指定的秘钥添加在当前运行的 ssh-agent 服务中，但是你可以改变这个默认行为让它将指定的密钥添加到 keychain 服务中，让 Mac 来帮你记住、管理并保障这些秘钥的安全性。

&#160; &#160; &#160; &#160;执行如下命令，
`$ ssh-add -K [path/to/your/ssh-key]`
<!-- more -->
&#160; &#160; &#160; &#160;之后，其他的程序请求 ssh 秘钥的时候，会通过 Keychain 服务来请求。下面的截图里你可以看到我当前的机器上 Keychain 为我管理的有关 ssh 的秘钥，这其中包括我自己生成的四个，以及 Github Client App 自己使用的一个——前者几个都是供 ssh 相关的命令所使用，而后者则指明了仅供 Github.app 这个应用程序使用。 另外，它们都是 login keychains 也就是只有当前用户登录之后才会生效的，换了用户或是未登录状态是不能使用的，这就是 Keychain 服务所帮你做的事情。

![KeyChain Access](http://7xsnoh.com2.z0.glb.clouddn.com/keychain%20access.png "KeyChain Access")


