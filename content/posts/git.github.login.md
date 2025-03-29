---
title: "GitHub SSH 登录配置指南"
date: 2025-03-27
draft: false
description: "详细介绍如何配置 GitHub SSH 登录，包括生成密钥、添加到 GitHub 和常见问题排查"
tags: ["Git", "GitHub", "SSH", "认证"]
categories: ["技术"]
---

在 GitHub 使用 **SSH 认证** 可以避免输入用户名和密码，并且比 HTTPS 方式更安全。以下是完整的配置步骤：

---

## **✅ 1. 检查是否已有 SSH 密钥**

在终端或 PowerShell 中运行：

```sh
ls ~/.ssh/id_rsa.pub  # macOS/Linux
dir $env:USERPROFILE\.ssh\id_rsa.pub  # Windows PowerShell
```

如果输出文件路径，说明你已经有 SSH Key，可直接跳到 **步骤 3**。否则请继续。

---

## **✅ 2. 生成 SSH 密钥**

如果没有 SSH Key，需要生成：

```sh
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

**解释**：

- `-t rsa`：使用 RSA 算法。
- `-b 4096`：生成 4096 位的密钥（更安全）。
- `-C "your_email@example.com"`：用你的 GitHub 邮箱作为注释。

### **👉 生成后会提示：**

```sh
Enter file in which to save the key (/Users/yourname/.ssh/id_rsa):
```

直接按 **Enter**，默认存储在 `~/.ssh/id_rsa`（Windows 为 `C:\Users\你的用户名\.ssh\id_rsa`）。

然后它会要求输入 **密码短语（可选）**：

```sh
Enter passphrase (empty for no passphrase):
```

可以直接 **Enter** 跳过，也可以输入密码以增加安全性。

---

## **✅ 3. 添加 SSH Key 到 GitHub**

### **1️⃣ 复制公钥**

运行：

```sh
cat ~/.ssh/id_rsa.pub  # macOS/Linux
type $env:USERPROFILE\.ssh\id_rsa.pub  # Windows PowerShell
```

**然后复制输出的整个密钥（以 `ssh-rsa` 开头）。**

### **2️⃣ 添加到 GitHub**

1. **打开 GitHub** → **Settings** → **SSH and GPG keys**  
   🔗 直接访问：[GitHub SSH Key 页面](https://github.com/settings/keys)
2. 点击 **New SSH Key**
3. **Title**：随便填一个名称（如 "My Laptop SSH Key"）
4. **Key**：粘贴刚才复制的 SSH 公钥
5. 点击 **Add SSH Key**

---

## **✅ 4. 测试 SSH 连接**

执行：

```sh
ssh -T git@github.com
```

如果成功，会显示：

```sh
Hi your-github-username! You've successfully authenticated, but GitHub does not provide shell access.
```

✅ **说明 SSH 连接已成功！**

---

## **✅ 5. 配置 Git 远程仓库**

如果你之前用 HTTPS 方式克隆了 GitHub 仓库，你可以改用 SSH：

### **1️⃣ 修改远程仓库地址**

```sh
git remote set-url origin git@github.com:your-username/your-repo.git
```

或者如果你要克隆一个新仓库，直接用 SSH：

```sh
git clone git@github.com:your-username/your-repo.git
```

---

## **✅ 6. 常见问题排查**

### **1️⃣ `Permission denied (publickey).`**

**解决方案**：

```sh
ssh-add ~/.ssh/id_rsa
```

如果 SSH 代理未启动，先运行：

```sh
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
```

然后再测试 `ssh -T git@github.com`。

---

### **2️⃣ Windows PowerShell 提示 `ssh: command not found`**

Windows 需要 **确保 SSH 可用**：

- **Windows 10+** 自带 SSH，不需要安装。
- **检查 SSH 是否启用**：

  ```sh
  Get-Service ssh-agent
  ```

  如果 `Status` 不是 `Running`，请启用：

  ```sh
  Start-Service ssh-agent
  ```

  并设置为开机自启：

  ```sh
  Set-Service -Name ssh-agent -StartupType Automatic
  ```

---

### **3️⃣ GitHub 提示 `Could not resolve hostname github.com`**

**可能是 DNS 问题**，尝试：

```sh
ping github.com
```

如果无法解析，可以用：

```sh
echo "140.82.113.3 github.com" | sudo tee -a /etc/hosts
```

（Windows 需要修改 `C:\Windows\System32\drivers\etc\hosts`）

---

## **🎯 总结**

| 步骤                  | 命令                                                                   |
| --------------------- | ---------------------------------------------------------------------- |
| **检查 SSH Key**      | `ls ~/.ssh/id_rsa.pub`                                                 |
| **生成 SSH Key**      | `ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`                |
| **复制公钥**          | `cat ~/.ssh/id_rsa.pub`                                                |
| **测试连接**          | `ssh -T git@github.com`                                                |
| **修改 Git 远程仓库** | `git remote set-url origin git@github.com:your-username/your-repo.git` |

---

### 🎉 **现在，你的 GitHub 已支持 SSH 登录！**
