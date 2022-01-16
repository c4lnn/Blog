---
title: 2019CCPC秦皇岛 I. Invoker
date: 2021-06-08 00:07:45
tags: [DP]
categories: 解题报告
mathjax: true
---

# 链接

<http://acm.hdu.edu.cn/showproblem.php?pid=6739>

# 题意

基本技能：Q，W，E

调用技能：R

特殊技能：Y，V，G，C，X，Z，T，F，D，B

每个特殊技能由三个基本技能组成，并且可以无序触发

拥有三个基本技能时，可以使用调用技能，以根据他当前拥有的基本技能获得特殊技能。调用后，基本技能不会消失，并且这三个基本技能的时间顺序也不会改变

现在给出了一系列特殊技能，用最少数量的基本技能和调用技能来逐一调用它们

<!--more-->

# 思路

一个特殊技能最多有 $6$ 种排列

将前后两个技能进行 $36$ 种排列配对

$dp[i][j]$( $i$ 为第 $i$ 个技能, $j$ 为技能 $i$ 的第 $j$ 种排列)

$dp[i][j]=min(dp[i][j],dp[i-1][k]+cmp(s[i-1]_{k},s[i]_j)(0\le j,k\le 5))$

# 代码

```cpp
#include<bits/stdc++.h>
using namespace std;
char s[100001];
map<char,int>mp;
char p[10][6][4]={
                    "QWE","QEW","WQE","WEQ","EQW","EWQ",
                    "WWW","WWW","WWW","WWW","WWW","WWW",
                    "WEE","WEE","EWE","EEW","EWE","EEW",
                    "QEE","QEE","EQE","EEQ","EQE","EEQ",
                    "QQE","QEQ","QQE","QEQ","EQQ","EQQ",
                    "EEE","EEE","EEE","EEE","EEE","EEE",
                    "QQW","QWQ","QQW","QWQ","WQQ","WQQ",
                    "QWW","QWW","WQW","WWQ","WQW","WWQ",
                    "QQQ","QQQ","QQQ","QQQ","QQQ","QQQ",
                    "WWE","WEW","WWE","WEW","EWW","EWW",
                  };
int dp[100001][6];
int cmp(int a,int b,int c,int d) {
    if(!strcmp(p[a][b],p[c][d])) return 0;
    else if(p[a][b][1]==p[c][d][0]&&p[a][b][2]==p[c][d][1]) return 1;
    else if(p[a][b][2]==p[c][d][0]) return 2;
    return 3;
}
int main() {
    mp['B']=0,mp['C']=1,mp['D']=2,mp['F']=3,mp['G']=4,mp['T']=5,mp['V']=6,mp['X']=7,mp['Y']=8,mp['Z']=9;
    while(~scanf("%s",s)) {
        for(int i=0;s[i];i++)
            for(int j=0;j<6;j++)
                dp[i][j]=(i+1)*3+i+1;
        for(int i=0;i<6;i++) dp[0][i]=3;
        for(int i=1;s[i];i++)
            for(int j=0;j<6;j++)
                for(int k=0;k<6;k++)
                    dp[i][j]=min(dp[i][j],dp[i-1][k]+cmp(mp[s[i-1]],k,mp[s[i]],j));
        int minn=0x3f3f3f3f;
        for(int i=0;i<6;i++) minn=min(minn,dp[strlen(s)-1][i]);
        printf("%d\n",minn+strlen(s));
    }
    return 0;
}
```
