---
title: 2019ICPC上海网络赛 D. Counting Sequences I
date: 2021-06-07 15:15:24
tags: [搜索]
categories: 解题报告
mathjax: true
---

# 链接

<https://nanti.jisuanke.com/t/41412>

# 题意

求长度为 $n$ 的序列，序列的和与积相等的方案数

<!--more-->

# 思路

DFS

将序列分为 $1$ 和 $!1$

已知 $\{!1\}_{min}$为 $2$

先考虑 $!1$ 的个数，取最小值 $2$，$2^{12}>3000$ 因此至多取 $11$ 个

再考虑 $!1$ 最大值

易得：序列中 $1$ 越多，${!1}_{max}$ 越大，且序列中至少有 $2$ 个$!1$，至多有 $2998$ 个 $1$

$(\{!1\}_{max}*2)-(\{!1\}_{max}+2)=2998\longrightarrow \{!1\}_{max}=3000$

计算 $ans$ 时 有重复元素的排列组合 ：如$1112233$，$res=\frac{7!}{3!*2!*2!}$

# 代码

```cpp
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int mod=1e9+7;
ll fac[3001],infac[3001],ans[3001],a[3001];
int v[3001];
ll qPow(ll n,int a) {
    ll res=1;
    while(a) {
        if(a&1) res=res*n%mod;
        n=n*n%mod;
        a>>=1;
    }
    return res;
}
void calc(int cur,int cnt) {
    for(int i=1;i<=cur;i++) v[a[i]]++;
    int res=fac[cnt];
    for(int i=1;i<=3000;i++) if(v[i]>1) res=res*infac[v[i]]%mod;
    ans[cnt]=(ans[cnt]+res)%mod;
}
void dfs(int cur,int start,int idx,ll mul,ll sum,ll cnt)  {
    if(cur==0) {
        memset(v,0,sizeof v);
        v[1]=mul-sum;
        calc(cnt,mul-sum+cnt);
        return;
    }
    for(int i=start;i<=3000;i++) {
        if(mul*i-sum-i>3000-cnt) return;
        a[idx]=i;
        dfs(cur-1,i,idx+1,mul*i,sum+i,cnt);
    }
}
int main() {
    memset(ans,0,sizeof ans);
    fac[0]=infac[0]=1;
    for(ll i=1;i<=3000;i++) fac[i]=fac[i-1]*i%mod,infac[i]=infac[i-1]*qPow(i,mod-2)%mod;
    for(int i=2;i<=11;i++) dfs(i,2,1,1,0,i);
    int T,n;
    scanf("%d",&T);
    while(T--) {
        scanf("%d",&n);
        printf("%d\n",ans[n]);
    }
    return 0;
}
```