---
title: 2021CCPC山东省赛 C. Cat Virus
date: 2021-07-01 13:41:51
tags: [构造]
categories: 解题报告
mathjax: true
---

# 链接

<https://codeforces.com/gym/103118/problem/C>

# 题意

给定一棵树，黑白染色方案，满足一个黑点的子树都是黑点，白点任意。

现在构造一棵树，使得它的染色方案数为 $K$。

<!--more-->

# 思路

记点 $i$ 为白色时子树总方案为 $f_i$：

- 如果点 $u$ 为黑，那么 $u$ 的子树总方案为 $1$；
- 如果点 $u$ 为白，那么 $u$ 的子树总方案为 $\prod_{v\in son(u)}{f_v+1}$。

所以节点 $u$ 的总方案为 $1+f_u=1+\prod_{v\in son(u)}{f_v+1}$。

已知当点 $u$ 没有儿子时，$u$ 的子树总方案为 $2$。

有如下构造方案：

- 当 $k$ 是奇数时：$k-1=2^t*k'$，连 $t+1$ 个儿子，当 $k'=1$ 结束，否则构造一个儿子子树方案为 $k'$的情况，$k'$ 始终为奇数；
- 当 $k$ 是偶数时：连一个儿子，转化为奇数情况。


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
VPII e;
int t=1;
void dfs(int rt,LL k) {
    if(k<=2) return;
    if(k&1) {
        --k;
        while(k%2==0) e.EB(rt,++t),k/=2;
        if(k>2) e.EB(rt,++t);
        dfs(t,k);
    }
    else {
        e.EB(rt,++t);
        dfs(t,k-1);
    }
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    LL n;cin>>n;
    dfs(1,n);
    cout<<t<<'\n';
    for(auto x:e) cout<<x.FI<<' '<<x.SE<<'\n';
    return 0;
}
```