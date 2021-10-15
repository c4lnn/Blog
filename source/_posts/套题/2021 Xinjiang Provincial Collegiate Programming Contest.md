---
title: 2021 Xinjiang Provincial Collegiate Programming Contest
date: 2021-10-04 23:40:34
tags: [组合数学,二分,基础 DP]
categories: 套题
mathjax: true
---

| Solved |   A   |   B   |   C   |   D   |   E   |   F   |   G   |   H   |   I   |   J   |   K   |
| :----: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
|  9/11  |   -   |   Ø   |   -   |   Ø   |   Ø   |   Ø   |   Ø   |   Ø   |   Ø   |   Ø   |   Ø   |

# 题目链接

<https://codeforces.com/gym/103115>

<!--more-->

# Problem B. cocktail with hearthstone

**题意：**

有 $2^{n+m}$ 个人，每个人初始分为 $(0,0)$。

当两个人当前分数都为 $(a,b)$ 时，代表 $a$ 胜 $b$ 负，那么这两人之间能够进行一场对局，最终一人为 $(a+1,b)$，一人为 $(a,b+1)$。

当某人拥有 $n$ 个胜场或者 $m$ 个负场时，其无法再进行比赛。

$q$ 此询问，求最终有多少人会是 $(a,b)$，保证 $a=n$ 或 $b=m$。

**思路：**

记 $f_{a,b}$ 为 $a$ 胜 $b$ 负的人数，$f_{a,b}$ 会为 $f_{a+1,b}$ 和 $f_{a,b+1}$ 带来 $\frac{f_{a,b}}{2}$ 的贡献。

可以将分子和分母分开考虑，分子为转移过来的状态的分子和，分母为 $2$ 的当前轮数幂次。

$$
\begin{aligned}
&(0,0)\\
&(0,1)(1,0)\\
&(0,2)(1,1)(2,0)\\
&(0,3) (1,2) (2,1) (3,0)\\
&\dots
\end{aligned}
$$

不难发现这是个杨辉三角，在上图中第 $i$ 行的第 $j$ 个元素代表的是 $\binom{i-1}{j-1}$，$f_{a,b}$ 的分子为 $f_{a-1,b}$ 和 $f_{a,b-1}$ 的分子和。

在最后计算答案时，因为 $\binom{0}{0}=1$，最终得到的是占总人数的比例，还需乘上总人数才为答案。

分情况讨论。

- 当 $a=n,b=m$ 时，无解。
- 当 $a=n,b\neq m$ 时，$(a,b)$ 只会从 $(a,b-1)$ 转移过来，$f_{a,b}=\frac{\frac{\binom{a+b-1}{a-1}}{2}}{2^{a+b-1}}*2^{n+m}=\binom{a+b-1}{a-1}2^{m-b}$。
- 当 $a\neq n,b=m$ 时，$(a,b)$ 只会从 $(a-1,b)$ 转移过来，$f_{a,b}=\frac{\frac{\binom{a+b-1}{b-1}}{2}}{2^{a+b-1}}*2^{n+m}=\binom{a+b-1}{b-1}2^{n-a}$。

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
const LL MOD=1e9+7;
const int N=4e5+5;
LL fac[N],invfac[N],power[N];
LL qpow(LL a,LL b) {
    LL ret=1;
    while(b) {
        if(b&1) ret=ret*a%MOD;
        a=a*a%MOD;
        b>>=1;
    }
    return ret;
}
void init(int n) {
    fac[0]=power[0]=1;
    for(int i=1;i<=n;i++) {
        fac[i]=fac[i-1]*i%MOD;
        power[i]=power[i-1]*2%MOD;
    }
    invfac[n]=qpow(fac[n],MOD-2);
    for(int i=n-1;~i;i--) invfac[i]=invfac[i+1]*(i+1)%MOD;
}
LL binom(int n,int m) {
    return fac[n]*invfac[m]%MOD*invfac[n-m]%MOD;
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    init(4e5);
    int n,m,q;cin>>n>>m>>q;
    while(q--) {
        int a,b;cin>>a>>b;
        if(a==n&&b==m) cout<<0<<'\n';
        else if(a==n) cout<<binom(a+b-1,a-1)*power[m-b]%MOD<<'\n';
        else cout<<binom(a+b-1,b-1)*power[n-a]%MOD<<'\n';
    }
    return 0;
}
```

# Problem D. cocktail with swap

**题意：**

长度为 $n$ 的字符串 $s$，每次操作可以选择 $i,j$ 满足 $l\le|i-j|\le r$，交换 $s_i,s_j$。

可以进行无限次操作，求能得到的字典序最小字符串。

**思路：**

对于 $x$，当集合中存在一个下标可以与 $x$ 交换，那么 $x$ 可以放入这个集合，这个集合一定可以通过交换从小到大排序。

- 当 $l=r$ 时，每个集合只有两个数，$a_i$ 与 $a_{i+l}$ 可以交换。
- 当 $l<r,n\ge 2l$ 时，$r$ 最小为 $l+1$，一开始 $1,l+1,l+2$ 属于一个集合，

  对于 $i\in[2,l]$，$i-1+l\le i+l\le i-1+r$，因此最终都属于一个集合，整个字符串从小到大排序即可。
- 当 $l<r,n< 2l$ 时，在 $[1,l]$ 中存在无法交换的下标，对于其他位置排序即可。

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
const int N=1e5+5;
vector<char> a[N];
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int n,l,r;cin>>n>>l>>r;
    string s;cin>>s;
    if(l==r) {
        int sz=0;
        for(int i=0;i<l;i++) {
            for(int j=i;j<n;j+=l) {
                a[i].PB(s[j]);
            }
            sort(ALL(a[i]));
            sz=max(sz,SZ(a[i]));
        }
        for(int i=0;i<sz;i++) {
            for(int j=0;j<l;j++) if(i<SZ(a[j])) {
                cout<<a[j][i];
            }
        }
    }
    else if(n>=2*l) {
        sort(ALL(s));
        cout<<s<<'\n';
    }
    else {
        a[0].PB(s[0]);
        for(int i=l;i<n;i++) a[0].PB(s[i]);
        for(int i=1;i<n-l;i++) a[0].PB(s[i]);
        sort(ALL(a[0]));
        int pos=0;
        for(int i=0;i<n-l;i++) cout<<a[0][pos++];
        for(int i=n-l;i<l;i++) cout<<s[i];
        for(int i=l;i<n;i++) cout<<a[0][pos++];
    }
    return 0;
}
```

# Problem E. is the order a rabbit ??

**题意：**

有 $n$ 天，每天上午和下午各有一个价格，每天最多只能买一个物品以及最多只能卖出一次物品，但数量不限。

在开始前以及结束后没有物品保留，求最大收益。

**思路：**

求后缀最大值，每天选择收益最大的时候买入，显然并不存在在同一天两个时间卖出。

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
const int N=1e5+5;
int a[N],b[N],mxa[N],mxb[N];
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int n;cin>>n;
    for(int i=1;i<=n;i++) cin>>a[i]>>b[i];
    LL res=0;
    for(int i=n;i>=1;i--) {
        mxb[i]=max(a[i+1],mxa[i+1]);
        mxa[i]=max(b[i],mxb[i]);
        res+=max({0,mxb[i]-b[i],mxa[i]-a[i]});
    }
    cout<<res<<'\n';
    return 0;
}
```

# Problem F. chino with ball

**题意：**

在一条水平直线上有 n 个位置不同球，每个球的速度只有可能是 $-1,0,1$，分别方向是往左，不动，往右。

两球相撞后会交换速度，求 $k$ 秒后每个球所在位置。

**思路：**

两球相撞后交换速度可以看成两球穿过，那么球的最终位置是确定的，考虑每个位置的球的编号。

实际上球不是真的对穿过去的，因此编号顺序并未发生改变，初始时的位置顺序就是最终的位置顺序。

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
const int N=1e5+5;
int b[N],res[N];
PII a[N];
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int n,k;cin>>n>>k;
    for(int i=1;i<=n;i++) {
        cin>>a[i].FI;
        a[i].SE=i;
        int v;cin>>v;
        b[i]=a[i].FI+v*k;
    }
    sort(a+1,a+1+n);
    sort(b+1,b+1+n);
    for(int i=1;i<=n;i++) res[a[i].SE]=b[i];
    for(int i=1;i<=n;i++) cout<<res[i]<<" \n"[i==n];
    return 0;
}
```

# Problem G. cocktail with snake

**题意：**

$n*m$ 的矩阵中，从 $(1,1)$ 走到 $(n,1)$，再走到 $(n,2)$，再走到 $(1,2)$，再走到 $(1,3)$，再走到 $(n,3)$，当纵坐标超出边界时停止。

一步走一格，求 $k$ 步后与起点的曼哈顿距离。

**思路：**

对纵坐标奇偶讨论。

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
        LL n,m,k;cin>>n>>m>>k;
        LL t=k/n;
        if(t&1) cout<<t+n-k%n-1<<'\n';
        else cout<<t+k%n<<'\n';
    }
    return 0;
}
```

# Problem H. cocktail with pony

**题意：**

水平数轴上有两个不重合的点 $a,b$，$a,b$ 轮流移动，$a$ 每次移动 $v_1$ 步，$b$ 每次移动 $v_2$ 步，$a$ 先手。

两点移动区间为 $[1,n]$，$b$ 要抓住 $a$（在移动过程中两点重合），$a$ 要让回合数尽量多，$b$ 要让回合数尽量少，求最终经过多少轮 $b$ 抓住 $a$。

**思路：**

$a$ 肯定要向 $b$ 的反方向移动，在走到边界时，只能左右移动来保持和 $b$ 的距离，模拟一下就行。

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
        int n,v1,v2,x1,x2;cin>>n>>v1>>v2>>x1>>x2;
        if(x1<x2) {
            x1=n-x1+1;
            x2=n-x2+1;
        }
        for(int i=1;;i++) {
            if(i&1) {
                if(x1==2&&x2==1) {cout<<i<<'\n';break;}
                if(x2>v2) x2-=v2;
                else if((v2-x2)&1) x2=1;
                else x2=2;
            }
            else {
                if(x1-x2<=v1) {cout<<i<<'\n';break;}
                x1-=v1;
            }
        }
    }
    return 0;
}
```

# Problem I. chino with mates

**题意：**

长度为 $n$ 的数组 $a$ 与长度为 $m$ 的数组 $b$。

给出 $l,r$ 求 $a_i*b_j\in[l,r]$ 的数对数量。

**思路：**

枚举 $a_i$，二分出左右边界。

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
    int n,m;cin>>n>>m;
    VI a(n),b(m);
    for(int i=0;i<n;i++) cin>>a[i];
    for(int i=0;i<m;i++) cin>>b[i];
    int l,r;cin>>l>>r;
    sort(ALL(b));
    LL res=0;
    for(int i=0;i<n;i++) {
        if(a[i]==0) {
            if(l<=0&&r>=0) res+=m;
        }
        else if(a[i]>0) {
            int lpos=lower_bound(ALL(b),l<0?l/a[i]:int(ceil(1.0*l/a[i])))-b.begin();
            int rpos=upper_bound(ALL(b),r<0?int(floor(1.0*r/a[i])):r/a[i])-b.begin()-1;
            res+=rpos-lpos+1;
        }
        else {
            int lpos=lower_bound(ALL(b),r<0?int(ceil(1.0*r/a[i])):r/a[i])-b.begin();
            int rpos=upper_bound(ALL(b),l<0?l/a[i]:int(floor(1.0*l/a[i])))-b.begin()-1;
            res+=rpos-lpos+1;
        }
    }
    cout<<res<<'\n';
    return 0;
}
```

# Problem J. do NOT a=2b

**题意：**
长度 为 $n$ 的数组 $p$，删去一些数，使数组中不存在 $p_i=2p_j$，求最少删去多少数。

- $1\le n \le 1e5,1\le p_i\le 1e6$

**思路：**

值域可分为好多条链，对值域按照删或不删 DP。

对于 $[5e5,1e6]$ 内的值统计答案。

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
const int N=1e6+5;
int a[N],dp[N][2];
bool vis[N];
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int n;cin>>n;
    for(int i=1;i<=n;i++) {
        int x;cin>>x;
        ++a[x];
    }
    for(int i=1;i<=1e6;i++) if(!vis[i]) {
        dp[i][0]=0;
        dp[i][1]=a[i];
        vis[i]=true;
        for(int j=i*2;j<=1e6;j*=2) {
            if(a[j/2]) dp[j][0]=dp[j/2][1];
            else dp[j][0]=min(dp[j/2][0],dp[j/2][1]);
            dp[j][1]=min(dp[j/2][0],dp[j/2][1])+a[j];
            vis[j]=true;
        }
    }
    int sum=0;
    for(int i=500001;i<=1e6;i++) sum+=min(dp[i][0],dp[i][1]);
    cout<<sum<<'\n';
    return 0;
}
```

# Problem K. chino with c language

**题意：**

memcpy 拷贝时会覆盖原始数据，memmove 不会，模拟这两种操作。

**思路：**

模拟。

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
    int n;cin>>n;
    string s;cin>>s;
    string t1=s,t2=s;
    int a,b,c;cin>>a>>b>>c;
    --a,--b;
    for(int i=0;i<c;i++) t1[b+i]=t1[a+i];
    for(int i=0;i<c;i++) t2[b+i]=s[a+i];
    cout<<t1<<'\n';
    cout<<t2<<'\n';
    return 0;
}
```