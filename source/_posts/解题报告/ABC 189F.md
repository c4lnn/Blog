---
title: AtCoder Beginner Contest 189 F. Sugoroku2
date: 2021-05-06 17:07:49
tags: [期望 DP]
categories: 解题报告
mathjax: true
---

# 链接

<https://atcoder.jp/contests/abc189/tasks/abc189_f>

# 题意

需要从 $0$ 走到 $n$，有一个转盘，转盘上有 $1\sim m$，转到哪个数走几步。

当走到 $a_i$，你要退回到 $0$，总共有 $k$ 个这样的点。

当到达 $n$ 或超过 $n$，则获胜。

问转动转盘直到获胜的期望次数。

<!--more-->

# 思路

记 $dp_i$ 为从 $i$ 到达 $n$ 或超过 $n$ 的期望次数
$dp_i=
\begin{cases}
0 & {i>=n} \\
dp_0 & {i \in a} \\
1+\frac{\sum_{j=1}^{m}{dp_{i+j}}}{m} & {else}
\end{cases}$

倒推，将 $dp_i$ 化为 $dp_i=kdp_0+b$ 的形式。

最后求出 $dp_0$。

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
const int N=1e5+5;
int n,m,k;
bool st[N];
pair<DB,DB> f[N],dp[N<<1];
int main() {
    //freopen("E:/OneDrive/IO/in.txt","r",stdin);
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cin>>n>>m>>k;
    for(int i=1;i<=k;i++) {
        int x;
        cin>>x;
        st[x]=true;
    }
    for(int i=n;~i;i--) {
        f[i].FI=f[i+1].FI-dp[i+1+m].FI+dp[i+1].FI;
        f[i].SE=f[i+1].SE-dp[i+1+m].SE+dp[i+1].SE;
        if(st[i]) dp[i]=MP(1,0);
        else if(i<n) dp[i].FI=f[i].FI/m,dp[i].SE=f[i].SE/m+1;
    }
    if(fabs(1-dp[0].FI)<1e-6) cout<<-1<<'\n';
    else cout<<fixed<<setprecision(4)<<dp[0].SE/(1-dp[0].FI)<<'\n';
    return 0;
}
```