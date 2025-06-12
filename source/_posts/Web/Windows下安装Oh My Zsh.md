---
title: Windows下安装Oh My Zsh
date: 2025-06-12 16:53:11
categories:
  - Web
tags:
  - Windows
  - Oh My Zsh
---

在 Windows 下安装 Oh My Zsh 比在 macOS 或 Linux 下稍有不同，因为 Oh My Zsh 本身是为 Unix-like 环境设计的。不过，可以通过以下步骤在 Windows 上使用 Oh My Zsh：

**1. 安装 Windows Subsystem for Linux (WSL)**

* WSL 允许你在 Windows 上运行 Linux 环境。
* **启用 WSL 功能：**
    * 打开“控制面板” -> “程序” -> “启用或关闭 Windows 功能”。
    * 勾选 "Windows Subsystem for Linux" 并点击“确定”。
    * 完成后，重启计算机。
* **安装 Linux 发行版：**
    * 打开 Microsoft Store。
    * 搜索并选择一个 Linux 发行版（推荐 Ubuntu, Debian, 或 Kali Linux）。
    * 点击 "获取" 安装。
    * 安装完成后，启动 Linux 发行版。  第一次启动会提示你创建一个用户和密码。

**2. 更新和升级 WSL 环境**

* 打开你的 Linux 发行版终端。
* 运行以下命令更新包列表：
  ```bash
  sudo apt update
  ```
* 运行以下命令升级已安装的包：
  ```bash
  sudo apt upgrade
  ```

**3. 安装 Zsh**

* 在 WSL 终端中运行以下命令安装 Zsh：
  ```bash
  sudo apt install zsh
  ```

**4. 安装 Git**

* Oh My Zsh 依赖 Git。  在 WSL 终端中运行以下命令安装 Git：
  ```bash
  sudo apt install git
  ```

**5. 安装 Oh My Zsh**

* 使用 `curl` 或 `wget` 安装 Oh My Zsh：

    * **使用 curl (如果已安装):**
      ```bash
      sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
      ```
    * **使用 wget:**
      ```bash
      sh -c "$(wget https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh -O-)"
      ```

* 安装脚本会询问你是否要将 Zsh 设置为默认 Shell。  选择 'y' (yes) 即可。

**6. (可选) 安装 powerlevel10k 主题 (推荐)**

* Powerlevel10k 是一个非常流行的 Zsh 主题，它快速、可定制性强。
* **安装 Meslo Nerd Font (重要):** Powerlevel10k 需要 Nerd Fonts。  你需要下载并安装一个。 推荐 MesloLGS NF Regular。  你可以从 Nerd Fonts 官方网站下载： [https://www.nerdfonts.com/font-downloads](https://www.nerdfonts.com/font-downloads)  下载后，在 Windows 系统上安装字体。
* **下载 Powerlevel10k 主题:**
   ```bash
   git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
   ```
* **配置 Zsh 使用 Powerlevel10k:**
    * 编辑 `~/.zshrc` 文件：
      ```bash
      nano ~/.zshrc
      ```
    * 找到 `ZSH_THEME="robbyrussell"` (或其他主题名称) 并将其更改为：
      ```bash
      ZSH_THEME="powerlevel10k/powerlevel10k"
      ```
    * 保存并关闭 `~/.zshrc` 文件。
* **重启 Zsh 或打开新的终端窗口:**  打开新的 WSL 终端窗口。 Powerlevel10k 应该会提示你进行配置。 按照屏幕上的指示进行操作。

**7. (可选) 安装 Zsh 插件**

* Oh My Zsh 支持许多插件，可以增强你的终端体验。  一些流行的插件包括：
    * **`git`:** 提供 Git 的别名和实用程序。
    * **`zsh-autosuggestions`:**  根据你的历史记录建议命令。
    * **`zsh-syntax-highlighting`:**  高亮显示命令语法。

* **安装插件:**
    1. **克隆插件到 custom 目录:**
        * 例如，安装 `zsh-autosuggestions`:
          ```bash
          git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
          ```
        * 例如，安装 `zsh-syntax-highlighting`:
          ```bash
          git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
          ```
    2. **在 `~/.zshrc` 文件中启用插件:**
        * 编辑 `~/.zshrc` 文件：
          ```bash
          nano ~/.zshrc
          ```
        * 找到 `plugins=(git)` 行，并添加你要启用的插件。  例如：
          ```bash
          plugins=(git zsh-autosuggestions zsh-syntax-highlighting)
          ```
        * 保存并关闭 `~/.zshrc` 文件。
    3. **重新加载 Zsh 配置:**
       ```bash
       source ~/.zshrc
       ```

**8. 配置 Windows Terminal (推荐)**

* Windows Terminal 是一个现代化的终端应用程序，可以让你更舒适地使用 WSL 和 Zsh。
* **安装 Windows Terminal:**  从 Microsoft Store 安装。
* **配置 Windows Terminal 使用 WSL 和 Zsh:**
    * 打开 Windows Terminal 设置 (点击顶部的下拉箭头，选择 "设置")。
    * 在左侧选择你的 WSL 发行版 (例如 "Ubuntu")。
    * 将 "命令行" 设置为： `wsl.exe ~`
    * 将 "起始目录" 设置为你希望启动终端时进入的目录 (例如 `~` 或 `/home/yourusername`)。
    * **设置字体:**  在 "外观" 设置中，选择你安装的 Nerd Font (例如 "MesloLGS NF")，以正确显示 Powerlevel10k 主题的图标。  确保 "使用自定义字体面" 已启用。
    * 保存设置。

**总结:**

通过以上步骤，你就可以在 Windows 上成功安装和配置 Oh My Zsh。  记住安装 Nerd Font 并配置 Windows Terminal 以获得最佳体验。