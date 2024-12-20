---
title: 在撸代码时的 Java 基础代码
author: Jarrycow
img: /medias/featureimages/codeJava.jpg
cover: false
top: false
mathjax: true
categories:
  - Java
tags:
  - Java
  - 算法
keywords: 在撸代码时的 Java 基础代码
abbrlink: codeJava
date: 2021-01-30 01:23:58
---
<!--more-->

[toc]

# 基本结构

**整数最小值**

``Integer.MIN_VALUE``

**整数最大值**

``Integer.MAX_VALUE``

**求和**

``stream.sum()``

**取最大值**

``Math.max(num1, num2)``

# 数据结构

# 数组

**数组排序**

```java
Arrays.sort(nums);
```

**数组求和**

```java
Arrays.stream(arr).sum();
```

**数组切片**

```java
Arrays.copyOfRange(nums, ibegin, iend + 1);
```

**数组最值**

```java
Arrays.stream(nums).min().getAsInt()
```

**多维数组排序**

```java
Arrays.sort(nums, (a, b) -> a[0] - b[0]);
```

## 可变数组

**初始化**

```java
ArrayList<Integer> ans = new ArrayList<>();
```

**添加元素**

```java
ans.add(num);
```

**访问元素**

```java
ans.get(id);
```

**修改元素**

```java
ans.set(id, newNum);
```

**删除元素**

```java
ans.remove(id);
```

**数组长度**

```java
ans.size();
```

**排序**

```java
Collections.sort(ans);
```

# 链表

## 双链表

**初始化**

```java
LinkedList<Integer> nums = new LinkedList<>();
```

**元素个数**

```java
nums.size();
```

**链表中是否存在元素**

```java
nums.contaions(num);
```

**尾部添加**

```java
nums.add(num);
nums.addLast(num);
```

**头部添加**

```java
nums.addFirst(num);
```

**尾部删除**

```java
nums.removeLast();
```

**头部删除**

```java
nums.removeFirst();
```

# 列表

**列表初始化**

```java
List<List<Integer>> lis = Arrays.asList(
    Arrays.asList(0, 1),
    Arrays.asList(2, 3)
);
```

**列表内容获取**

```java
lis.get(index);
```

**数组转列表**

```java
Arrays.asList(arrays);
```



# 栈

**新建栈**

```java
Stack<Integer> st = new Stack<Integer>();
```

**入栈**

```java
st.push(x);
```

**出栈**

```java
st.pop();
```

**栈空**

```java
st.empty();
```

# 队列

**初始化队列**

```java
Queue<Integer> queue = new LinkedList<Integer>();
```

**入队**

```java
queue.offer(x);
```

**出队**

```java
queue.poll();
```

**取队首**

```java
queue.peek();
```

**遍历队列**

```java
for(Integer q : queue){
    ...
}
```

## 双端队列

**声明一个双端队列**

```java
Deque<String> deque = new ArrayDeque<>();
```

**队尾入栈**

```java
deque.addLast(ch);
deque.offerLast(ch);
```

**队尾出栈**

```java
deque.pollLast();
```

**取队尾元素**

```java
deque.peekLast()
```



## 堆（优先权队列）

**初始化小顶堆**

```java
Queue<Integer> heap = new PriorityQueue<>();
```

**初始化大顶堆**

```java
Queue<Integer> heap = new PriorityQueue<>((x, y) -> (y - x));
```

**添加元素**

```java
heap.add(num);
```

**出队元素**

小根堆默认递增出队

```java
heap.poll();
```

**队顶元素**

```java
heap.peek();
```

# 字符串

**字符定位**

```java
s.indexOf(ch);
```

**重复构造字符串 cnt 次**

```java
s.repeat(cnt)
```

**字符串替换**

```java
s.replace(old_str, new_str);
```

**字符串转换字符数组**

```java
s.toCharArray();
```

**字符转化为字符串**

```java
String.valueOf(ch)
```

**字符串分割**

```java
s.split(grex);
```

**字符串分割**

```java
s.substring(start, end);
```

**去除开头结尾空格**

```java
s.trim();
```

**数组凑成字符串**

```java
String.join(" ", wordList);
```

**字符串相等**

```java
s1.equals(s2)
```

`s1 == s2` 有些时候在本地跑的通，但力扣不一定

**字符串拼接**

```java
s1 + s2;
```

但是效率并不高，并不建议在 for 循环中使用。如果需要进行频繁的字符串拼接，推荐使用 `StringBuilder/StringBuffer`

## 可变字符串

在 Python 和 Java 等语言中，字符串都被设计成「不可变」的类型，即无法直接修改字符串的某一位字符，需要新建一个字符串实现。

**新建字符串**

```java
StringBuilder res = new StringBuilder();
```

**字符串拼接**

```java
res.append(ch);
```

`ch` 支持拼接字符、字符串、数字等类型

**字符串删除**

```java
res.delete(start, end);
res.deleteCharAt(idx);
```

**字符串替换**

```java
res.replace(start, end, newStr);
```

**转换字符串**

```java
res.toString();
```

# 哈希表

**新建哈希表**

`Map<Integer, Integer> hashtable = new HashMap<Integer, Integer>();`

**判断键值是否存在**

```
hashtable.containsKey(key);
```

**返回键所映射到的值**

```java
hashtable.get(key);
```
如果键不存在，返回默认值
```java
hashtable.getOrDefault(key, defaultValue)
```

**哈希表插入**

```java
hashtable.put(key, value);
```

**修改哈希表的值**

```java
hashtable.put(key, hashtable.getOrDefault(key, 0) + 1);
```

**哈希表删除**

```java
hashtable.remove(key);
```

**哈希表的清理**

```java
hashtable.clear();
```

**根据 Key 排序**

直接默认 `hashtable.keySet()` 是按照将 Key 转换为字符串的字典序

```java
List<String> keySet = new ArrayList<>(hashtable.keySet());
Collections.sort(keySet);
```

## 哈希集合

**新建集合**

```java
Set<Integer> hashset = new HashSet<Integer>();
```

**添加元素**

```java
hashset.add(num)
```

**批量添加元素**

```java
Set<String> hashset = new HashSet<>(Arrays.asList("a", "b"));
```

**删除元素**

```java
hashset.remove(num);
```

**元素存在**

```java
hashset.contains(num);
```

## 有序哈希表

有序哈希表中的键值对是 **按照插入顺序排序** 的。

**新建哈希表**

```java
Map<Character, Boolean> hashdic = new LinkedHashMap<>();
```

