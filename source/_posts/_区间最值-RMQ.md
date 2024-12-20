---
title: 区间最值 RMQ
author: Jarrycow
img: /medias/featureimages/xxx.png
cover: false
top: false
mathjax: true
categories:
  - 算法
tags:
  - 算法
keywords: 区间最值 RMQ
abbrlink: 区间最值 RMQ
date: 2023-08-01 15:59:43
---

RMQ 是英文 Range Maximum/Minimum Query 的缩写，表示依次寻找区间最值。

<!--more-->

# 递归分治

首先最容易想到的当然是递归，即寻找到最值的坐标，然后递归寻找其左右区间的最值。

```java

protected TreeNode findMax(int[] nums, int l, int r){
    if(l > r)    return null;  // 左边大于右边终止
    int idx = l;  // 最值下标
    for(int i = 0; i < nums.length; ++i){
        if(nums[i] > nums[idx]){
            idx = i;
        }
    }
    TreeNode res = new TreeNode(nums[idx]);
    res.left = findMax(nums, l, idx - 1);  // 寻找左边
    res.right = findMax(nums, idx + 1, r);  // 寻找右边
    return imax;
}
```

**时间复杂度：**$O\left(n^2\right)$

**空间复杂度：**$O\left(1\right)$

# 线段树

```java
class Solution {
    class Node {
        int l, r, val;
        Node (int _l, int _r) {
            l = _l; r = _r;
        }
    }
    void build(int u, int l, int r) {
        tr[u] = new Node(l, r);
        if (l == r) return ;
        int mid = l + r >> 1;
        build(u << 1, l, mid);
        build(u << 1 | 1, mid + 1, r);
    }
    void update(int u, int x, int v) {
        if (tr[u].l == x && tr[u].r == x) {
            tr[u].val = Math.max(tr[u].val, v);
            return ;
        }
        int mid = tr[u].l + tr[u].r >> 1;
        if (x <= mid) update(u << 1, x, v);
        else update(u << 1 | 1, x, v);
        pushup(u);
    }
    int query(int u, int l, int r) {
        if (l <= tr[u].l && tr[u].r <= r) return tr[u].val;
        int mid = tr[u].l + tr[u].r >> 1, ans = 0;
        if (l <= mid) ans = query(u << 1, l, r);
        if (r > mid) ans = Math.max(ans, query(u << 1 | 1, l, r));
        return ans;
    }
    void pushup(int u) {
        tr[u].val = Math.max(tr[u << 1].val, tr[u << 1 | 1].val);
    }
    Node[] tr = new Node[4010];
    int[] hash = new int[1010];
    public TreeNode constructMaximumBinaryTree(int[] nums) {
        int n = nums.length;
        build(1, 1, n);
        for (int i = 0; i < n; i++) {
            hash[nums[i]] = i + 1;
            update(1, i + 1, nums[i]);
        }
        return dfs(nums, 1, n);
    }
    TreeNode dfs(int[] nums, int l, int r) {
        if (l > r) return null;
        int val = query(1, l, r), idx = hash[val];
        TreeNode ans = new TreeNode(val);
        ans.left = dfs(nums, l, idx - 1);
        ans.right = dfs(nums, idx + 1, r);
        return ans;
    }
}

作者：宫水三叶
链接：https://leetcode.cn/problems/maximum-binary-tree/solutions/1761712/by-ac_oier-s0wc/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

**时间复杂度：**$O(n\log n)$

**空间复杂度：**$O(n)$

# 单调栈

`nums` 中的任二节点所在构建树的水平截面上的位置仅由下标大小决定。不难想到可抽象为找最近元素问题，可使用单调栈求解。

可以从前向后处理所有 `nums[i]`，若栈顶元素存在且栈顶比当前值小，可确定栈顶元素可作为当前 `nums[i]` 对应节点的左节点。为了确保最终 `nums[i]` 的左节点为 `[0,i−1]` 范围的最大值，`[0,i−1]` 中的最大值最后出队，此时可知容器栈具有「单调递减」特性。

```java
public TreeNode constructMaximumBinaryTree(int[] nums) {
    Stack<TreeNode> stack = new Stack<>();  // 单调递减栈
    for (int num : nums) {
        TreeNode node = new TreeNode(num);
        while(!stack.isEmpty() && stack.peek().val < num)  // 栈非空且栈顶小于该数
            node.left = stack.pop();  // 取出栈中最大值给结点左侧
        if(!stack.isEmpty())  // 栈非空（栈顶大于结点）
            stack.peek().right = node;  // 栈顶右侧是结点
        stack.push(node);
    }
    return stack.peek();
}
```

**时间复杂度：**$O(n)$

**空间复杂度：**$O(n)$

# ST 表





# 参考文献

[【宫水三叶】一题三解 :「递归分治」&「线段树」&「单调栈」](https://leetcode.cn/problems/maximum-binary-tree/solutions/1761712/by-ac_oier-s0wc/)