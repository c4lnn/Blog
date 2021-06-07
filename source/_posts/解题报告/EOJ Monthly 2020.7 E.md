---
title: EOJ Monthly 2020.7 E. 因数串
date: 2021-06-07 14:57:46
tags: [搜索]
categories: 解题报告
mathjax: true
---

# 链接

<https://acm.ecnu.edu.cn/contest/292/problem/E/>

# 题意

由正整数 $a$ 的所有因数构成一个数列，需要满足从数列的第 $2$ 个数开始，每个数都必须由其前一个数乘以某个质数或除以某个质数得到，每个因数只能使用一次

<!--more-->

# 思路

DFS

# 代码

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N=30;
int n,k[N],st[N];
long long p[N],x;
void dfs(int cur) {
    if(cur==n+1) {
        printf("%lld\n",x);
        return;
    }
    dfs(cur+1);
    if(!st[cur]) for(int i=1;i<=k[cur];i++) x*=p[cur],dfs(cur+1);
    else for(int i=1;i<=k[cur];i++) x/=p[cur],dfs(cur+1);
    st[cur]^=1;
}
int main() {
    scanf("%d",&n);
    for(int i=1;i<=n;i++) scanf("%lld%d",&p[i],&k[i]);
    x=1;
    dfs(1);
    return 0;
}
```