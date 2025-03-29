---
title: "Git 子仓库管理指南"
date: 2025-03-28
draft: false
description: "详细介绍如何使用 Git Submodule 和 Git Subtree 管理子仓库"
tags: ["Git", "版本控制", "子仓库"]
categories: ["技术"]
---

## **✅ 方法 1：使用 Git Submodule（推荐）**

适用于：

- 需要保持子仓库的独立性。
- 你希望子仓库作为独立项目，并可以单独更新。

### **1️⃣ 在指定文件夹中添加子模块**

在你的 GitHub 仓库根目录下运行：

```sh
git submodule add <子仓库URL> <目标文件夹路径>
```

例如：

```sh
git submodule add https://github.com/example/repo.git external/repo
```

这样，`repo` 仓库会被克隆到 `external/repo/` 目录中。

---

### **2️⃣ 提交更改**

```sh
git add .gitmodules external/repo
git commit -m "Add submodule: external/repo"
git push origin main
```

💡 **`.gitmodules` 文件** 记录了子模块信息，必须提交。

---

### **3️⃣ 拉取时同步子模块**

当其他人克隆你的仓库时，默认子模块不会自动拉取，需要执行：

```sh
git clone --recursive <你的仓库URL>
```

如果已经克隆了仓库但子模块目录是空的：

```sh
git submodule update --init --recursive
```

---

### **4️⃣ 更新子模块**

如果子仓库有新提交，你可以更新：

```sh
git submodule update --remote external/repo
git add external/repo
git commit -m "Update submodule: external/repo"
git push origin main
```

---

### **🚨 删除子模块**

如果你不想再使用这个子模块：

```sh
git submodule deinit -f -- external/repo
rm -rf .git/modules/external/repo
git rm -f external/repo
git commit -m "Remove submodule: external/repo"
```

---

## **✅ 方法 2：使用 Git Subtree（适用于合并子仓库代码）**

适用于：

- 你想把子仓库的代码合并到主仓库，而不是依赖子模块。
- 不希望子仓库依赖 `.gitmodules`，更适用于共享代码。

### **1️⃣ 添加子仓库到指定文件夹**

```sh
git subtree add --prefix=external/repo https://github.com/example/repo.git main --squash
```

- `--prefix=external/repo`：表示将子仓库的代码放入 `external/repo/` 文件夹。
- `main`：子仓库的分支。
- `--squash`：合并子仓库的历史为单个提交（避免 commit 过多）。

---

### **2️⃣ 更新子仓库代码**

当子仓库有更新时：

```sh
git subtree pull --prefix=external/repo https://github.com/example/repo.git main --squash
```

---

### **🚨 删除 Subtree**

```sh
rm -rf external/repo
git commit -am "Remove subtree external/repo"
git push origin main
```

---

## **🎯 选择哪种方法？**

| 方式                      | 适用场景                       | 是否需要 `.gitmodules` | 是否独立管理  |
| ------------------------- | ------------------------------ | ---------------------- | ------------- |
| **Git Submodule（推荐）** | 需要独立管理子仓库，适合共享库 | ✅ 需要                | ✅ 子模块独立 |
| **Git Subtree**           | 想直接合并代码，适合一次性引入 | ❌ 不需要              | ❌ 代码混合   |

如果你希望子仓库 **独立管理**，建议用 **Submodule**。如果只是想引入代码一次性使用，可以用 **Subtree**。

---

示例

```sh
# 添加子模块
git submodule add git@github.com:Kamker/hugo-PaperMod.git themes/PaperMod
git add .gitmodules themes/PaperMod
git commit -m "Add submodule: themes/PaperMod"
git push origin master

# 更新子模块
git submodule update --remote themes/PaperMod
git add themes/PaperMod
git commit -m "Update submodule: themes/PaperMod"
git push origin master
```
