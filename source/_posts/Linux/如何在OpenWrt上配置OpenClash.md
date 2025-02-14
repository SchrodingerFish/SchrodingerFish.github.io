---
title: 如何在OpenWrt上配置OpenClash
date: 2025-02-14 16:57:34
categories:
  - Linux
tags:
  - Linux
  - OpenWRT
  - OpenClash
---

## 概述

Clash 是一个基于Go语言开发的跨平台代理程序。

OpenClash 是 Clash 在通用 Openwrt 平台下的一个图形化分支。

## 安装 OpenClash

多数情况下，第三方编译的固件已自带 OpenClash 。  
若未安装，可以参考 OpenClash 的 [GitHub Releases](https://github.com/vernesong/OpenClash/releases) 自行安装。(无法提供帮助)

## 配置 OpenClash

如果第一次使用 OpenClash，需要手动选择内核编译版本。否则可跳过此步骤。在 OpenClash 页面，进入 “Global Settings (全局设置)” ，点击 “General Settings (基本设置)” 。  
在 “Chose to Download (选择内核编译版本)” 列表中选择对应您 CPU 架构的 Clash 版本。  
点击页面底部的 “Apply Configurations (应用配置)” 。  
![](https://storage.crisp.chat/users/helpdesk/website/d516709242f0180/2021-03-13-13758_42jzop.png)  
进入 “Version Update (版本更新)”，点击 Dev 内核后的 “Check and Update (检查并更新)” 。  
![](https://storage.crisp.chat/users/helpdesk/website/d516709242f0180/2021-03-13-13855_fh261.png)  
只需下载 Dev 版本的 Clash 内核，就能使用基本的 Clash 功能。如果无法下载 Clash 内核，请前往 OpenClash 的 [GitHub Releases](https://github.com/vernesong/OpenClash/releases) 页面下载对应您 CPU 架构的 Clash 版本，并通过 FTP 等工具上传到指定的文件夹进行手动导入。

## 添加 Clash 配置订阅

请前往官网 用户中心 复制 Clash 配置链接。  
进入 “Config Update (配置文件订阅)” 页面。  
点击 “Add (添加)” 按钮，键入 “Config Alias (配置文件名)” 和 “Subscribe Address (订阅地址)” 。  
点击 “Commit Configurations (保存配置)” 按钮。  
![](https://storage.crisp.chat/users/helpdesk/website/d516709242f0180/2021-03-13-208352_57l8fe.png)

###### ͏ 开启自动更新，OpenClash 可以定时更新添加的配置文件。

点击启用 “Auto Update (自动更新)” 。  
设置更新模式为 “Appointment Mode (预约)” ，更新时间建议选择每天 4:00 。  
点击 “Apply Configurations (应用配置)” ，OpenClash 将下载配置文件。  
![](https://storage.crisp.chat/users/helpdesk/website/d516709242f0180/2021-03-13-20609_604w0w.png)

## 选择节点

###### ͏ 配置文件下载成功后，OpenClash 将自动运行。

在 “Overview (运行状态)” 页面，点击 “Open Panel (打开面板)” 。  
![](https://storage.crisp.chat/users/helpdesk/website/d516709242f0180/2021-03-13-22115_130tlbn.png)  
点击页面左侧的 Proxies (代理)，在 🍃 Proxies 策略组中选择需要使用的节点。  
![](https://storage.crisp.chat/users/helpdesk/website/d516709242f0180/2021-03-13-22415_mcy1tp.png)  
Clash 支持通过策略组，对不同的网站使用不同的策略。合理使用分流可以提升您的使用体验。

🍃 Proxies 为节点策略组，可在其中选择节点。  
🍂 Domestic 决策中国大陆网站，默认 DIRECT 直连。  
☁️ Others 决策没有收录进规则的网站，默认使用 🍃 Proxies 代理访问。

## Welcome to Internet

![](https://storage.crisp.chat/users/helpdesk/website/d516709242f0180/2021-03-13-23428_1xb327j.png)
