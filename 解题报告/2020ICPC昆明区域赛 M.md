---
title: 2020ICPC昆明区域赛 M. Stone Games
date: 2021-04-22 14:32:28
tags: [主席树]
categories: 解题报告
mathjax: true
---

# 链接

<https://ac.nowcoder.com/acm/contest/12548/M>

# 题意

设有正整数多重集 $S$，定义 $f(s)$ 为最小的不能由 $S$ 中若干个数（可以为 $0$ 个数）相加得到的非负整数（每个元素至多被用一次）。

给定长度为 $n$ $(n \le 1e6)$ 的正整数序列 $a_1,\dots,a_n$ $(1 \le a_i \le 1e9)$，$q$ $(q \le 1e5)$ 次询问一个区间 $[l,r]$，求 $f(\{a_l,\dots,a_r\})$，强制在线。

<!--more-->

# 思路

对于升序序列 $A$，定义 $S(i)=\sum_{j=1}^{i}{A_j}$，求最小的 $i$ 满足 $S(i)+1<A_{i+1}$，那么 $f(A)=S(i)+1$。

假设我们考虑到 $i$ ，此时可以得到的和为 $[0,S(i)]$：

- 若 $A_{i+1} \le S(i)+1$，那么现在可以得到 $[0,S(i)+A_{i+1}]$；
- 若 $A_{i+1} > S(i)+1$，无法得到 $S(i)+1$。


因此从 $i=0,s=0$ 开始考虑，求出 $\sum_{j=1}^{A_j\le s+1} A_j$，若 $s=\sum_{j=1}^{A_j\le s+1} A_j$，说明 $A_i$ 后的数都大于 $s+1$，那么 $s+1$ 就是答案，否则让 $s=\sum_{j=1}^{A_j\le s+1} A_j$。

用主席树维护，每次 $s$ 都会至少增加一倍，因此每次询问的时间复杂度是 $2$ 个 $\log$。

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
const int N=1e6+5;
int cnt,rt[N],ls[N<<5],rs[N<<5];
LL a[N],sum[N<<5];
void update(int &p,int q,int l,int r,int x) {
    p=++cnt;
    ls[p]=ls[q],rs[p]=rs[q],sum[p]=sum[q]+x;
    if(l==r) return;
    int mid=l+r>>1;
    if(x<=mid) update(ls[p],ls[q],l,mid,x);
    else update(rs[p],rs[q],mid+1,r,x);
}
LL query(int p,int q,int L,int R,LL x) {
    if(R<=x) return sum[q]-sum[p];
    int mid=L+R>>1;
    LL ret=query(ls[p],ls[q],L,mid,x);
    if(x>mid) ret+=query(rs[p],rs[q],mid+1,R,x);
    return ret;
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int n,q;cin>>n>>q;
    for(int i=1;i<=n;i++) cin>>a[i];
    for(int i=1;i<=n;i++) update(rt[i],rt[i-1],1,1e9,a[i]);
    LL ans=0;
    for(int i=1;i<=q;i++) {
        int l,r;cin>>l>>r;
        PII x=minmax((l+ans)%n+1,(r+ans)%n+1);
        LL S=0;
        for(;;) {
            LL ret=query(rt[x.FI-1],rt[x.SE],1,1e9,S+1);
            if(ret==S) break;
            S=ret;
        }
        cout<<(ans=S+1)<<'\n';
    }
    return 0;
}
```