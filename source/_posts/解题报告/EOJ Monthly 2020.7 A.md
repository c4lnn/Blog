---
title: EOJ Monthly 2020.7 A. 打字机
date: 2021-06-07 15:12:22
tags: [贪心]
categories: 解题报告
mathjax: true
---

# 链接

<https://acm.ecnu.edu.cn/contest/292/problem/A/>

# 题意

操作 1：将任意一个 "X" 替换为 "aX"

操作 2：将任意一个 "X" 替换为 "aXbX"

操作 3：删除任意一个 "X"

初始仅有一个 "X"

仅包含 "a"，"b"  的字符串 $S$，如果 $S$ 中的某个 "a" 既可以通过操作 1 得到，又可以通过操作 2 得到，输出 Sad Fang，不能打出字符串 $S$，输出 Dead Fang，否则输出 Happy Fang

<!--more-->

# 思路

1）每个前缀都有 $cnt["a"]>cnt["b"]$，否则 Dead

2）最后一个以 "b" 结尾的前缀有 $cnt["a"]<=cnt["b"]$，否则 Sad

其余情况均为 Happy

# 代码

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N=1e6+5;
char s[N];
int f,t,cnt[200];
int main() {
    int T;
    scanf("%d",&T);
    while(T--) {
        scanf("%s",s+1);
        int n=strlen(s+1);
        cnt['a']=cnt['b']=0;
        f=t=0;
        for(int i=1;i<=n;i++) {
            cnt[s[i]]++;
            if(s[i]=='b') t=cnt['a'];
            if(s[i]=='b'&&cnt['b']>cnt['a']) {f=2;break;}
        }
        if(f!=2&&cnt['a']>cnt['b']&&t>cnt['b']) f=1;
        if(cnt['b']==0) f=0;
        if(f==0) puts("Happy Fang");
        else if(f==1) puts("Sad Fang");
        else puts("Dead Fang");
    }
    return 0;
}
```