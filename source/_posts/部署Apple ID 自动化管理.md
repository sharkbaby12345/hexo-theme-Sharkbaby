---
title: 部署Apple ID 自动化管理
tags:
  - 基于密保问题，自动解锁Apple ID，自动关闭双重认证，提供前端账号展示，支持多账号。
categories:
  - 源码搭建
date: 2023-05-30 18:19:03
updated: 2023-05-30 22:00:00
---
<h1 align="center">Apple ID 自动化管理</h1>
<h3 align="center">中文文档 | <a href="README_en.md">English</a> </h3>
<h3 align="center">请仔细阅读本文档以及使用说明，再使用。</h3>
<h3 align="center">使用本项目可能需要一定的基础知识。</h3>
<h3 align="center">本项目仍在更新当中。<a href="https://github.com/sharkbaby12345/appleid_auto">源码下载</a></h3>

# 基本简介

“以全新方式管理你的 Apple ID” —— 这是一款基于密保问题的自动化 Apple ID 检测&解锁程序。

前端用于管理账号，支持添加多个账号，并提供展示账号页面；

支持创建包含多个账号的分享页面，并可以为分享页面设置密码。

后端定时检测账号是否被锁定，若被锁定或开启二步验证则自动解锁，修改密码并向API回报密码。

登录Apple ID并自动删除Apple ID中的设备。

启用代理池和Selenium集群，提高解锁成功率，防止风控。

# 项目特点

- 多用户使用，权限控制
- 多账号管理
- 账号分享页，支持设置密码、有效期、自定义HTML内容
- 自动解锁与关闭二步验证
- 自动/定时修改密码
- 自动删除Apple ID中的设备
- 代理池与Selenium集群，提高解锁成功率
- 允许手动触发解锁
- ……

# 使用文档

我们已将所有文档与使用说明整理到了GitBook上，欢迎前往查看：

https://appleid-auto.gitbook.io/doc_zhcn/

