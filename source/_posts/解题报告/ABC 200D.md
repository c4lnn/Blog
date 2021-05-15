---
title: AtCoder Beginner Contest 200 D. Happy Birthday! 2
date: 2021-05-09 11:28:58
tags: [组合数学,状态压缩]
categories: 解题报告
mathjax: true
---

# 链接

<https://atcoder.jp/contests/abc200/tasks/abc200_d>

# 题意

$n$ 个数，找出两个不同的子集，必须保证子集和对 $200$ 取模后余数相同。

<!--more-->

# 思路

根据鸽巢原理可知至少需要 $8$ 个数，这样有 $2^8$ 种方案，一定能保证有两个方案模 $200$ 后余数相同。

取前 $8$ 个数，状压计算所有方案。

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
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int n;cin>>n;
    VI a(n),s[200];
    for(int i=0;i<n;i++) cin>>a[i];
    int lim=min(n,8);
    for(int i=0;i<1<<lim;i++) {
        VI t;
        int sum=0;
        for(int j=0;j<lim;j++) if(i&(1<<j)) {
            t.PB(j+1);
            sum=(sum+a[j])%200;
        }
        if(SZ(s[sum])) {
            cout<<"YES"<<'\n';
            cout<<SZ(s[sum])<<' ';
            for(int j=0;j<SZ(s[sum]);j++) cout<<s[sum][j]<<" \n"[j==SZ(s[sum])-1];
            cout<<SZ(t)<<' ';
            for(int j=0;j<SZ(t);j++) cout<<t[j]<<" \n"[j==SZ(t)-1];
            return 0;
        }
        else s[sum]=t;
    }
    cout<<"NO"<<'\n';
    return 0;
}
```