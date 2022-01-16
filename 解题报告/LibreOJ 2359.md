---
title: LibreOJ 2359. 天天爱跑步
date: 2021-06-10 16:01:50
tags: [树上差分,LCA]
categories: 解题报告
mathjax: true
---

# 链接

<https://loj.ac/p/2359>

# 题意

树形图上有 $m$ 个玩家，从 $s$ 走到 $t$

每个节点都有一个观察员

节点 $x$ 的观察员在第 $w$ 秒观察玩家，若某个玩家在第 $w$ 秒正好走到节点 $x$，则这个玩家被节点 $x$ 的观察员观察到

求所有节点的观察员能够观察到的玩家的总数

<!--more-->

# 思路

树上差分

每个玩家的路径可以分为两部分：$s \sim lca(s,t)$ 和 $lca(s,t) \sim t$，后者不包括 $lca(s,t)$

记 $d$ 数组为每个节点的深度

对于 $s \sim lca(s,t)$ 上的节点 $x$ ：当 $d[s]=w[x]+d[x]$ 时，$x$ 能够观察到这个玩家

对于 $lca(s,t) \sim t$ 上的节点 $x$ ：当 $d[s]-2*d[lca(s,t)]=w[x]-d[x]$ 时，$x$ 能够观察到这个玩家

这两个条件所在路径并不重叠，因此可以分开计算

以第一个为例：对于玩家 $i$ 让$s \sim lca(s,t)$的路径上的每个点都拥有一个类型为 $d[s]$ 的物品，因此对于每个点我们只需要查询它拥有多少个 $w[x]-d[x]$ 的物品就行

通过树上差分，我们在节点s产生一个 $d[s]$ 的物品，在 $fa[lca(s,t)]$ 减去一个 $d[s]$ 物品，我们对每个节点建立一个 $vector$ 记录增加和减去的物品，并且因为对于每个节点我们只需要知道特定物品的个数，所以我们再维护一个全局计数数组，利用区间减法得到子树和

对于第二段路径同理,但是物品是在 $lca(s,t)$ 消失，并且 $w[x]-d[x]$ 可能为负，要将范围平移 $n$

# 代码

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N=3e5+5;
int n,m;
int w[N],s[N],t[N];
int cnt,to[N*2],nxt[N*2],head[N];
int fa[N],sz[N],d[N],top[N],son[N];
vector<pair<int,int> >p[N];
int c[N*2];
int ans[N];
void addedge(int u,int v) {
    cnt++;
    to[cnt]=v;
    nxt[cnt]=head[u];
    head[u]=cnt;
}
void dfs1(int u,int t) {
    d[u]=t;
    sz[u]=1;
    for(int i=head[u];i;i=nxt[i]) {
        int v=to[i];
        if(v==fa[u]) continue;
        fa[v]=u;
        dfs1(v,t+1);
        sz[u]+=sz[v];
        if(sz[v]>sz[son[u]]) son[u]=v;
    }
}
void dfs2(int u,int t) {
    top[u]=t;
    if(!son[u]) return;
    dfs2(son[u],t);
    for(int i=head[u];i;i=nxt[i]) {
        int v=to[i];
        if(v==son[u]||v==fa[u]) continue;
        dfs2(v,v);
    }
}
int lca(int a,int b) {
    while(top[a]!=top[b]) {
        if(d[top[a]]>d[top[b]]) a=fa[top[a]];
        else b=fa[top[b]];
    }
    return d[a]<d[b]?a:b;
}
void dif1() {
    for(int i=1;i<=m;i++) {
        int LCA=lca(s[i],t[i]);
        p[s[i]].push_back({d[s[i]],1});
        p[fa[LCA]].push_back({d[s[i]],-1});
    }
}
void dif2() {
    for(int i=1;i<=m;i++) {
        int LCA=lca(s[i],t[i]);
        p[t[i]].push_back({d[s[i]]-2*d[LCA],1});
        p[LCA].push_back({d[s[i]]-2*d[LCA],-1});
    }
}
void dfs_solve1(int u) {
    int tot=c[w[u]+d[u]];
    for(int i=head[u];i;i=nxt[i]) {
        int v=to[i];
        if(v==fa[u]) continue;
        dfs_solve1(v);
    }
    for(auto x:p[u]) c[x.first]+=x.second;
    ans[u]+=c[w[u]+d[u]]-tot;
}
void dfs_solve2(int u) {
    int tot=c[w[u]-d[u]+n];
    for(int i=head[u];i;i=nxt[i]) {
        int v=to[i];
        if(v==fa[u]) continue;
        dfs_solve2(v);
    }
    for(auto x:p[u]) c[x.first+n]+=x.second;
    ans[u]+=c[w[u]-d[u]+n]-tot;
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    cin>>n>>m;
    for(int i=1;i<n;i++) {
        int u,v;
        cin>>u>>v;
        addedge(u,v);
        addedge(v,u);
    }
    dfs1(1,1);
    dfs2(1,1);
    for(int i=1;i<=n;i++) cin>>w[i];
    for(int i=1;i<=m;i++) cin>>s[i]>>t[i];
    dif1();
    dfs_solve1(1);
    for(int i=1;i<=n;i++) p[i].clear();
    memset(c,0,sizeof c);
    dif2();
    dfs_solve2(1);
    for(int i=1;i<=n;i++) cout<<ans[i]<<" ";
    cout<<endl;
    return 0;
}
```