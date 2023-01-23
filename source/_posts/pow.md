---
title: pow
author: Jarrycow
cover: false
top: false
mathjax: true
categories:
  - 力扣
tags:
  - 算法
  - 数论
keywords: pow
abbrlink: pow
date: 2022-10-20 16:59:43
---



<!--more-->

# 题目描述

实现$x^n$

**示例 1：**

> 输入：x = 2.00000, n = 10
> 输出：1024.00000

**示例 2：**

> 输入：x = 2.10000, n = 3
> 输出：9.26100

**示例 3：**

> 输入：x = 2.00000, n = -2
> 输出：0.25000
> 解释：2-2 = 1/22 = 1/4 = 0.25

**提示：**

- `-100.0 < x < 100.0`
- `-2^{31} <= n <= 2^{31}-1`
- `-10^4 <= xn <= 10^4`

# 题解

最简单的其实直接调Math库，不过应该不会这么简单

## 我的

```java
class Solution {
    public double myPow(double x, int n) {
        double result = Math.pow(x,n);
        return result;
    }
}
```

## 官方：快速幂

时间复杂度$O(\log_2n)$计算$x^n$

每一步将$n$对半分进行平方计算

### 递归快速幂

$$
a^n=\left\{\begin{matrix}
a^{n-1}\cdot a & n\%2\ne0\\
a^{\frac n2\cdot a^{\frac n2}}&   n\%2=0 & \text{AND} & n\ne 0\\
1 & n=0
\end{matrix}\right.
$$

```java
class Solution {
    public double myPow(double x, int n) {
        long N = n;
        return N >= 0 ? quickMul(x, N) : 1.0 / quickMul(x, -N);  // 负数取倒数
    }

    public double quickMul(double x, long N) {
        if (N == 0) {
            return 1.0;
        }
        double y = quickMul(x, N / 2);
        return N % 2 == 0 ? y * y : y * y * x;
    }
}
```

### 迭代快速幂

```java
class Solution {
    public double myPow(double x, int n) {
        long N = n;
        return N >= 0 ? quickMul(x, N) : 1.0 / quickMul(x, -N);
    }

    public double quickMul(double x, long N) {
        double ans = 1.0;
        // 贡献的初始值为 x
        double x_contribute = x;
        // 在对 N 进行二进制拆分的同时计算答案
        while (N > 0) {
            if (N % 2 == 1) {
                // 如果 N 二进制表示的最低位为 1，那么需要计入贡献
                ans *= x_contribute;
            }
            // 将贡献不断地平方
            x_contribute *= x_contribute;
            // 舍弃 N 二进制表示的最低位，这样我们每次只要判断最低位即可
            N /= 2;
        }
        return ans;
    }
}
```

