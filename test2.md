---
theme: default
title: 蛮力法
info: 算法设计与分析 - 第4章 基本的算法策略
author: OpenAI
class: text-center
drawings:
  persist: false
transition: slide-left
mdc: true
highlighter: shiki
lineNumbers: true
---

# 算法设计与分析

## 第4章 基本的算法策略

<div class="mt-8 text-xl opacity-80">
4.1 迭代算法 &nbsp;|&nbsp;
4.2 蛮力法 &nbsp;|&nbsp;
4.3 分而治之算法 &nbsp;|&nbsp;
4.4 贪婪算法 &nbsp;|&nbsp;
4.5 动态规划 &nbsp;|&nbsp;
4.6 算法策略间的比较
</div>

---

# 4.2 蛮力法

## 基本思想

蛮力法利用计算机运算速度快的特点，采取一种"懒惰"的策略：

<div v-click="1">

- 不经过复杂推理
- 或只经过较少分析
- 对所有可能情况逐一尝试
- 从中找出问题的解

</div>

<div v-click="2" class="mt-6 border-l-4 border-blue-400 pl-4 text-sm opacity-80">
适用场景：选择排序、冒泡排序、插入排序、顺序查找、朴素字符串匹配等
</div>

---

# 蛮力法常用方法

## 常见策略

- **枚举法**：逐一列出所有可能解
- **盲目搜索法**：系统搜索解空间
- 本节重点：**枚举法**

<div v-click class="mt-6">

## 基本步骤

1. **找出枚举范围** — 分析问题所涉及的各种情况
2. **找出约束条件** — 分析问题的解需要满足的条件，并用逻辑表达式表示

</div>

---

# 4.2.1 枚举法

## 枚举法（enumerate / 穷举法）

<div class="my-8 text-lg">

> 根据问题条件列出所有可能情况，逐一尝试，找到满足条件的解。
> 当解空间很大时，需要尽可能排除不合理情况。

</div>

<div v-click>

```txt
for each candidate in all_possible_cases:
    if candidate satisfies constraints:
        output answer
```

</div>

---

# 例1：百钱百鸡问题

## 问题描述

中国古代数学家张丘建在《算经》中提出：

<div class="grid grid-cols-3 gap-4 mt-6">
<div class="text-center">
<div class="text-3xl">🐓</div>
<div>鸡翁 1 只，值钱 5</div>
</div>
<div class="text-center">
<div class="text-3xl">🐔</div>
<div>鸡母 1 只，值钱 3</div>
</div>
<div class="text-center">
<div class="text-3xl">🐤</div>
<div>鸡雏 3 只，值钱 1</div>
</div>
</div>

<div v-click class="mt-8 text-xl font-bold text-amber-600">
百钱买百鸡，问公鸡、母鸡、鸡雏各几何？
</div>

---

# 百钱百鸡：数学模型

设：

- 公鸡为 $x$
- 母鸡为 $y$
- 鸡雏为 $z$

<div v-click class="mt-4">

则满足约束：

$$\begin{cases}
x + y + z = 100 \\
5x + 3y + z = 100
$$

</div>

---

# 百钱百鸡：算法设计1

## 三重枚举

<div class="grid grid-cols-2 gap-4">

<div>

### 枚举范围
- 公鸡 $x$: $1 \sim 20$
- 母鸡 $y$: $1 \sim 33$
- 鸡雏 $z$: $1 \sim 100$

### 约束条件
- $x + y + z = 100$
- $5x + 3y + z/3 = 100$
- $z \bmod 3 = 0$

</div>

<div v-click>

```c
main() {
    int x, y, z;
    for (x = 1; x <= 20; x++)
        for (y = 1; y <= 33; y++)
            for (z = 1; z <= 100; z++)
                if (x + y + z == 100 &&
                    5*x + 3*y + z/3 == 100 &&
                    z % 3 == 0)
                    printf("%d %d %d\n", x, y, z);
}
```

</div>

</div>

<div v-click class="mt-4 text-center text-sm opacity-80">
枚举规模：20 × 33 × 100 = <span class="font-bold">66,000</span>
</div>

---

# 百钱百鸡：算法设计2

## 优化枚举

<div class="grid grid-cols-2 gap-4">

<div>

### 枚举范围
- 公鸡 $x$: $1 \sim 20$
- 母鸡 $y$: $1 \sim 33$
- 鸡雏 $\mathbf{z = 100 - x - y}$

### 约束条件
- $z \bmod 3 = 0$
- $5x + 3y + z/3 = 100$

</div>

<div v-click>

```c
main() {
    int x, y, z;
    for (x = 1; x <= 20; x++)
        for (y = 1; y <= 33; y++) {
            z = 100 - x - y;
            if (z % 3 == 0 &&
                5*x + 3*y + z/3 == 100)
                printf("%d %d %d\n", x, y, z);
        }
}
```

</div>

</div>

<div v-click class="mt-4 text-center text-sm opacity-80">
枚举规模：20 × 33 = <span class="font-bold">660</span> &nbsp;(降低了 100 倍!)
</div>

---

# 例2：乘法数字问题

## 题意

寻找满足如下条件的乘法式：

<div class="my-8 flex justify-center">

```
      A B C A B
   ×          A
   -----------
    D D D D D D
```

</div>

<div v-click class="text-center">

某个**五位数**乘以**一位数**，结果为**六位数**，且结果的各位数字相同。

</div>

---

# 乘法数字问题：算法设计1

## 按乘法枚举

<div class="grid grid-cols-2 gap-4">

<div>

### 枚举范围
- $A$: $3 \sim 9$
- $B$: $0 \sim 9$
- $C$: $0 \sim 9$

### 思路
构造五位数 $F = ABCAB$，计算 $F \times A$，检查乘积各位是否相同。

</div>

<div v-click>

```c
main() {
    int A, B, C, D, E, E1, F, G1, G2, i;
    for (A = 3; A <= 9; A++)
        for (B = 0; B <= 9; B++)
            for (C = 0; C <= 9; C++) {
                F = A*10000 + B*1000 + C*100 + A*10 + B;
                E = F * A;  E1 = E;
                G1 = E1 % 10;
                for (i = 1; i <= 5; i++) {
                    G2 = G1;  E1 = E1 / 10;
                    G1 = E1 % 10;
                    if (G1 != G2) break;
                }
                if (i == 6)
                    printf("%d * %d = %d\n", F, A, E);
            }
}
```

</div>

</div>

<div v-click class="mt-4 text-center text-sm opacity-80">
枚举规模：7 × 10 × 10 = <span class="font-bold">700</span>
</div>

---

# 乘法数字问题：算法设计2

## 更强的结构约束 —— 逆推法

<div class="my-4 text-center text-lg">

$$DDDDDD \div A = ABCAB$$

</div>

<div class="grid grid-cols-2 gap-4">

<div>

### 枚举范围
- $A$: $3 \sim 9$
- $D$: $1 \sim 9$

### 思路
构造六位数 $E$，判断是否可被 $A$ 整除，再检查商 $F$ 是否满足重复结构。

</div>

<div v-click>

```c
main() {
    int A, B, C, D, E, F;
    for (A = 3; A <= 9; A++)
        for (D = 1; D <= 9; D++) {
            E = D*100000 + D*10000 + D*1000
              + D*100 + D*10 + D;
            if (E % A == 0) {
                F = E / A;
                if (F/10000 == A &&
                    (F%100)/10 == A &&
                    (F/1000)%10 == F%10)
                    printf("%d * %d = %d\n", F, A, E);
            }
        }
}
```

</div>

</div>

<div v-click class="mt-4 text-center text-sm opacity-80">
枚举规模：7 × 9 = <span class="font-bold">63</span> &nbsp;(比算法1降低了约10倍!)
</div>

---

# 4.2.2 其他范例

## 例1：狱吏问题

<div class="mt-4">

某王国对监狱实行大赦，让狱吏通过牢房 $n$ 次：

</div>

<div v-click class="mt-4 space-y-2">

- **第 1 次**：转动所有门锁
- **第 2 次**：每隔 1 间转动一次
- **第 k 次**：从第 $k$ 间开始，每隔 $k-1$ 间转动一次

</div>

<div v-click class="mt-6 border-l-4 border-amber-400 pl-4 font-bold">
问题：最后哪些牢房的锁是打开的？
</div>

---

# 狱吏问题：算法设计1

## 模拟开关锁过程

<div class="mb-4 text-sm opacity-80">
规则：<code>a[i] = 1</code> 表示锁上，<code>a[i] = 0</code> 表示打开，开关锁：<code>a[i] = 1 - a[i]</code>
</div>

```c
main() {
    int s, i, j, n;
    input(n);
    for (i = 1; i <= n; i++) {
        s = 1;
        for (j = 2; j <= i; j++)
            if (i % j == 0)
                s = s + 1;
        if (s % 2 == 1)
            printf("%d is free.\n", i);
    }
}
```

<div v-click class="mt-2 text-sm opacity-75">

时间复杂度：$O(n^2)$

</div>

---

# 狱吏问题：算法设计2

## 因子个数分析

<div class="space-y-3 mt-6">

- 第一次转动的是编号为 **1 的倍数**的锁
- 第二次转动的是编号为 **2 的倍数**的锁
- 第三次转动的是编号为 **3 的倍数**的锁

</div>

<div v-click class="mt-8 border-l-4 border-blue-400 pl-4">

**核心转化：某个编号的牢房被转动的次数 = 它的因子个数**

</div>

<div v-click class="mt-4 text-center">

| 因子个数 | 锁的状态 |
|:---:|:---:|
| 奇数 | 🔓 打开 |
| 偶数 | 🔒 关闭 |

</div>

---

# 狱吏问题：算法设计3

## 数学结论

<div class="flex justify-center my-4">

| $n$ | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| $d(n)$ | 1 | 2 | 2 | 3 | 2 | 4 | 2 | 4 | 3 | 4 | 2 | 6 | 2 | 4 |

</div>

<div v-click class="mt-4">

**数学模型：** 只有当 $n$ 是完全平方数时，$d(n)$ 才为奇数

<div class="mt-2 text-sm opacity-80">
一般因子成对出现，只有 $n = a^2$ 时会多出一个未配对因子 $a$
</div>

</div>

<div v-click class="mt-6 border-l-4 border-green-400 pl-4">

**复杂度**：$O(\sqrt{n})$

</div>

---

# 总结

## 蛮力法特点

<div class="grid grid-cols-2 gap-8 mt-8">

<div>
<div class="text-xl font-bold text-green-600 mb-4">✅ 优点</div>

- 思路直接
- 易于理解和实现
- 对很多小规模问题有效

</div>

<div>
<div class="text-xl font-bold text-red-600 mb-4">❌ 缺点</div>

- 通常效率较低
- 解空间大时耗时显著
- 往往需要结合约束剪枝来优化

</div>

</div>

<div v-click class="mt-8 border-l-4 border-blue-400 pl-4">

### 核心归纳

- 枚举法是蛮力法的重要形式
- 对问题建模和约束分析非常关键
- **改进枚举策略能显著降低计算量**（从 66000 → 660 → 63）

</div>

---

# 谢谢观看

<div class="mt-4 text-xl opacity-80">
算法设计与分析<br/>
第4章 基本的算法策略<br/>
4.2 蛮力法
</div>

<style>
/* 所有幻灯片留出顶部(避开logo)和底部(避开控制栏) */
.slidev-layout {
  padding-top: 3.5rem !important;
  padding-bottom: 3rem !important;
}

/* 代码块区域底部加额外间距，避免深色区域贴近底部控制栏 */
.slidev-code {
  margin-bottom: 1.5rem !important;
}
</style>