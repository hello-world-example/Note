---
title: "Mac 清理"
date: 2020-11-14
lastmod: 2020-11-14
draft: false
tags: ["MAC"]
categories: ["MAC"]
author: "Kail"
---



>最近打开电脑的时候，发现 MAC 磁盘占占用又满了
>
>这里进行记录，便于下次再出现的时候快速删除无用的文件



<!-- more -->



## 基于命令

```bash
# sort -n 数字排序
# sort -n 倒序
$ du -s ~ | sort -nr
```



## 处理的文件

- `du -s /opt/* | sort -nr`
- `du -s ~/Library/Caches/* | sort -nr`
  - `du -s ~/Library/Caches/JetBrains/* | sort -nr` ！！！
  - ..
- `du -s ~/Library/Logs/* | sort -nr`
  - `du -s ~/Library/Logs/JetBrains/* | sort -nr`  ！！！
  - ...
- Docker 占用：
  -  `du -sh ~/Library/Containers/com.docker.docker/Data/vms/0/Docker.raw`



## PS

- 主要删除了一些 JetBrains 久版本的 Cache 和 Logs
- 后续不够用再删

