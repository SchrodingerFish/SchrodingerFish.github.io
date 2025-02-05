---
title: Git常用操作
date: 2025-02-05 10:26:00
categories:
  - Web
tags:
  - Git
---


在日常开发中，Git 是一个非常重要的工具，熟练掌握 Git 的常用命令，可以显著提高你的效率。以下是一些 Git 常用命令，分为几个核心类别进行整理。

---

### **1. Git 配置**
```bash
# 设置用户名
git config --global user.name "Your Name"

# 设置邮箱
git config --global user.email "youremail@example.com"

# 查看配置信息
git config --list

# 设置默认编辑器
git config --global core.editor "vim"

# 设置换行符转换方式（Windows）
git config --global core.autocrlf true
```

---

### **2. 仓库操作**
```bash
# 初始化一个新的 Git 仓库
git init

# 克隆远程仓库
git clone [仓库地址]
```

---

### **3. 文件操作**
```bash
# 查看当前状态（工作目录和暂存区域）
git status

# 添加文件到暂存区
git add [文件名]
git add .

# 从暂存区移除（取消暂存）
git reset [文件名]

# 提交代码到本地仓库
git commit -m "提交信息"

# 修改上一次提交的备注信息
git commit --amend -m "新的提交信息"

# 将暂存区的修改提交到仓库中，并跳过使用编辑器添加备注
git commit --no-edit
```

---

### **4. 分支操作**
```bash
# 查看分支
git branch

# 创建新分支
git branch [分支名]

# 切换分支
git checkout [分支名]

# 创建并切换到新分支
git checkout -b [分支名]

# 删除本地分支
git branch -d [分支名]       # 删除已合并的分支
git branch -D [分支名]       # 强制删除

# 合并分支到当前分支
git merge [分支名]

# 查看分支图（依赖工具 `git log`）
git log --oneline --graph --decorate
```

---

### **5. 远程操作**
```bash
# 查看当前远程仓库信息
git remote -v

# 添加远程仓库
git remote add origin [仓库地址]

# 推送本地代码到远程分支
git push -u origin [分支名]  # 初次推送并绑定
git push                     # 之后直接使用

# 拉取远程仓库代码
git pull [远程仓库] [分支名]

# 获取远程仓库的更新但不合并
git fetch [远程仓库]

# 将远程分支与本地分支关联
git branch --set-upstream-to=origin/[远程分支] [本地分支]
```

---

### **6. 日志查看**
```bash
# 查看提交历史
git log

# 简要查看提交历史
git log --oneline

# 查看指定文件的提交记录
git log [文件名]
```

---

### **7. 回退操作**
```bash
# 撤销修改（恢复最近一次提交后的状态）
git checkout -- [文件名]

# 回退到上一个提交版本，但保留工作区文件
git reset --soft HEAD^

# 回退到上一个提交版本，同时清空暂存区，但保留工作区修改
git reset --mixed HEAD^

# 回退到指定版本，丢弃所有工作区修改
git reset --hard [指定版本的commit_id]

# 恢复某个文件到指定版本（在当前分支上还原）
git checkout [commit_id] -- [文件名]
```

---

### **8. 标签操作**
```bash
# 创建标签
git tag [标签名]

# 创建带附注的标签
git tag -a [标签名] -m "标签信息"

# 查看标签
git tag

# 推送标签到远程
git push origin [标签名]

# 推送所有本地标签到远程
git push origin --tags

# 删除本地标签
git tag -d [标签名]

# 删除远程标签
git push origin --delete [标签名]
```

---

### **9. Stash（暂存功能）**
```bash
# 保存当前工作现场
git stash

# 查看 stash 列表
git stash list

# 恢复最近一次保存的工作现场
git stash pop

# 恢复指定 stash
git stash apply stash@{编号}

# 删除最近的 stash
git stash drop

# 删除所有 stash
git stash clear
```

---

### **10. 配合 `.gitignore` 文件**
在项目根目录创建 `.gitignore` 文件，用来忽略不需要追踪的文件。
```bash
# 创建一个简单的 .gitignore 文件
touch .gitignore

# 常见规则示例
*.log       # 忽略所有 .log 文件
temp/       # 忽略 temp 目录
!important/ # 忽略目录，但保留其中的 important 文件夹
```

---

这些命令覆盖了 Git 的大部分常用功能，建议在实际开发中多加练习，熟悉这些命令的使用方式。