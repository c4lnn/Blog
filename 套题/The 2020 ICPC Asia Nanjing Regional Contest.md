---
title: The 2020 ICPC Asia Nanjing Regional Contest
date: 2021-10-31 13:07:51
tags: [三分]
categories: 套题
mathjax: true
---

| Solved |   A   |   B   |   C   |   D   |   E   |   F   |   G   |   H   |   I   |   J   |   K   |   L   |   M   |
| :----: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
|  4/13  |   -   |   -   |   -   |   -   |   Ø   |   Ø   |   -   |   -   |   -   |   -   |   Ø   |   Ø   |   -   |

# 题目链接

<https://codeforces.com/gym/102992>

<!--more-->

# Problem E. Evil Coordinate

**题意：**

机器人从 $(0,0)$ 出发，每次可以上下左右走一格，给出操作序列，求是否存在一个操作序列的排列，使机器人没有到达过 $(x,y)$。

**思路：**

可以证明存在一种答案，使得相同的方向是连续排在一起的。枚举 $24$ 种排列即可。

**代码：**

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
typedef vector<LL> VLL;
typedef vector<PII> VPII;
// head
const int DIR[4][2]={0,1,0,-1,-1,0,1,0};
const char S[]="UDLR";
int x,y,cnt[4];
bool solve() {
    VI a={0,1,2,3};
        do {
            int xx=0,yy=0;
            bool f=false;
            for(int i=0;i<4;i++) {
                for(int j=0;j<cnt[a[i]];j++) {
                    xx+=DIR[a[i]][0],yy+=DIR[a[i]][1];
                    if(xx==x&&yy==y) {f=true;break;}
                }
                if(f) break;
            }
            if(!f) {
                for(int i=0;i<4;i++) {
                    for(int j=0;j<cnt[a[i]];j++) {
                        cout<<S[a[i]];
                    }
                }
                cout<<'\n';
                return true;
            }
        }while(next_permutation(ALL(a)));
    return false;
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int T;cin>>T;
    while(T--) {
        string s;cin>>x>>y>>s;
        memset(cnt,0,sizeof cnt);
        for(auto c:s) {
            if(c=='U') ++cnt[0];
            else if(c=='D') ++cnt[1];
            else if(c=='L') ++cnt[2];
            else ++cnt[3];
        }
        if(x==0&&y==0||!solve()) cout<<"Impossible\n";
    }
    return 0;
}
```

# Problem F. Fireworks

**题意：**

每次花费 $n$ 分钟制作烟花，有 $p*10^{-4}$ 的概率制造出完美的烟花，每次制作完烟花后，可以选择花费 $m$ 分钟点燃所有烟花。

如果在点燃的烟花中至少有一个完美的烟花，那么结束。

求最少的期望时间结束。

**思路：**

假设制造了 $k$ 个烟花，其中至少有一个完美的烟花，那么期望时间为 $\frac{kn+m}{1-(1-\frac{p}{10000})^k}$。

这是一个单谷函数，三分即可。

**代码：**

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
typedef vector<LL> VLL;
typedef vector<PII> VPII;
// head
const DB EPS=1e-6;
int sgn(DB x) {return fabs(x)<EPS?0:(x>0?1:-1);}
DB n,m,p;
DB qpow(DB a,int b) {
    DB ret=1;
    while(b) {
        if(b&1) ret=ret*a;
        a=a*a;
        b>>=1;
    }
    return ret;
}
DB calc(int x) {
    return 1.0*(x*n+m)/(1-qpow(1-p,x));
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int T;cin>>T;
    while(T--) {
        cin>>n>>m>>p;
        p*=0.0001;
        int l=1,r=1e9;
        while(l<r) {
            int lmid=l+(r-l)/3,rmid=r-(r-l)/3;
            if(sgn(calc(lmid)-calc(rmid))<0) r=rmid-1;
            else l=lmid+1;
        }
        cout<<fixed<<setprecision(10)<<calc(l)<<'\n';
    }
    return 0;
}
```

# Problem K. K Co-prime Permutation

**题意：**

求一个排列满足恰好有 $k$ 个位置 $\gcd(i,p_i)=1$。

**思路：**

当 $k$ 为奇数时，让 $p_i=i+1,p_{i+1}=i$ $(i<k,i\%2=0)$，其余位置 $p_i=i$。

当 $k$ 为偶数时，让 $p_i=i+1,p_{i+1}=i$ $(i<k,i\%2=1)$，其余位置 $p_i=i$。

**代码：**

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
typedef vector<LL> VLL;
typedef vector<PII> VPII;
// head
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int n,k;cin>>n>>k;
    if(k==0) return cout<<-1<<'\n',0;
    if(k&1) {
        cout<<1<<" \n"[n==1];
        for(int i=2;i<=k;i++) cout<<(i&1?i-1:i+1)<<" \n"[i==n];
    }
    else for(int i=1;i<=k;i++) cout<<(i&1?i+1:i-1)<<" \n"[i==n];
    for(int i=k+1;i<=n;i++) cout<<i<<" \n"[i==n];
    return 0;
}
```

# Problem L. Let's Play Curling

**题意：**

$x$ 轴上有 $n$ 个红点 $a$，$m$ 个蓝点 $b$，找到一个点 $c$，使尽量多的 $a_i$ $(1\le i\le n)$ 满足 $|c-a_i|<|c-b_j|$ $(1\le j\le m)$。

**思路：**

本题实际上就是求两个蓝点之间最多有几个红点。

**代码：**

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
typedef vector<LL> VLL;
typedef vector<PII> VPII;
// head
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int T;cin>>T;
    while(T--) {
        int n,m;cin>>n>>m;
        map<int,int> mp;
        for(int i=1;i<=n;i++) {
            int x;cin>>x;
            ++mp[x];
        }
        for(int i=1;i<=m;i++) {
            int x;cin>>x;
            mp[x]=0;
        }
        int sum=0,res=0;
        for(auto x:mp) {
            if(x.SE==0) res=max(res,sum),sum=0;
            else sum+=x.SE;
        }
        res=max(res,sum);
        if(res==0) cout<<"Impossible\n";
        else cout<<res<<'\n';
    }
    return 0;
}
```