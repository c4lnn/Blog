---
title: 第 45 届国际大学生程序设计竞赛（ICPC）亚洲网上区域赛模拟赛 D. Pokemon Ultra Sun
date: 2021-04-19 23:26:28
tags: [期望 DP]
categories: 解题报告
mathjax: true
---

# 链接

<https://ac.nowcoder.com/acm/contest/8688/D>

# 题意

$A$ 有 $hp1$ 血量，$B$ 有 $hp2$ 血量。

每个回合 $A$ 会发起一次攻击，每次攻击造成 $w$ 伤害，有 $p$ 概率击中 $B$，$1-p$ 概率击中自己。

当有一个人血量少于等于 $0$ 时，游戏结束。

求回合数的期望。

<!--more-->

# 思路

记 $dp[i][j]$ 为 $A$ 剩 $1$ 血量 $b$ 剩 $j$ 血量的回合数期望。

当 $i=0$ 或 $j=0$ 时，期望必为 $0$。

倒推，$dp[hp1][hp2]$ 为答案。

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
typedef unsigned long long ULL;
typedef pair<int,int> PII;
typedef vector<int> VI;
typedef vector<PII> VPII;
//head
const int N=3005;
DB dp[N][N];
int main() {
    //freopen("E:/OneDrive/IO/in.txt","r",stdin);
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int T;
    cin>>T;
    while(T--) {
        int hp1,hp2,w;
        DB p;
        cin>>hp1>>hp2>>w>>p;
        for(int i=1;i<=hp1;i++) {
            for(int j=1;j<=hp2;j++) {
                dp[i][j]=(dp[max(0,i-w)][j]+1)*(1-p)+(dp[i][max(0,j-w)]+1)*p;
            }
        }
        cout<<fixed<<setprecision(6)<<dp[hp1][hp2]<<'\n';
    }
    return 0;
}
```