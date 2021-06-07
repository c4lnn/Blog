---
title: EOJ Monthly 2020.7 C. OLED
date: 2021-06-07 15:10:53
tags: [前缀和,差分]
categories: 解题报告
mathjax: true
---

# 链接

<https://acm.ecnu.edu.cn/contest/292/problem/C/>

# 题意

一个 $n*m$ 的 $01$ 矩阵 $p$ 在 $a*b$ 的矩阵 $q$ 中移动，求每个点为 $1$ 的概率，按照概率最大的点是 $100$ 换算每个点的答案

<!--more-->

# 思路

差分 & 二维前缀和

分别讨论 $p$ 中每个点对 $q$ 的贡献

# 代码

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N=4000;
int n,m,a,b,d[N][N],mx;
int main() {
    scanf("%d%d%d%d",&n,&m,&a,&b);
    for(int i=1;i<=n;i++)
        for(int j=1;j<=m;j++) {
        int t;
        scanf("%d",&t);
            if(!t) continue;
            d[i][j]++;
            int x=a-n+1+i,y=b-m+1+j;
            d[x][j]--;
            d[i][y]--;
            d[x][y]++;
        }
    for(int i=1;i<=a;i++)
        for(int j=1;j<=b;j++) {
            d[i][j]+=d[i-1][j]+d[i][j-1]-d[i-1][j-1];
            mx=max(mx,d[i][j]);
        }
    for(int i=1;i<=a;i++) {
        for(int j=1;j<=b;j++)
            printf("%d ",100*d[i][j]/mx);
        puts("");
    }
    return 0;
}
```