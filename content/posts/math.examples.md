---
title: "数学公式示例"
date: 2025-03-26
draft: false
description: "展示 Hugo 中 KaTeX 数学公式的使用方法"
tags: ["Hugo", "数学", "KaTeX"]
categories: ["技术"]
---

## KaTeX 数学公式示例

在 Hugo 中使用 KaTeX 可以渲染漂亮的数学公式。让我们来看一些例子：

### 行内公式

行内公式使用单个 `$` 符号包裹，例如：$E = mc^2$、$a^2 + b^2 = c^2$

### 块级公式

块级公式使用双 `$$` 符号包裹，例如：

$$
\frac{n!}{k!(n-k)!} = \binom{n}{k}
$$

### 矩阵

$$
\begin{pmatrix}
a & b \\\\
c & d
\end{pmatrix}
$$

{{< raw >}}
$$
\begin{bmatrix}
a & b \\
c & d
\end{bmatrix}
$$
{{< /raw >}}

### 积分

$$
\int_{a}^{b} f(x) dx
$$

### 求和

$$
f(x) = \sum_{n=0}^{\infty} \frac{f^{(n)}(a)}{n!} (x-a)^n
$$

### 极限

$$
\lim_{x \to \infty} \frac{1}{x} = 0
$$

## 使用技巧

1. 行内公式使用单个 `$` 符号
2. 块级公式使用双 `$$` 符号
3. 可以使用 `\begin{align}` 和 `\end{align}` 来对齐多个公式
4. 使用 `\label` 和 `\ref` 来引用公式

## 注意事项

1. 确保在 front matter 中启用了 KaTeX
2. 某些复杂的公式可能需要额外的 KaTeX 扩展
3. 建议使用 CDN 加载 KaTeX 资源以提高加载速度

希望这些示例对您有帮助！
