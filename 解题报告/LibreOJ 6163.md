---
title: LibreOJ 6163. 合并回文子串
date: 2021-06-10 22:22:25
tags: [区间 DP]
categories: 解题报告
mathjax: true
---

# 链接

<https://loj.ac/p/6163>

# 题意

将字符串 $A$ 和 $B$ 合并成一个回文串 $C$，属于 $A$ 和 $B$ 的字符在 $C$ 中顺序保持不变

求可能的 $C$ 中最大的长度

<!--more-->

# 思路

区间 DP

设 $f[i][j][k][l]$ 为 $A$ 的第 $i$ 个字符到第 $j$ 个字符与 $B$ 的第 $k$ 个字符到第 l 个字符是否能组成的满足要求的回文串

转移方程：

$f[i][j][k][l]=(f[i+1][j-1][k][l]+a[i]==a[j])|(f[i+1][j][k][l-1]+a[i]==b[l])|(f[i][j-1][k+1][l]+a[j]==b[k])|(f[i][j][k+1][l-1]+b[k]==b[l])$

如果 $f[i][j][k][l]$ 为 $1$， 说明能合并成回文串，那么更新 $res$

# 代码

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N=60;
char a[N],b[N];
int f[N][N][N][N];
int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    int T;
    cin>>T;
    while(T--) {
        cin>>a+1;
        cin>>b+1;
        int len_a=strlen(a+1),len_b=strlen(b+1);
        int res=1;
        for(int len1=0;len1<=len_a;len1++)
            for(int len2=0;len2<=len_b;len2++)
                for(int i=1;i+len1-1<=len_a;i++)
                    for(int k=1;k+len2-1<=len_b;k++) {
                        int j=i+len1-1,l=k+len2-1;
                        if(len1+len2<=1) f[i][j][k][l]=1;
                        else {
                            f[i][j][k][l]=0;
                            if(len1>1) f[i][j][k][l]|=(f[i+1][j-1][k][l]&&a[i]==a[j]);
                            if(len2>1) f[i][j][k][l]|=(f[i][j][k+1][l-1]&&b[k]==b[l]);
                            if(len1&&len2) {
                                f[i][j][k][l]|=(f[i+1][j][k][l-1]&&(a[i]==b[l]));
                                f[i][j][k][l]|=(f[i][j-1][k+1][l]&&(a[j]==b[k]));
                            }
                        }
                        if(f[i][j][k][l]) res=max(res,len1+len2);
                    }
        cout<<res<<endl;
    }
    return 0;
}
```