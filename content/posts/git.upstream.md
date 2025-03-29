---
title: "GitHub Fork 仓库同步指南"
date: 2025-03-27
draft: false
description: "详细介绍如何同步 GitHub Fork 仓库与上游仓库，包括添加 upstream、获取更新和合并代码"
tags: ["Git", "GitHub", "Fork", "同步"]
categories: ["技术"]
---

在 GitHub 上 **fork** 了一个仓库后，原仓库（**上游仓库 upstream**）可能会有新的提交。如果你想把这些更新拉取到你的 fork 仓库中，可以按照以下步骤进行操作。

---

## **✅ 1. 添加上游（Upstream）仓库**

首先，你需要告诉 Git 你的 fork 关联的是哪个原仓库。

### **查看已有的远程仓库**

```sh
git remote -v
```

通常你会看到：

```sh
origin  git@github.com:your-username/repo.git (fetch)
origin  git@github.com:your-username/repo.git (push)
```

此时，**你的仓库只有 `origin`（fork 仓库），没有 `upstream`（原仓库）**。

### **添加上游仓库**

```sh
git remote add upstream https://github.com/original-owner/repo.git
```

**检查是否添加成功**：

```sh
git remote -v
```

现在应该能看到：

```sh
origin    git@github.com:your-username/repo.git (fetch)
origin    git@github.com:your-username/repo.git (push)
upstream  https://github.com/original-owner/repo.git (fetch)
upstream  https://github.com/original-owner/repo.git (push)
```

---

## **✅ 2. 获取上游仓库最新代码**

### **拉取上游最新代码**

```sh
git fetch upstream
```

这会拉取 **上游仓库的所有分支**，但不会自动合并。

---

## **✅ 3. 合并上游更新到你的 Fork**

### **方式 1：合并（Merge，保留提交历史）**

如果你在 `main` 分支：

```sh
git checkout main
git merge upstream/main
```

然后推送到你的 GitHub fork：

```sh
git push origin main
```

### **方式 2：变基（Rebase，保持线性历史）**

如果你希望你的提交历史干净：

```sh
git checkout main
git rebase upstream/main
```

然后推送：

```sh
git push origin main --force
```

⚠ **注意**：如果已经 push 过该分支，`--force` 可能导致历史记录变化，请谨慎使用。

---

## **✅ 4. 处理其他分支**

如果你的 fork 里有其他分支（例如 `feature-branch`），可以用：

```sh
git checkout feature-branch
git rebase upstream/main
git push origin feature-branch --force
```

---

## **✅ 5. GitHub 上同步 fork（无需命令行）**

如果你不想用命令行，可以直接：

1. **进入你的 GitHub fork 仓库**
2. 点击 **"Sync fork"**（如果可用）
3. 选择 **"Update branch"**

这样 GitHub 会自动合并原仓库的更新。

---

## **🎯 总结**

| 任务                   | Git 命令                                                             |
| ---------------------- | -------------------------------------------------------------------- |
| 添加 upstream          | `git remote add upstream https://github.com/original-owner/repo.git` |
| 拉取 upstream 最新代码 | `git fetch upstream`                                                 |
| 合并 upstream/main     | `git merge upstream/main`                                            |
| 变基（Rebase）         | `git rebase upstream/main`                                           |
| 推送更新到 Fork        | `git push origin main`                                               |

---

### 🎉 **你的 fork 现在已同步原仓库！**
