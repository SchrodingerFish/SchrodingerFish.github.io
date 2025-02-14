---
title: OpenWrt版本查看
date: 2025-02-14 15:57:34
categories:
  - Linux
tags:
  - Linux
  - OpenWRT
---
在 OpenWrt 系统上，可以通过以下方法查看当前运行的 OpenWrt 版本信息：

---

### 1. 使用 `cat` 查看系统版本文件
通过读取 OpenWrt 的版本文件可以快速获取版本信息：

```bash
cat /etc/openwrt_release
```

输出结果类似如下：
```bash
DISTRIB_ID='OpenWrt'
DISTRIB_RELEASE='22.03.5'
DISTRIB_REVISION='r20134-5f15225c1e'
DISTRIB_TARGET='ramips/mt7621'
DISTRIB_ARCH='mipsel_24kc'
DISTRIB_DESCRIPTION='OpenWrt 22.03.5 r20134-5f15225c1e'
DISTRIB_TAINTS=''
```

从中可以提取关键信息，例如：
- **DISTRIB_RELEASE**：22.03.5，表示 OpenWrt 的版本号。
- **DISTRIB_TARGET**：ramips/mt7621，表示当前固件支持的硬件架构。
- **DISTRIB_REVISION**：r20134-5f15225c1e，表示源码的具体版本。
- **DISTRIB_ARCH**：mipsel_24kc，表示 CPU 的架构类型。

---

### 2. 使用 `uname` 查看内核版本
可以通过 `uname` 命令查看当前系统的 Linux 内核版本：

```bash
uname -a
```

输出类似如下：
```bash
Linux OpenWrt 5.10.176 #0 SMP Tue Feb 21 15:54:42 2023 mips GNU/Linux
```

从中可以看到具体的内核版本信息（如 `5.10.176`）。

---

### 3. 使用 `opkg` 查看软件版本
通过 `opkg` 命令查看当前 OpenWrt 软件包管理器的信息，其中可能会显示版本信息：

```bash
opkg print-architecture
```

这会输出当前系统支持的架构信息：

```bash
arch all 1
arch ramips_24kec 100
arch mipsel_24kc 200
```

---

### 4. Web 管理界面查看版本
如果你使用的是带 Web 界面的 OpenWrt（如 LuCI），可以通过以下步骤查看版本：
1. 登录 Web 管理界面（默认地址是 `192.168.1.1`）。
2. 进入 **状态 > 概览** 页面（Status > Overview）。
3. 在 **固件版本**（Firmware Version）或 **系统信息** 中，你可以看到当前固件的具体版本，比如 `OpenWrt 22.03.5`。

---

### 5. 使用 `dmesg` 查看启动日志
`dmesg` 可以查看系统启动日志，其中可能包含 OpenWrt 固件版本信息：

```bash
dmesg | grep OpenWrt
```

如果你的固件有特殊的定制信息，比如 "F大固件" 或其他社区编译版本，也可以从这里发现。

---

以上这些方法都可以用来快速查询当前 OpenWrt 的版本信息，操作简单，适配不同场景。建议先使用第一个方法 `cat /etc/openwrt_release`，这是最简明直观的方式。