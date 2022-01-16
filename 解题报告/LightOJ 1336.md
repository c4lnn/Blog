---
title: LightOJ 1336. Sigma Function
date: 2021-05-08 18:51:40
tags: [约数]
categories: 解题报告
mathjax: true
---

# 链接

<http://lightoj.com/volume_showproblem.php?problem=1336>

# 题意

$t(t\le100)$ 次询问，每次询问 $1\sim n(1\le n\le 1e12)$ 中有多少数的正约数和是偶数。

<!--more-->

# 思路

若正整数 $N$ 被唯一分解为 $N=p_1^{c_1}p_2^{c_2}\cdots p_m^{c_m}$，其中 $c_i$ 都是正整数，$p_i$ 都是质数。

那么 $N$ 的正约数和公式为：$(1+p_1+p_1^2+\cdots+p_1^{c_1})*\cdots*(1+p_m+p_m^2+\cdots+p_m^{c_m})=\prod\limits_{i=1}^{m}\sum\limits_{j=0}^{c_i}p_i^j$。

因为偶数 $*$ 奇数 $=$ 偶数，所以当公式中每一项都为奇数时，正约数和才为奇数。

易得：

- 当 $p=2$ 时，$\sum\limits_{i=0}^{c}p^i$ 为奇数。
- 当 $p\ne2$ 时，$c$ 为偶数时，$\sum\limits_{i=0}^{c}p^i$ 为奇数。

所以如果 $N$ 的正约数和为奇数，那么公式中除 $p=2$ 外的每一项 $c$ 都为偶数。

即 $N$ 是完全平方数或 $N/2$ 是完全平方数。

对于 $1\sim n$ 中 有多少正约数和是偶数：

- 当 $i*i\le n$，$i*i$ 的正约数和为奇数，共有 $\lfloor\sqrt{n}\rfloor$ 个。
- 当 $2*i*i\le n$，$2*i*i$ 的正约数和为奇数，共有 $\lfloor\sqrt{\lfloor\frac{n}{2}\rfloor}\rfloor$ 个。

所以 $1\sim n$ 中正约数和是偶数的个数为 $n-\lfloor\sqrt{n}\rfloor-\lfloor\sqrt{\lfloor\frac{n}{2}\rfloor}\rfloor$ 个。

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
typedef pair<int,int> PII;
typedef vector<int> VI;
typedef vector<PII> VPII;
//head
int main() {
    //freopen("E:/OneDrive/IO/in.txt","r",stdin);
    int tt;
    scanf("%d",&tt);
    for(int cs=1;cs<=tt;cs++) {
        LL n;
        scanf("%lld",&n);
        printf("Case %d: %lld\n",cs,n-(LL)sqrt(n)-(LL)sqrt(n/2));
    }
    return 0;
}
```