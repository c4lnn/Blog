---
title: LibreOJ 6014.「网络流 24 题」最长 k 可重区间集
date: 2021-04-16 21:53:01
tags: [最小费用最大流]
categories: 解题报告
mathjax: true
---

# 链接

<https://loj.ac/p/6014>

# 题意

$n$ 个开区间，求最长 $k$ 可重区间集。

<!--more-->

# 思路

设起点为 $s$，终点为 $t$，离散化后有 $m$ 个点。

对于每个区间，在 $l,r$ 之间连一条容量为 $1$，费用为 $l-r$ 的边；

在 $i,i+1$ 之间连一条容量为 $inf$，费用为 $0$ 的边；

在 $s,1$ 之间连一条容量为 $k$，费用为 $0$ 的边；

在 $m,t$ 之间连一条容量为 $k$，费用为 $0$ 的边。

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
// head
const int INF=0x3f3f3f3f;
const int N=1005;
const int M=4005;
int l[N],r[N],dist[N],h[N],preu[N],pree[M];
struct E {
    int v;
    int c,w;
    E(){}
    E(int v,int c,int w):v(v),c(c),w(w){}
};
vector<E> e;
VI g[N],b;
void add_edge(int u,int v,int c,int w) {
    e.EB(v,c,w);
    e.EB(u,0,-w);
    g[u].PB(SZ(e)-2);
    g[v].PB(SZ(e)-1);
}
bool dijkstra(int n,int s,int t) {
    for(int i=1;i<=n;i++) dist[i]=INF;
    priority_queue<PII,VPII,greater<PII>> q;
    dist[s]=0;
    q.emplace(0,s);
    while(SZ(q)) {
        int d=q.top().FI,u=q.top().SE;
        q.pop();
        if(dist[u]!=d) continue;
        for(auto x:g[u]) {
            int v=e[x].v,c=e[x].c,w=e[x].w;
            if(c>0&&dist[v]>dist[u]-h[v]+w+h[u]) {
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
        for(int i=1;i<=n;i++) h[i]=min(INF,h[i]+dist[i]);
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
    int n,k;
    cin>>n>>k;
    for(int i=1;i<=n;i++) {
        cin>>l[i]>>r[i];
        if(l[i]>r[i]) swap(l[i],r[i]);
        b.PB(l[i]),b.PB(r[i]);
    }
    sort(ALL(b));
    b.resize(unique(ALL(b))-b.begin());
    for(int i=1;i<=n;i++) {
        l[i]=lower_bound(ALL(b),l[i])-b.begin()+1;
        r[i]=lower_bound(ALL(b),r[i])-b.begin()+1;
        add_edge(l[i],r[i],1,-(b[r[i]-1]-b[l[i]-1]));
    }
    int s=SZ(b)+1,t=s+1;
    add_edge(s,1,k,0);
    add_edge(SZ(b),t,k,0);
    for(int i=1;i<SZ(b);i++) add_edge(i,i+1,INF,0);
    cout<<-mcmf(t,s,t).SE<<'\n';
    return 0;
}
```