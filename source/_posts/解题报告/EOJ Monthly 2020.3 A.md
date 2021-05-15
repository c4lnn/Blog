---
title: EOJ Monthly 2020.3 A. 迷宫
date: 2021-04-20 18:52:24
tags: [最大流]
categories: 解题报告
mathjax: true
---

# 链接

<https://acm.ecnu.edu.cn/contest/255/problem/A>

# 题意

有向图上每条边有每天最大通过人数，问至少几天所有人从 $s$ 走到 $t$。

<!--more-->

# 思路

建分层图，跑最大流。

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
typedef long long LL;
typedef unsigned long long ULL;
typedef pair<int,int> PII;
typedef vector<int> VI;
typedef vector<PII> VPII;
//head
const int N=5100;
int k,V,m,S,T,n,s,t,d[N],cur[N];
VPII e;
VI g[N];
struct Edge {
    int u,v,c;
    Edge() {}
    Edge(int u,int v,int c):u(u),v(v),c(c) {}
};
vector<Edge> E;
void init(int x) {
    e.clear();
    for(int i=1;i<=n;i++) g[i].clear();
}
void add_edge(int u,int v,int c) {
    e.EB(v,c);
    e.EB(u,0);
    g[u].EB(SZ(e)-2);
    g[v].EB(SZ(e)-1);
}
bool bfs() {
    for(int i=1;i<=n;i++) d[i]=0;
    queue<int> q;
    q.push(s);
    d[s]=1;
    while(SZ(q)) {
        int u=q.front();
        q.pop();
        for(auto x:g[u]) {
            int v=e[x].FI,c=e[x].SE;
            if(d[v]||c<=0) continue;
            d[v]=d[u]+1;
            q.push(v);
        }
    }
    return d[t];
}
int dfs(int u,int a) {
    if(u==t) return a;
    int flow=0,f;
    for(int &i=cur[u];i<SZ(g[u]);i++) {
        int v=e[g[u][i]].FI,&c=e[g[u][i]].SE;
        if(d[v]!=d[u]+1||c<=0||(f=dfs(v,min(a,c)))<=0) continue;
        c-=f;
        e[g[u][i]^1].SE+=f;
        a-=f;
        flow+=f;
        if(!a) break;
    }
    return flow;
}
int dinic() {
    int flow=0;
    while(bfs()) {
        for(int i=1;i<=n;i++) cur[i]=0;
        flow+=dfs(s,0x3f3f3f3f);
    }
    return flow;
}
bool check(int x) {
    init(x);
    for(int i=0;i<x;i++) {
        for(int j=0;j<m;j++) {
            add_edge(E[j].u+V*i,E[j].v+V*(i+1),E[j].c);
        }
    }
    s=V*(x+1)+1;
    for(int i=0;i<x;i++) add_edge(s,S+V*i,k);
    ++s;
    add_edge(s,s-1,k);
    t=s+1;
    for(int i=0;i<=x;i++) add_edge(T+V*i,t,k);
    n=t;
    return dinic()==k;
}
int solve() {
    int l=1,r=105;
    while(l<r) {
        int mid=l+r>>1;
        if(check(mid)) r=mid;
        else l=mid+1;
    }
    return l;
}
int main() {
    //freopen("E:/OneDrive/IO/in.txt","r",stdin);
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cin>>k>>V>>m>>S>>T;
    for(int i=1;i<=m;i++) {
        int u,v,c;
        cin>>u>>v>>c;
        E.EB(u,v,c);
    }
    cout<<solve()<<'\n';
    return 0;
}
```