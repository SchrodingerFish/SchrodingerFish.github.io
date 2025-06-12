---
title: Rocky Linux下 Python项目生成虚拟环境
date: 2025-06-12 16:50:34
categories:
  - Python
tags:
  - Python
  - Rocky Linux
  - .venv
---
在 Rocky Linux 8 上为 Python 3.12 项目生成和使用虚拟环境的步骤：

**1. 确保安装了 `venv` 模块:**

`venv` 模块是 Python 3 官方推荐的创建虚拟环境的工具。 大部分情况下 `venv` 已经默认安装。 检查是否安装：

```bash
python3.12 -m venv --help
```

如果出现帮助信息，说明 `venv` 已经安装。 如果出现 "No module named venv" 错误，则需要手动安装 `python3-venv` 包：

```bash
sudo dnf install python3-venv
```

**2. 创建虚拟环境:**

导航到你的项目目录，或者你想创建虚拟环境的任何目录。 然后使用以下命令创建虚拟环境：

```bash
python3.12 -m venv .venv
```

* `python3.12 -m venv`:  使用 Python 3.12 运行 `venv` 模块。
* `.venv`:  虚拟环境的名称。 这会将虚拟环境文件存储在当前目录下的 `.venv` 目录中。 你也可以选择其他名称，例如 `venv` 或 `myenv`。 通常推荐使用点开头命名，表示隐藏目录

**3. 激活虚拟环境:**

创建虚拟环境后，需要激活它才能开始使用。 使用以下命令激活虚拟环境：

```bash
source .venv/bin/activate
```

激活后，你的终端提示符会发生变化，通常会在前面显示虚拟环境的名称，例如 `(.venv)`。

**4. 在虚拟环境中安装软件包:**

激活虚拟环境后，可以使用 `pip` 命令安装项目所需的软件包。 例如：

```bash
pip install requests flask
```

请注意，在激活的虚拟环境中，可以直接使用 `pip` 命令，而无需指定 `python3.12 -m pip`。  `pip` 命令会自动指向虚拟环境中的 Python 解释器和软件包。

**5. 停用虚拟环境:**

完成工作后，可以使用以下命令停用虚拟环境：

```bash
deactivate
```

停用后，终端提示符会恢复到正常状态。

**完整的例子:**

```bash
# 创建项目目录 (如果还没有)
mkdir myproject
cd myproject

# 创建虚拟环境
python3.12 -m venv .venv

# 激活虚拟环境
source .venv/bin/activate

# 安装软件包
pip install requests flask

# (进行项目开发)

# 停用虚拟环境
deactivate
```

**解释和最佳实践:**

* **`.venv` 目录:**  通常将虚拟环境目录命名为 `.venv` 并放在项目根目录下。  这是一种常见的约定，并且可以方便地将 `.venv` 添加到 `.gitignore` 文件中，以防止将虚拟环境文件提交到版本控制系统。
* **`venv` vs. `virtualenv`:**  `venv` 是 Python 3 官方推荐的虚拟环境工具，并且已经包含在 Python 3.3 及更高版本中。 `virtualenv` 是一个第三方工具，在 `venv` 出现之前被广泛使用。 现在，`venv` 通常是更好的选择，因为它不需要额外安装。
* **`requirements.txt`:**  使用 `requirements.txt` 文件可以方便地管理项目依赖。  可以使用以下命令将当前虚拟环境中的所有软件包及其版本导出到 `requirements.txt` 文件中：

   ```bash
   pip freeze > requirements.txt
   ```

  然后，可以使用以下命令从 `requirements.txt` 文件安装所有软件包：

   ```bash
   pip install -r requirements.txt
   ```

  这在与他人共享项目或在不同的环境中部署项目时非常有用。
* **将 `.venv` 添加到 `.gitignore`:**  为了避免将虚拟环境文件提交到版本控制系统（例如 Git），请将 `.venv` 添加到项目的 `.gitignore` 文件中：

   ```
   .venv/
   ```

  这可以节省存储空间并避免潜在的冲突。
* **在 IDE 中配置虚拟环境:**  如果你使用集成开发环境 (IDE)，例如 VS Code、PyCharm 或 Sublime Text，请确保将虚拟环境配置为项目的 Python 解释器。  这样，IDE 就可以自动识别虚拟环境中的软件包，并提供代码完成和 linting 等功能。  具体配置方法请参考 IDE 的文档。

使用虚拟环境是 Python 项目开发的一个基本但重要的实践。 它可以帮助你保持项目的整洁和可维护性，并避免依赖冲突。  按照这些步骤操作，你应该能够轻松地为你的 Python 3.12 项目创建和使用虚拟环境。