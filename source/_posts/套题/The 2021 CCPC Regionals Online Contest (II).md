---
title: The 2021 CCPC Regionals Online Contest (II)
date: 2021-10-20 23:33:57
tags: [STL,二分,DP,并查集]
categories: 套题
mathjax: true
---

| Solved |  01   |  02   |  03   |  04   |  05   |  06   |  07   |  08   |  09   |  10   |  11   |  12   |
| :----: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
|  5/12  |   -   |   O   |   -   |   O   |   Ø   |   O   |   -   |   -   |   -   |   -   |   Ø   |   -   |

# 题目链接

<https://vjudge.net/article/2706>

<!--more-->

# Problem 04. Primality Test

**题意：**

定义 $f(x)$ 为大于 $x$ 的最小质数，判断 $\lfloor\frac{f(x)+f(f(x))}{2}\rfloor$ 是否为质数。

**思路：**

两个相邻的质数之间没有质数。

**代码：**

```cpp
#include <bits/stdc++.h>
#define SZ(x) (int)(x).size()
#define ALL(x) (x).begin(), (x).end()
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
typedef pair<int, int> PII;
typedef vector<int> VI;
typedef vector<LL> VLL;
typedef vector<PII> VPII;
// head
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int T;
    cin >> T;
    while (T--) {
        LL n;
        cin >> n;
        cout << (n == 1 ? "YES" : "NO") << '\n';
    }
    return 0;
}
```

# Problem 05. Monopoly

**题意：**

有 $n$ 个点，每走到一个点，得分加上 $a_i$。

从 $0$ 走到 $n$，再从 $n$ 走到 $1$，再从 $1$ 走到 $n$，以此类推。

$m$ 个询问，每次询问一个 $x$，求至少走多少步能得到 $x$ 分。

$T$ 组询问，$1\le n,m\le 1e5,-1e9\le a_i\le 1e9,-1e12\le x\le 1e12,\sum n\le 5e5,\sum m\le 5e5$。

**思路：**

记 $n$ 个点的和为 $S$，前 $i$ 个点的和为 $s_i$。

假设能够得到 $x$ 分，那么一定能表示为 $x=s_i+kS$，我们要找到最小的 $k$。

- 当 $x=0$ 时，输出 $0$。
- 当 $S=0$ 时，$k$ 一定等于 $0$，所以只要找出最小的 $i$ 满足 $s_i=x$。
- 当 $S>0$ 时，二分找到 $s_i\le x$ 且对 $S$ 取模与 $x$ 同余的最大的 $s_i$（$s_i$ 越大，$k$ 越小），如果这样的 $i$ 有多个，找到 $i$ 最小的那个。
- 当 $S<0$ 时，将 $x$ 和 $s_{1\sim n}$ 取反，转为 $S>0$ 的情况。

可以用哈希 map 嵌套 map，时间复杂度 $O(m\log(n))$。

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
        VLL a(n+1),sum(n+1);
        bool f=false;
        unordered_map<LL,int> vis;
        for(int i=1;i<=n;i++) {
            cin>>a[i];
            sum[i]=sum[i-1]+a[i];
            if(!vis.count(sum[i])) vis[sum[i]]=i;
        }
        if(sum[n]==0) {
            while(m--) {
                LL x;cin>>x;
                if(x==0) cout<<0<<'\n';
                else if(vis.count(x)) cout<<vis[x]<<'\n';
                else cout<<-1<<'\n';
            }
        }
        else {
            unordered_map<LL,map<LL,int>> mp;
            if(sum[n]<0) {
                for(int i=1;i<=n;i++) sum[i]=-sum[i];
                f=true;
            }
            for(int i=n;i>=1;i--) {
                LL rem=(sum[i]%sum[n]+sum[n])%sum[n];
                mp[rem][sum[i]]=i;
            }
            while(m--) {
                LL x;cin>>x;
                if(f) x=-x;
                LL rem=(x%sum[n]+sum[n])%sum[n];
                if(x==0) cout<<0<<'\n';
                else if(!mp.count(rem)) cout<<-1<<'\n';
                else {
                    auto pos=mp[rem].upper_bound(x);
                    if(pos==mp[rem].begin()) cout<<-1<<'\n';
                    else {
                        --pos;
                        cout<<pos->second+(x-pos->first)/sum[n]*n<<'\n';
                    }
                }
            }
        }
    }
    return 0;
}
```

# Problem 06. Nun Heh Heh Aaaaaaaaaaa

**题意：**

求字符串 $S$ 中有多少子序列是由 "numheheh" 和 "aaa..." 组成的，后半段字符 'a' 的个数不能为 $0$。

**思路：**

线性 DP。

**代码：**

```cpp
#include <bits/stdc++.h>
#define SZ(x) (int)(x).size()
#define ALL(x) (x).begin(), (x).end()
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
typedef pair<int, int> PII;
typedef vector<int> VI;
typedef vector<LL> VLL;
typedef vector<PII> VPII;
// head
const string S = "nunhehheha";
const LL MOD = 998244353;
const int N = 1e5 + 5;
LL dp[N][20];
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int T;
    cin >> T;
    while (T--) {
        string s;
        cin >> s;
        if(s[0] == 'n') dp[0][0] = 1;
        else dp[0][0] = 0;
        for (int i = 1; i < SZ(s); i++) {
            for (int j = 0; j < 10; j++) {
                dp[i][j] = dp[i - 1][j];
                if (s[i] != S[j]) continue;
                if (j == 0) dp[i][j] = (dp[i][j] + 1) % MOD;
                else if (j == 9) dp[i][j] = (dp[i][j] + dp[i-1][j-1] + dp[i-1][j]) % MOD;
                else dp[i][j] = (dp[i][j] + dp[i-1][j-1]) % MOD;
            }
        }
        cout << dp[SZ(s) - 1][9] << '\n';
    }
    return 0;
}
```

# Problem 11. Jumping Monkey

**题意：**

$n$ 个节点的点权树，每个节点的点权均不相同。

当且仅当 $a$ 到 $b$ 的简单路径上 $b$ 为点权最大的点时，能够从 $a$ 出发，跳到 $b$。

分别以 $1\sim n$ 为起点，保证跳的步数最多，输出最多能到达多少个点（包括起点）。

**思路：**

按节点从大到小考虑，当前图中点权最大的点一定无法跳到其余点，而其余与该点连通的点一定能跳到该点，将与该点连通的点答案 $+1$，删除该点，重复上述操作，直到图中没有点，这样答案就被计算了出来。

但是该过程代码实现较复杂，考虑逆过程。

开始时图中没有点，按节点权值从小到大加入图中，将在原图中与当前节点相邻且已加入该图中的节点所在的连通块与当前点相连，当前点为祖先。

每个点的答案为新图中的深度。

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
int fa[N],dep[N];
PII a[N];
VI g[N],gg[N];
bool st[N];
int find(int x) {
    return x==fa[x]?x:fa[x]=find(fa[x]);
}
void dfs(int u,int fa) {
    for(auto v:gg[u]) if(v!=fa) {
        dep[v]=dep[u]+1;
        dfs(v,u);
    }
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int T;cin>>T;
    while(T--) {
        int n;cin>>n;
        for(int i=1;i<=n;i++) {
            g[i].clear();
            gg[i].clear();
            st[i]=false;
            fa[i]=i;
        }
        for(int i=1;i<n;i++) {
            int u,v;cin>>u>>v;
            g[u].PB(v);
            g[v].PB(u);
        }
        for(int i=1;i<=n;i++) {
            cin>>a[i].FI;
            a[i].SE=i;
        }
        sort(a+1,a+1+n);
        for(int i=1;i<=n;i++) {
            int u=a[i].SE;
            for(auto v:g[u]) if(st[v]) {
                int fv=find(v);
                gg[u].PB(fv);
                fa[fv]=u;
            }
            st[u]=true;
        }
        dep[a[n].SE]=1;
        dfs(a[n].SE,0);
        for(int i=1;i<=n;i++) cout<<dep[i]<<'\n';
    }
    return 0;
}
```