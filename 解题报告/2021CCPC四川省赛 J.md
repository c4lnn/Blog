---
title: 2021CCPC四川省赛 J. Ants
date: 2021-06-29 11:53:31
tags: [思维]
categories: 解题报告
mathjax: true
---

# 链接

<https://codeforces.com/gym/103117/problem/J>

# 题意

长度 $L=10^9+1$ 的数轴上 $N$ 只蚂蚁，蚂蚁互相碰到之后各自转身，数轴两端各有一块挡板，一只蚂蚁撞到之后直接转身，挡板被撞 $K$ 次就消失，问所有蚂蚁走出去的时间。

<!--more-->

# 思路

经过 $2L$ 时间所有蚂蚁会会到原位且方向不变，左右挡板都会被碰撞 $n$ 次。

因此我们只需要模拟最后一轮，蚂蚁互相碰到之后各自转身可当作穿过，不用管。

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
const int LEN=1e9+1;
const int N=1e6+5;
int cnt1,cnt2,a[N],v1[N],v2[N],n,l,r;
LL b[N];
void solve() {
    int cnt=n;
    while(cnt) {
        for(int i=1;i<=cnt1;i++) {
            int x=v1[i];
            b[x]+=a[x];
            a[x]=0;
            if(l) {
                --l;
                v2[++cnt2]=x;
            }
            else --cnt;
            if(!cnt) return;
        }
        cnt1=0;
        for(int i=1;i<=cnt2;i++) {
            int x=v2[i];
            b[x]+=LEN-a[x];
            a[x]=LEN;
            if(r) {
                --r;
                v1[++cnt1]=x;
            }
            else --cnt;
            if(!cnt) return;
        }
        cnt2=0;
    }
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cin>>n>>l>>r;
    int t=min(l/n,r/n);
    l-=t*n,r-=t*n;
    for(int i=1;i<=n;i++) {
        cin>>a[i];
        b[i]+=2ll*t*LEN;
    }
    for(int i=1;i<=n;i++) {
        int x;cin>>x;
        if(!x) v1[++cnt1]=i;
        else v2[++cnt2]=i;
    }
    reverse(v2+1,v2+1+cnt2);
    solve();
    LL res=0;
    for(int i=1;i<=n;i++) res=max(res,b[i]);
    cout<<res<<'\n';
    return 0;
}
```