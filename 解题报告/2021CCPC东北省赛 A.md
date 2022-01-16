---
title: 2021CCPC东北省赛 A. Matrix
date: 2021-09-13 22:40:49
tags: [组合数学]
categories: 解题报告
mathjax: true
---

# 链接

<https://codeforces.com/gym/103145/problem/C>

# 题意

将 $1\sim n^2$ 填进 $n*n$ 的矩阵中。

记 $a_i$ 为第 $i$ 行最小的元素，$S=\{a_1,a_2,...,a_n\}\cap\{1,2,...,n\}$。

考虑所有填充方法，求 $\sum{|S|}$。

<!--more-->

# 思路

考虑每个 $i$ $(1\le i \le n)$ 对答案的贡献：

该行其余 $n-1$ 个数的选择为 $\binom{n^2-i}{n-1}$，该行位置可以随便变换，乘上 $n!$，其余 $n-1$ 行随便填，乘上 $(n^2-n)!$，该行整体可随意放，乘上 $n$。

所以 $res=\sum_{i=1}^n\binom{n^2-i}{n-1}*n!*(n^2-n)!*n$。

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
const LL MOD=998244353;
const int N=25000005;
LL fac[N],invfac[N];
LL qpow(LL a,LL b) {
    LL ret=1;
    while(b) {
        if(b&1) ret=ret*a%MOD;
        a=a*a%MOD;
        b>>=1;
    }
    return ret;
}
void init(int n) {
    fac[0]=1;
    for(int i=1;i<=n;i++) fac[i]=fac[i-1]*i%MOD;
    invfac[n]=qpow(fac[n],MOD-2);
    for(int i=n-1;~i;i--) invfac[i]=invfac[i+1]*(i+1)%MOD;
}
LL binom(int n,int m) {
    return fac[n]*invfac[m]%MOD*invfac[n-m]%MOD;
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    init(25000000);
    int T;cin>>T;
    while(T--) {
        int n;cin>>n;
        LL res=0;
        for(int i=1;i<=n;i++) res=(res+binom(n*n-i,n-1)*fac[n]%MOD*fac[n*n-n]%MOD*n%MOD)%MOD;
        cout<<res<<'\n';
    }
    return 0;
}
```