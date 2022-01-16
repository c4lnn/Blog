---
title: 2019CCPC秦皇岛 F. Forest Program
date: 2021-06-08 00:10:03
tags: [图的连通性]
categories: 解题报告
mathjax: true
---

# 链接

<http://acm.hdu.edu.cn/showproblem.php?pid=6736>

# 题意

在仙人掌图中删去一些边使图中没有环，求方案数

<!--more-->

# 思路

tarjan 算法找环

设图中环的大小分别为 $c_1$ $c_2$, ..., $c_k$，不属于任何一个环的边数为 $b$，则答案为：

$2^b*\prod _{i=1}^{k}{(2^{c_i}-1)}$

# 代码

```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N=3e6+5;
const int M=5e7+5;
const int mod=998244353;
int n,m,cnt,to[M],nxt[M],head[N],st[N];
ll ans;
void init() {
    ans=1;
    memset(st,0,sizeof st);
    cnt=0;
    memset(head,0,sizeof head);
}
void addedge(int a,int b) {
    cnt++;
    to[cnt]=b;
    nxt[cnt]=head[a];
    head[a]=cnt;
}
ll ksm(ll n,int a) {
    ll res=1;
    while(a) {
        if(a&1) res=res*n%mod;
        n=n*n%mod;
        a>>=1;
    }
    return res;
}
void dfs(int u,int pre) {
    st[u]=st[pre]+1;
    for(int i=head[u];i;i=nxt[i]) {
        int v=to[i];
        if(!st[v]) dfs(v,u);
        else if(st[v]>0&&v!=pre) {
            m-=st[u]-st[v]+1;
            ans=ans*(ksm(2,st[u]-st[v]+1)-1+mod)%mod;
        }
    }
    st[u]=-1;
}
int main() {
    int a,b;
    while(~scanf("%d%d",&n,&m)) {
        init();
        for(int i=0;i<m;i++) {
            scanf("%d%d",&a,&b);
            addedge(a,b);
            addedge(b,a);
        }
        for(int i=1;i<=n;i++) if(!st[i]) dfs(i,i);
        printf("%lld\n",ans*ksm(2,m)%mod);
    }
    return 0;
}
```