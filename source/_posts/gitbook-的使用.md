---
title: gitbook 的使用
date: 2020-12-24 17:23:23
tags:
categories: 工具
---

# 安装
- 需要安装 `nodejs` , 使用 `node -v` 来检测
- 执行命令 `npm install gitbook-cli -g` 进行安装
- 执行命令 `gitbook init {dir}` 下载 gitbook 并在指定目录下初始化
- 执行命令 `gitbook server` 以服务的形式启动 , 浏览器访问 `127.0.0.1:4000`
- 执行命令 `gitbook build` 完成html的建立

# build失败
```
如果 gitbook build 时报错
找到类似下面的路径
C:\Users\32631\.gitbook\versions\3.2.3\lib\output\website
找到目录下的 copyPluginAssets.js 文件
修改 112 行和 67 行 将confirm 改为false
```

# HTML页面无法通过目录跳转
```
build 之后无法通过左侧目录进行跳转
vim _book/gitbook/theme.js 找的这个文件进行修改
找到 if(m)for(n.handler 这句话把 m 换成 false
每次 build 后都需要进行修改
```
# 配合 [database-doc-generator](https://github.com/enilu/database-doc-generator)

```
从github上下载database-doc-generator
使用命令生成一系列md文档
通过gitbook build 或 gitbook serve 编译
```