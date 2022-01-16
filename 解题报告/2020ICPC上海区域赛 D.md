---
title: 2020ICPC上海区域赛 D. Walker
date: 2021-04-20 18:48:41
tags: [二分]
categories: 解题报告
mathjax: true
---

# 链接

<https://ac.nowcoder.com/acm/contest/9925/D>

# 题意

线段上有两点以各自速度随意移动，问最少经过多久两点移动的路径能覆盖整段线段。

<!--more-->

# 思路

二分时间。

考虑如下情况:

- 某点能覆盖整段线段
- 左边的点向右移动，右边的点向左移动
- 左边的点向左移动，右边的点向右移动（两点可先向中间移动再折返）
  - 都不先向中间移动
  - 左边的点先向中间移动再折返，右边的点向右移动
  - 右边的点先向中间移动再折返，左边的点向右移动
  - 两点都先向中间移动再折返

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
DB n,p1,p2,v1,v2;
bool check(DB t) {
    DB a,b;
    a=min(p2-p1,max(0.0,v1*t-2*p1));
    b=min(p2-p1,max(0.0,v2*t-2*(n-p2)));
    if(v1*t>=p1&&v2*t>=n-p2&&a+b>=p2-p1) return true;
    if(v1*t>=p1&&v2*t>=n-p2+2*(p2-p1-a)) return true;
    if(v1*t>=p1+2*(p2-p1-b)&&v2*t>=n-p2) return true;
    a=min(p2-p1,max(0.0,(t-p1/v1)*v1/2));
    b=min(p2-p1,max(0.0,(t-(n-p2)/v2)*v2/2));
    if(t*v1>=p1&&t*v2>=n-p2&&a+b>=p2-p1) return true;
    return false;
}
int main() {
    //freopen("E:/OneDrive/IO/in.txt","r",stdin);
    int tt;
    scanf("%d",&tt);
    while(tt--) {
        scanf("%lf%lf%lf%lf%lf",&n,&p1,&v1,&p2,&v2);
        if(p1>p2) swap(p1,p2),swap(v1,v2);
        DB l=0,r=min({min(n+p1,n+n-p1)/v1,min(n+p2,n+n-p2)/v2,max((n-p1)/v1,p2/v2)});
        for(int i=0;i<200;i++) {
            DB mid=(l+r)/2.0;
            if(check(mid)) r=mid;
            else l=mid;
        }
        printf("%.10f\n",r);
    }
    return 0;
}
```