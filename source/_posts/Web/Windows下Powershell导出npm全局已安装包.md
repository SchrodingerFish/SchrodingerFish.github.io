---
title: Windows Powershell 导出 npm 全局已安装包
date: 2026-04-25 08:20:11
categories:
  - Web
tags:
  - powershell
  - npm
---
# Windows Powershell 导出 npm 全局已安装包

#### 1. 导出列表 (修复版)

这一行命令通过 `npm list` 获取路径，然后通过 `Split-Path` 提取最后一个名字：

```powershell
npm list -g --depth=0 --parseable | ForEach-Object { Split-Path $_ -Leaf } | Where-Object { $_ -ne "node_modules" -and $_ -ne "npm" } > npm-global-list.txt
```

**命令解释：**

*   `npm list -g --depth=0 --parseable`：以可解析的完整路径格式列出所有全局包。
*   `Split-Path $_ -Leaf`：只取路径的最后一部分（即文件夹名/包名）。
*   `Where-Object`：排除掉 `npm` 自身和可能的 `node_modules` 根目录字样。

---

#### 2. 批量安装

当你拿到 `npm-global-list.txt` 后，在另一台机器或新环境下执行：

```powershell
Get-Content npm-global-list.txt | ForEach-Object { npm install -g $_ }
```

---

### 