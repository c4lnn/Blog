---
title: 第八届“图灵杯”NEUQ-ACM程序设计竞赛个人赛 A. 切蛋糕
date: 2021-04-30 11:44:58
tags: [思维]
categories: 解题报告
mathjax: true
---

# 链接

<https://ac.nowcoder.com/acm/contest/11746/A>

# 题意

每次只能将一块蛋糕平均分成两块，蛋糕最小为 $\frac 1 {2^{15}}$。

可以将几块蛋糕打包分给某人，现在要分给 $k$ 个人，要求每个人分到的蛋糕与$\frac 1 k$ 的差的绝对值不大于$\frac 1 {2^{10}}$。

切蛋糕为一次操作，打包为 $1$ 操作，求操作次数不超过 $6000$ 次的方案并输出。

<!--more-->

# 思路

将蛋糕切成 $1024$ 块，等分给 $k$ 个人，每个人分到的蛋糕满足以下式子：

- $|\frac{1}{k}-\frac{\lfloor \frac {1024} {k} \rfloor}{1024}|\le\frac{1}{1024}$
- $|\frac{1}{k}-\frac{\lceil \frac {1024} {k} \rceil}{1024}|\le\frac{1}{1024}$

#  代码

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
int t,cnt=0;
void solve(int x) {
    if(x+1>10) return;
    cout<<1<<' '<<x<<'\n';
    solve(x+1);
    solve(x+1);
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int k;
    cin>>k;
    cout<<1023+k<<'\n';
    solve(0);
    int a=1024%k,b=1024/k;
    for(int i=1;i<=a;i++) {
        cout<<2<<' '<<b+1<<' ';
        for(int j=1;j<=b+1;j++) cout<<10<<" \n"[j==b+1];
    }
    for(int i=1;i<=k-a;i++) {
        cout<<2<<' '<<b<<' ';
        for(int j=1;j<=b;j++) cout<<10<<" \n"[j==b];
    }
    return 0;
}
```