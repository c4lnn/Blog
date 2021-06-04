---
title: LibreOJ 2594. 关押罪犯
date: 2021-05-30 13:23:42
tags: [二分,二分图判定]
categories: 解题报告
mathjax: true
---

# 链接

<https://loj.ac/p/2594>

# 题意

$n$ 个点，$m$ 条无向边，将所有点分成两个集合，使两个集合里的边权最大值最小

<!--more-->

# 思路

二分边权

将边权大于 $mid$ 的边加入图中判定是否为二分图

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
typedef pair<int,int> PII;
typedef vector<int> VI;
typedef vector<PII> VPII;
//head
const int N=20005;
int n,m,a[N*5],b[N*5],c[N*5],col[N];
bool st[N];
VI g[N];
bool dfs(int u,int t) {
    col[u]=t;
    for(auto v:g[u]) {
        if(col[v]==3-t) continue;
        if(col[v]==t||!dfs(v,3-t)) return false;
    }
    return true;
}
bool check(int mid) {
    for(int i=1;i<=n;i++) g[i].clear(),col[i]=0;
    for(int i=1;i<=m;i++) {
        if(c[i]<=mid) continue;
        g[a[i]].PB(b[i]);
        g[b[i]].PB(a[i]);
    }
    for(int i=1;i<=n;i++) if(!col[i]&&!dfs(i,1)) return false;
    return true;
}
int main() {
    scanf("%d%d",&n,&m);
    for(int i=1;i<=m;i++) scanf("%d%d%d",&a[i],&b[i],&c[i]);
    int l=0,r=1000000000;
    while(l<r) {
        int mid=l+r>>1;
        if(check(mid)) r=mid;
        else l=mid+1;
    }
    printf("%d\n",l);
    return 0;
}
```