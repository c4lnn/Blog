---
title: 2019CCPC秦皇岛 J. MUV LUV EXTRA
date: 2021-06-08 00:08:28
tags: [KMP]
categories: 解题报告
mathjax: true
---

# 链接

<http://acm.hdu.edu.cn/showproblem.php?pid=6740>

# 题意

已知一个小数，定义它的可靠值为：$a*p-b*l$（ $p$ 为已经出现的循环部分总长度，$l$ 为循环节长度）

求最大的可靠值

<!--more-->

# 思路

倒着 KMP

# 代码

```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N=1e7+5;
char s[N];
ll nxt[N];
int len;
void getNext() {
    int i=0;
    int j=-1;
    nxt[0]=-1;
    while(i<len) {
        if(j==-1||s[i]==s[j]) {
            i++,j++;
            nxt[i]=j;
        }
        else j=nxt[j];
    }
}
int main() {
    int a,b;
    while(~scanf("%lld%lld",&a,&b)) {
        scanf("%s",s);
        sscanf(s,"%*[^.].%s",s);
		len=strlen(s);
        reverse(s,s+len);
        getNext();
        ll ans=0xc0c0c0c0c0c0c0c0;
        for(ll i=1;i<=len;i++) ans=max(ans,a*i-b*(i-nxt[i]));
        printf("%lld\n",ans);
    }
    return 0;
}
```
