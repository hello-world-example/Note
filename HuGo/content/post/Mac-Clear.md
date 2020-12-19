---
title: "Mac 清理"
date: 2020-11-14
lastmod: 2020-12-19
draft: false
tags: ["MAC"]
categories: ["MAC"]
author: "Kail"
---



>最近打开电脑的时候，发现 MAC 磁盘占用又满了
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



## 清理 Maven 编译

> $ vim mvn-clean-all.sh
>
> $ bash mvn-clean-all.sh ~/IdeaProjects

```bash
#!/bin/bash

if [[ "x$1" == "x" ]]; then
  echo '缺少扫描路径'
  exit 1
fi

echo "扫描路径为： $1"

# 查找所有 pom.xml 文件，保存 pom.xml 的 父目录， /path/pom.xml --> /path
for pom in $(find $1 -name pom.xml); do
  pom_parent=(${pom_parent[@]} ${pom%/*})
done

# 对 pom.xml 所在的目录进行排序，这样 子父模块的项目 父模块路径会排在前面
for item in $(echo ${pom_parent[*]} | tr ' ' '\n' | sort); do
  pom_parent_sorted=("${pom_parent_sorted[@]}" "${item}")
done

# echo "--------------------------------------------------------------------------------"
# echo "结果： "${pom_parent_sorted[@]}
# echo "长度： "${#pom_parent_sorted[@]}
# echo "--------------------------------------------------------------------------------"

# 去除子模块，保留父模块
pom_roots=(${pom_parent_sorted[0]})
# 排序后第一个位置肯定是 根模块 路径
current_root=${pom_parent_sorted[0]}

for path in ${pom_parent_sorted[@]}; do
  # 1. 子模块替换掉根路径后，是模块名
  # 2. 如果替换之后没有变化，说明已经到下一个项目了
  if [[ ${path#${current_root}} == ${path} ]]; then
    # 记录下一个项目的 根模块
    pom_roots=("${pom_roots[@]}" "${path}")
    current_root=${path}
  fi
done

# -----------------------------------

for pom_root in ${pom_roots[@]}; do
  pom_path="$pom_root/pom.xml"
  echo $pom_path
  # 清理 Maven 项目
  mvn clean -f $pom_path --offline >/dev/null
done
```

