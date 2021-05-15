---
title: The 2019 ICPC Asia Shanghai Regional Contest H. Tree Partition
date: 2021-04-19 18:41:48
tags: [二分,贪心]
categories: 解题报告
mathjax: true
---

# 链接

<https://ac.nowcoder.com/acm/contest/4370/H>

# 题意

将一个 $n$ 个节点的点权树切为 $k$ 个树，使每棵树的点权和的最大值最小。

<!--more-->

# 思路

二分答案。

记 $f_i$ 为节点 $i$ 与子树的权值和。

自底向上考虑，如果 $f_i$ 大于答案，则将子节点按照 $f$ 从大到小切，直到 $f_i$ 小于等于答案。

如果切了小于 $k$ 次，则该答案成立。

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
const int N=2e5+5;
int n,k,tot;
LL w[N],f[N];
VI g[N];
bool cmp(int a,int b) {
    return f[a]>f[b];
}
bool dfs(int u,int fa,LL mx) {
    f[u]=w[u];
    for(auto v:g[u]) {
        if(v==fa) continue;
        if(!dfs(v,u,mx)) return false;
        f[u]+=f[v];
    }
    if(f[u]<=mx) return true;
    sort(ALL(g[u]),cmp);
    for(auto v:g[u]) {
        if(v==fa) continue;
        if(++tot>k) return false;
        f[u]-=f[v];
        if(f[u]<=mx) return true;
    }
    return false;
}
bool check(LL mid) {
    tot=0;
    return dfs(1,0,mid);
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int T;
    cin>>T;
    for(int cs=1;cs<=T;cs++) {
        cin>>n>>k;
        --k;
        for(int i=1;i<=n;i++) g[i].clear();
        for(int i=1;i<n;i++) {
            int u,v;
            cin>>u>>v;
            g[u].PB(v);
            g[v].PB(u);
        }
        for(int i=1;i<=n;i++) cin>>w[i];
        LL l=0,r=1e14;
        while(l<r) {
            LL mid=l+r>>1;
            if(check(mid)) r=mid;
            else l=mid+1;
        }
        cout<<"Case #"<<cs<<": "<<l<<'\n';
    }
    return 0;
}
```