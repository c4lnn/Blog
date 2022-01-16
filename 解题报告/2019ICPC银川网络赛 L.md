---
title: 2019ICPC银川网络赛 L. Continuous Intervals
date: 2021-06-10 19:18:04
tags: [单调栈,线段树]
categories: 解题报告
mathjax: true
---

# 链接

<https://nanti.jisuanke.com/t/41296>

# 题意

给一个长度为 $n$ 的序列，求区间内元素从小到大排序后相邻元素差的的绝对值小于等于 $1$ 的区间数

<!--more-->

# 思路

区间内元素从小到大排序后相邻元素差的的绝对值小于等于 $1$

即区间内最大数 $-$ 区间内最小数 $-$ 区间内不同数的个数 $=-1$

记 $f=$ 区间内最大数 $-$ 区间内最小数 $-$ 区间内不同数的个数

枚举区间右端点，用线段树维护区间左端点

需要记录 $f$ 的最小值以及最小值的个数

对于区间最大值和最小值的更新，用单调栈维护，每次弹出在线段树上更新

对于区间内不同数的个数，记录当前位置 $x$ 的数上次出现的位置 $y$，那么 $[y+1,x]$ 多一个不同数

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
const int N=1e5+5;
int a[N],pre[N],top1,top2,s1[N],s2[N],ls[N<<2],rs[N<<2],mn[N<<2],add[N<<2],cnt[N<<2];
map<int,int> pos;
void build(int p,int l,int r) {
    ls[p]=l,rs[p]=r;
    mn[p]=add[p]=0;
    cnt[p]=r-l+1;
    if(l==r) return;
    int mid=l+r>>1;
    build(p<<1,l,mid);
    build(p<<1|1,mid+1,r);
}
void push_down(int p) {
    if(add[p]) {
        mn[p<<1]+=add[p],mn[p<<1|1]+=add[p];
        add[p<<1]+=add[p],add[p<<1|1]+=add[p];
        add[p]=0;
    }
}
void update(int p,int l,int r,int v) {
    if(ls[p]>=l&&rs[p]<=r) {
        mn[p]+=v;
        add[p]+=v;
        return;
    }
    push_down(p);
    int mid=ls[p]+rs[p]>>1;
    if(l<=mid) update(p<<1,l,r,v);
    if(r>mid) update(p<<1|1,l,r,v);
    mn[p]=min(mn[p<<1],mn[p<<1|1]);
    cnt[p]=0;
    if(mn[p<<1]==mn[p]) cnt[p]+=cnt[p<<1];
    if(mn[p<<1|1]==mn[p]) cnt[p]+=cnt[p<<1|1];
}
int query(int p,int l,int r) {
    if(ls[p]>=l&&rs[p]<=r) {
        return mn[p]==-1?cnt[p]:0;
    }
    push_down(p);
    int mid=ls[p]+rs[p]>>1;
    if(r<=mid) return query(p<<1,l,r);
    if(l>mid) return query(p<<1|1,l,r);
    return query(p<<1,l,r)+query(p<<1|1,l,r);
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int T;cin>>T;
    for(int cs=1;cs<=T;cs++) {
        int n;cin>>n;
        pos.clear();
        top1=top2=0;
        for(int i=1;i<=n;i++) {
            cin>>a[i];
            if(!pos.count(a[i])) pos[a[i]]=0;
            pre[i]=pos[a[i]];
            pos[a[i]]=i;
        }
        build(1,1,n);
        LL res=0;
        for(int i=1;i<=n;i++) {
            while(top1&&a[s1[top1]]<a[i]) {
                int t=s1[top1--];
                update(1,s1[top1]+1,t,a[i]-a[t]);
            }
            s1[++top1]=i;
            while(top2&&a[s2[top2]]>a[i]) {
                int t=s2[top2--];
                update(1,s2[top2]+1,t,a[t]-a[i]);
            }
            s2[++top2]=i;
            update(1,pre[i]+1,i,-1);
            res+=query(1,1,i);
        }
        cout<<"Case #"<<cs<<": "<<res<<'\n';
    }
    return 0;
}
```