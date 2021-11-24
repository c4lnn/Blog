---
title: The 2021 ICPC Asia Jinan Regional Contest
date: 2021-11-17 02:29:59
tags: [组合数学]
categories: 套题
mathjax: true
---

| Solved |   A   |   B   |   C   |   D   |   E   |   F   |   G   |   H   |   I   |   J   |   K   |   L   |   M   |
| :----: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
|  2/13  |   -   |   -   |   Ø   |   -   |   -   |   -   |   -   |   -   |   -   |   O   |   -   |   -   |   -   |

# 题目链接

<https://pintia.cn/problem-sets/1459829212832296960/problems/type/7>

<!--more-->

# Problem C. Optimal Strategy

**题意：**

有 $n$ 件物品，第 $i$ 件的价值为 $a_i$。

A 和 B 轮流取物品，A 先手。

每个人都要最大化自己取到的物品的价值和，求有多少种可能的游戏过程。

**思路：**

记价值为 $i$ 的物品有 $cnt_i$ 个。

当剩余物品中最大价值为 $x$ 时：

- 当 $cnt_x$ 为偶数时，当一个人取了 $x$ 后，下一个回合另一个人也必须取 $x$，下下回合可以随意取，这样最终每个人都能拿到 $\frac {cnt_x} {2}$ 个 $x$。
- 当 $cnt_x$ 为奇数时，在一开始取走一个 $x$，转化为偶数情况。

按照物品价值从小到大考虑，记价值 $\le i$ 的物品有 $f_i$ 种游戏过程：

$f_i=f_{i-1}*cnt_i!*\binom{\lfloor\frac{cnt_i}{2}\rfloor+\sum_{j=1}^{i-1}{cnt_j}}{\lfloor\frac{cnt_i}{2}\rfloor}$。

**代码：**

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
typedef vector<LL> VLL;
typedef vector<PII> VPII;
// head
const int N=1e6+5;
const LL MOD=998244353;
int cnt[N];
LL fac[N],invfac[N],f[N],sum[N];
LL binom(int n,int m) {
    return fac[n]*invfac[m]%MOD*invfac[n-m]%MOD;
}
LL qpow(LL a,LL b) {
    LL ret=1;
    while(b) {
        if(b&1) ret=ret*a%MOD;
        a=a*a%MOD;
        b>>=1;
    }
    return ret;
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int n;cin>>n;
    fac[0]=1;
    for(int i=1;i<=n;i++) fac[i]=fac[i-1]*i%MOD;
    invfac[n]=qpow(fac[n],MOD-2);
    for(int i=n-1;~i;i--) invfac[i]=invfac[i+1]*(i+1)%MOD;
    for(int i=1;i<=n;i++) {
        int x;cin>>x;
        ++cnt[x];
    }
    for(int i=1;i<=n;i++) sum[i]=sum[i-1]+cnt[i];
    f[0]=1;
    for(int i=1;i<=n;i++) {
        if(cnt[i]) f[i]=f[i-1]*fac[cnt[i]]%MOD*binom(cnt[i]/2+sum[i-1],cnt[i]/2)%MOD;
        else f[i]=f[i-1];
    }
    cout<<f[n]<<'\n';
    return 0;
}
```

# Problem K. Search For Mafuyu

**题意：**

树形图，A 在点 $1$，B 可能在除了 $1$ 以外的任何点，概率是一样的。

A 要找到 B，如果 A 采取最优策略，求找到 B 的最小期望时间。

**思路：**

显然欧拉遍历所需步数是最少的。

**代码：**

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
typedef vector<LL> VLL;
typedef vector<PII> VPII;
// head
const int N=105;
VI g[N];
LL sum,step;
void dfs(int u,int fa) {
    for(auto v:g[u]) if(v!=fa) {
        ++step;
        sum+=step;
        dfs(v,u);
        ++step;
    }
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int T;cin>>T;
    while(T--) {
        int n;cin>>n;
        for(int i=1;i<=n;i++) g[i].clear();
        step=sum=0;
        for(int i=1;i<n;i++) {
            int u,v;cin>>u>>v;
            g[u].PB(v);
            g[v].PB(u);
        }
        dfs(1,0);
        cout<<fixed<<setprecision(10)<<1.0*sum/(n-1)<<'\n';
    }
    return 0;
}
```