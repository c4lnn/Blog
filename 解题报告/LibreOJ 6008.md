---
title: LibreOJ 6008.「网络流 24 题」餐巾计划
date: 2021-04-17 23:43:11
tags: [最小费用最大流]
categories: 解题报告
mathjax: true
---

# 链接

<https://loj.ac/p/6008>

# 题意

一个餐厅在相继的 $n$ 天里，每天需用的餐巾数不尽相同。假设第 $i$ 天需要 $r_i$ 块餐巾，餐厅可以购买新的餐巾，每块餐巾的费用为 $P$ 分，或者把旧餐巾送到快洗部，洗一块需 $M$ 天，其费用为 $F$ 分，或者送到慢洗部，洗一块需 $N$ 天，其费用为 $S$ 分 $(S<F)$。

每天结束时，餐厅必须决定将多少块脏的餐巾送到快洗部，多少块餐巾送到慢洗部，以及多少块保存起来延期送洗。但是每天洗好的餐巾和购买的新餐巾数之和，要满足当天的需求量。

试设计一个算法为餐厅合理地安排好 $n$ 天中餐巾使用计划,使总的花费最小。

<!--more-->

# 思路

最小费用最大流。

记起点为 $s$，终点为 $t$，将一天分为两个点 $u_i,v_i$。

$s$ 与 $u_{1 \sim n}$ 连一条容量无限，费用 $P$ 的边，表示每日购买新餐巾数量；

$u_{1 \sim n}$ 与 $t$ 连一条容量 $r_{1 \sim n}$，费用 $0$ 的边，表示每日使用餐巾数量；

$s$ 与 $v_{1 \sim n}$ 连一条容量 $r_{1 \sim n}$，费用 $0$ 的边，表示每日回收旧餐巾数量；

$v_i$ 与 $v_{i+1}$ 连一条容量无限，费用 $0$ 的边，表示每日回收的旧餐巾可以延后清洗；

$v_i$ 与 $u_{i+M}$ 连一条容量无限，费用 $F$ 的边，表示 第 $i+M$ 天收到快洗部的旧餐巾数量；

$v_i$ 与 $u_{i+N}$ 连一条容量无限，费用 $S$ 的边，表示 第 $i+N$ 天收到慢洗部的旧餐巾数量。

# 代码

```cpp
#include <bits/stdc++.h>
#define SZ(x) (int)(x).size()
#define ALL(x) (x).begin(),(x).end()
#define PB push_back
#define EB emplace_back
#define MP make_pair
#define FI first
#define SE second
using namespace std;
typedef double DB;
typedef long double LD;
typedef long long LL;
typedef unsigned long long ULL;
typedef pair<int,int> PII;
typedef vector<int> VI;
typedef vector<PII> VPII;
//head
const int N=2005;
const int M=25005;
const int INF=0x3f3f3f3f;
int a[N],dist[N],h[N],preu[N],pree[M];
bool st[N];
struct E {
    int v;
    int c,w;
    E(){}
    E(int v,int c,int w):v(v),c(c),w(w){}
};
vector<E> e;
VI g[N];
void add_edge(int u,int v,int c,int w) {
    e.EB(v,c,w);
    e.EB(u,0,-w);
    g[u].PB(e.size()-2);
    g[v].PB(e.size()-1);
}
bool dijkstra(int n,int s,int t) {
    for(int i=1;i<=n;i++) dist[i]=INF,st[i]=false;
    priority_queue<PII,VPII,greater<PII>> q;
    dist[s]=0;
    q.emplace(0,s);
    while(SZ(q)) {
        int u=q.top().SE;
        q.pop();
        if(st[u]) continue;
        st[u]=true;
        for(auto x:g[u]) {
            int v=e[x].v,c=e[x].c,w=e[x].w;
            if(!st[v]&&c>0&&dist[v]>dist[u]-h[v]+w+h[u]) {
                dist[v]=dist[u]-h[v]+w+h[u];
                preu[v]=u;
                pree[v]=x;
                q.emplace(dist[v],v);
            }
        }
    }
    return dist[t]!=INF;
}
PII mcmf(int n,int s,int t) {
    int flow=0,cost=0;
    while(dijkstra(n,s,t)) {
        int c=INF;
        for(int i=1;i<=n;i++) h[i]+=dist[i];
        for(int u=t;u!=s;u=preu[u]) c=min(c,e[pree[u]].c);
        flow+=c;
        cost+=c*h[t];
        for(int u=t;u!=s;u=preu[u]) {
            e[pree[u]].c-=c;
            e[pree[u]^1].c+=c;
        }
    }
    return MP(flow,cost);
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int n,p,t1,c1,t2,c2;
    cin>>n>>p>>t1>>c1>>t2>>c2;
    for(int i=1;i<=n;i++) cin>>a[i];
    int s=n+n+1,t=s+1;
    for(int i=1;i<=n;i++) add_edge(s,i,a[i],0);
    for(int i=1;i<=n;i++) add_edge(s,i+n,INF,p);
    for(int i=1;i<=n;i++) add_edge(i+n,t,a[i],0);
    for(int i=1;i<n;i++) add_edge(i,i+1,INF,0);
    for(int i=1;i+t1<=n;i++) add_edge(i,i+n+t1,INF,c1);
    for(int i=1;i+t2<=n;i++) add_edge(i,i+n+t2,INF,c2);
    cout<<mcmf(t,s,t).SE<<'\n';
    return 0;
}
```