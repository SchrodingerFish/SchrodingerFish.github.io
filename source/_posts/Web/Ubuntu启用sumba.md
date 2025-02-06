---
title: Ubuntu启用Samba
date: 2025-02-04 15:17:00
categories:
  - Web
tags:
  - Unbutu
  - Samba
excerpt: 本文将介绍如何在 Ubuntu 上启用 Samba 服务，以便在局域网内共享文件和目录。Samba 是一个开源的 SMB/CIFS 协议实现，可以让 Linux 和 Windows 之间实现文件和打印机共享。
---
## 按照以下步骤启用 Samba

0. 运行命令:
    ```bash 
    sudo apt install samba
   ```

1. **确保用户账号正确配置**：

   - 确保 Samba 用户 `johnson` 已正确添加，并设置了 Samba 密码。

     ```bash

     sudo smbpasswd -a johnson

     ```
2. **检查 Samba 配置文件** (`/etc/samba/smb.conf`)：

   - 确保 `[共享名称]` 小节中的配置正确无误。根据您的设置，应该类似这样：

     ```bash
     [我的云存储]
     comment = 我的云存储
     path = /mnt/ntfs
     valid users = johnson
     read only = no
     guest ok = no
     create mask = 0660
     directory mask = 0770

     ```
3. **检查权限**：

   - 确保 `/mnt/ntfs` 目录的权限允许 `johnson` 用户读取和写入。可以使用以下命令检查和设置权限：

     ```bash

     sudo chown -R johnson:johnson /mnt/ntfssudo chmod -R 0770 /mnt/ntfs

     ```
4. **Samba 服务重启**：

   - 每次更改配置后，别忘了重启 Samba 服务：

     ```bash

     sudo systemctl restart smbd
     sudo systemctl restart nmbd

     ```
5. **客户端设置**：

   - 确保在尝试连接到共享文件夹时，使用的是 `johnson` 用户名和设置的密码。
   - 如果是在 Windows 上访问，请确保在连接网络驱动器时选择“使用其他凭据”选项，并输入 Samba 用户名和密码。
6. **检查 Samba 日志**：

   - 如果问题仍然存在，可以检查 Samba 的日志文件以获取更多信息。日志文件通常位于 `/var/log/samba/` 下。
