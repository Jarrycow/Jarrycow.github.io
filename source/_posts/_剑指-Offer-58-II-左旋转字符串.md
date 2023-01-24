---
title: 剑指 Offer 58 - II. 左旋转字符串
author: Jarrycow
cover: false
top: false
mathjax: true
categories:
  - 力扣
tags:
  - 算法
  - 力扣
  - 字符串
keywords: 剑指 Offer 58 - II. 左旋转字符串
abbrlink: leetcode_Offer58
date: 2022-09-07 12:10:27
---



<!--more-->

# 题目描述

字符串的左旋转操作是把字符串前面的若干个字符转移到字符串的尾部。请定义一个函数实现字符串左旋转操作的功能。比如，输入字符串"abcdefg"和数字2，该函数将返回左旋转两位得到的结果"cdefgab"。

**示例 1：**

> 输入: s = "abcdefg", k = 2
> 输出: "cdefgab"

**示例 2：**

> 输入: s = "lrloseumgh", k = 6
> 输出: "umghlrlose"

**限制：**

- `1 <= k < s.length <= 10000`

# 题解

## 自己

```java
class Solution {
    public String reverseLeftWords(String s, int n) {
        String s0 = s.substring(0, n);
        String s1 = s.substring(n, s.length());
        return s1 + s0;
    }
}
```

## 官方

### 字符串切片

```java
class Solution {
    public String reverseLeftWords(String s, int n) {
        return s.substring(n, s.length()) + s.substring(0, n);
    }
}
```

时间复杂度：$O(n)$，实际效率最高

空间复杂度：$O(n)$

### 列表遍历拼接

1. 新建一个`StringBuilder(Java)`，记为`res`
2. 向`res`添加第`n+1`位至末尾的字符
3. 向`res`添加首位到`n`位字符
4. `res`转化为字符串

```java
class Solution {
    public String reverseLeftWords(String s, int n) {
        StringBuilder res = new StringBuilder();
        for(int i = n; i < s.length(); i++)
            res.append(s.charAt(i));
        for(int i = 0; i < n; i++)
            res.append(s.charAt(i));
        return res.toString();
    }
}
```

可以利用求余简化代码

```java
class Solution {
    public String reverseLeftWords(String s, int n) {
        StringBuilder res = new StringBuilder();
        for(int i = n; i < n + s.length(); i++)
            res.append(s.charAt(i % s.length()));
        return res.toString();
    }
}
```

时间复杂度：$O(n)$，`StringBuilder`为可变对象，最终添加新的字符元素**仅申请1次内存**

空间复杂度：$O(n)$

### 字符串遍历拼接

```java
class Solution {
    public String reverseLeftWords(String s, int n) {
        String res = "";
        for(int i = n; i < s.length(); i++)
            res += s.charAt(i);
        for(int i = 0; i < n; i++)
            res += s.charAt(i);
        return res;
    }
}
```

时间复杂度：$O(n)$，字符串为不可变对象，每轮拼接都要新建一个字符串，效率低下

空间复杂度：$O(n)$

# 知识点

**字符串按位置切片**

```s.substring(begin, end)```

**字符串拼接**

```str1 + str2```