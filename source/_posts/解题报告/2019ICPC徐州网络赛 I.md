---
title: 2019ICPC徐州网络赛 I. query
date: 2021-05-12 17:52:02
tags: [二维偏序,树状数组]
categories: 解题报告
mathjax: true
---

# 链接

<https://nanti.jisuanke.com/t/41391>

# 题意

给出 $n$ 的排列，$m$ 个询问，求 $[l,r]$ 区间内满足 $l\le i<j\le r,\min(p_i,p_j)=\gcd(p_i,p_j)$ 的数对 $(i,j)$ 的数量。

<!--more-->

# 思路

$\min(p_i,p_j)=\gcd(p_i,p_j)$ 即两者较大数是两者较小数的倍数。

建立两个数组 $pos1,pos2$：

- $pos1_i$ 存 $j>i,p_i\mid p_j$ 的下标 $j$。
- $pos2_i$ 存 $j>i,p_j\mid p_i$ 的下标 $j$。

对于满足要求的数对 $(i,j)$，$c_k$ 记录以 $k$ 为 $j$ 的数对个数，用树状数组维护。


将询问按左端点为第一关键字，右端点为第二关键字排序。

对于第 $q$ 次询问，将 $(i,j)$ ($i\in[l_{q-1},l_q))$ 的所有数对删去，前缀和就是该次询问答案。

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
const int N=1e5+5;
int n,m,a[N],b[N],c[N],res[N];
VI pos[2][N];
struct O {
    int l,r,id;
    bool operator < (const O &T) const {
        return l<T.l||l==T.l&&r<T.r;
    }
}o[N];
int lowbit(int x) {
    return x&-x;
}
void update(int x,int v) {
    for(int i=x;i<=n;i+=lowbit(i)) c[i]+=v;
}
int query(int x) {
    int ret=0;
    for(int i=x;i>=1;i-=lowbit(i)) ret+=c[i];
    return ret;
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cin>>n>>m;
    for(int i=1;i<=n;i++) {
        cin>>a[i];
        b[a[i]]=i;
    }
    for(int i=1;i<=n;i++)
        for(int j=a[i]*2;j<=n;j+=a[i])
            if(b[j]>i) pos[0][i].PB(b[j]);
            else pos[1][b[j]].PB(i);
    for(int i=1;i<=n;i++)
        for(int j=a[i]*2;j<=n;j+=a[i])
            if(b[j]>i) update(b[j],1);
            else update(i,1);
    for(int i=1;i<=m;i++) {
        cin>>o[i].l>>o[i].r;
        o[i].id=i;
    }
    sort(o+1,o+1+m);
    int l=1;
    for(int i=1;i<=m;i++) {
        while(l<o[i].l) {
            for(auto x:pos[0][l]) update(x,-1);
            for(auto x:pos[1][l]) update(x,-1);
            ++l;
        }
        res[o[i].id]=query(o[i].r);
    }
    for(int i=1;i<=m;i++) cout<<res[i]<<'\n';
    return 0;
}
```