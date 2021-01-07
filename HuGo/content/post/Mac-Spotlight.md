---
title: "MAC 聚焦搜索卡死"
date: 2021-01-07
lastmod: 2021-01-07
draft: false
tags: ["MAC"]
categories: ["MAC"]
author: "Kail"
---



最近 Command + Space 搜索的时候，特别卡，打开之后，鼠标指针一直是加载中状态



<!-- more -->



试了几种，最终解决方法来源于： https://zhuanlan.zhihu.com/p/98316770



需要以下命令

```bash
# 关闭聚焦搜索（Spotlight）
$ sudo mdutil -a -i off

# 不加载控制聚焦参数的文件
$ sudo launchctl unload -w /System/Library/LaunchDaemons/com.apple.metadata.mds.plist

# 重新加载控制聚焦参数的文件
$ sudo launchctl load -w /System/Library/LaunchDaemons/com.apple.metadata.mds.plist

# 打开聚焦
$ sudo mdutil -a -i on
```



