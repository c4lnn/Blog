---
title: 'Codeforces Round #741 (Div. 2)'
date: 2021-10-25 16:59:13
tags: []
categories: 套题
mathjax: true
---

| Solved |   A   |   B   |   C   |  D1   |  D2   |   E   |   F   |
| :----: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
|  5/7   |   Ø   |   Ø   |   Ø   |   Ø   |   Ø   |   -   |   -   |

# 题目链接

<https://codeforces.com/contest/1562>

<!--more-->

# Problem A. The Miracle and the Sleeper

**题意：**

已知 $l,r$，求 $a\bmod b$ $(l\le b\le a\le r)$ 的最大结果。

**思路：**

$\begin{cases}
b\ge l\\
b+b-1\le r
\end{cases}\rightarrow l\le b\le\lfloor\frac{r+1}{2}\rfloor$

- 当 $l\le b\le\lfloor\frac{r+1}{2}\rfloor$ 时，$b=\lfloor\frac{r+1}{2}\rfloor,a=2*b-1$。
- 否则，$b=l,a=r$。

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
        int a,b;cin>>a>>b;
        if(b/2+1>=a) cout<<b%(b/2+1)<<'\n';
        else cout<<b%a<<'\n';
    }
    return 0;
}
```

# Problem B. Scenes From a Memory

**题意：**

长度为 $n$ 的整数，求最多能删去多少位，使剩下的数不是素数，保证有解。

**思路：**

当存在 $1,4,6,8,9$，这些数均不是素数，所以最短长度为 $1$。

否则 剩下 $2,3,5,7$。

当存在 $22,32,52,72,33,25,35,55,75,27,57,77$ 时最短长度为 $2$。

只有这两种情况。

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
        int k;string s;cin>>k>>s;
        bool f=false;
        for(auto c:s) {
            if(c=='1'||c=='4'||c=='6'||c=='8'||c=='9') {
                cout<<1<<'\n'<<c<<'\n';
                f=true;
                break;
            }
        }
        if(f) continue;
        vector<bool> vis(10);
        cout<<2<<'\n';
        for(auto c:s) {
            if(c=='2'||c=='5') {
                if(vis[2]) {cout<<2<<c<<'\n';break;}
                if(vis[3]) {cout<<3<<c<<'\n';break;}
                if(vis[5]) {cout<<5<<c<<'\n';break;}
                if(vis[7]) {cout<<7<<c<<'\n';break;}
            }
            else if(c=='3') {
                if(vis[3]) {cout<<33<<'\n';break;}
            }
            else if(c=='7') {
                if(vis[2]) {cout<<27<<'\n';break;}
                if(vis[5]) {cout<<57<<'\n';break;}
                if(vis[7]) {cout<<77<<'\n';break;}
            }
            vis[c-'0']=true;
        }
    }
    return 0;
}
```

# Problem C. Rings

**题意：**

长度为 $n$ 的 二进制串 $s$，找出两个子串 $s[l_1:r_1]$ 和 $s[l_2:r_2]$，使前者代表的十进制数是后者的倍数。

两个子串的长度均大于 $\lfloor\frac{n}{2}\rfloor$。

**思路：**

对于一个二进制数，左移一位，即在末尾加一个 $0$，相当于原数乘 $2$。

对于 $i>\lfloor \frac n 2\rfloor$，若 $s_i=0$，那么 $s[1:i]$ 和 $s[1:i-1]$ 满足要求。

若找不到说明对于 $i>\lfloor \frac n 2\rfloor$，$s[i]$ 均为 $1$。

那么从 $i=\lfloor \frac n 2\rfloor$ 倒着遍历，当遇到 $s[i]=0$ 时，$s[i:i+\lfloor \frac n 2\rfloor-1]$和 $s[n-\lfloor \frac n 2\rfloor+1:n]$ 满足要求。

否则说明 $s$ 中没有 $0$，随便取两段长度 $>=\lfloor \frac n 2\rfloor$ 的子串均满足要求。

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
void solve(const string &s) {
    int len=SZ(s)/2;
    for(int i=len;i<SZ(s);i++) if(s[i]=='0') {
        cout<<1<<' '<<i+1<<' '<<1<<' '<<i<<'\n';
        return;
    }
    for(int i=len-1;~i;i--) if(s[i]=='0') {
        cout<<i+1<<' '<<i+1+len<<' '<<SZ(s)-len+1<<' '<<SZ(s)<<'\n';
        return;
    }
    cout<<1<<' '<<SZ(s)-1<<' '<<2<<' '<<SZ(s)<<'\n';
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int T;cin>>T;
    while(T--) {
        int n;string s;cin>>n>>s;
        solve(s);
    }
    return 0;
}
```

# Problem D. Two Hundred Twenty One

**题意：**


可以将数组 $a$ 删去几个数，使删去后的数组满足：$\sum_{i=1}^n(-1)^{i-1}a_i=0$。

求最小删去几个数，并输出方案。

**思路：**

对于连续的 $a_i$ 与 $a_{i+1}$，若 $a_i=a_{i+1}$，因为符号相反，相加为 $0$。

可以把这些 $i$ 无视，因为每次无视长度都为 $2$，那么最终剩余长度与原长度同奇偶。

剩下的相邻两数一定异号，那么总和的绝对值一定等于剩余长度。

删去一个数，相当于将后面一段对总和的贡献取相反数。

- 当原来总和为 $0$ 时，无需删去。
- 当长度为奇数时，无视连续的同值位置后长度仍为奇数，取中间位置删去，最终和必为 $0$，只需删去一个数。
- 当长度为偶数时，随便删去一个数转为奇数情况，只需删去两个数。

输出方案：

删去 $i$ 后总和为：$sum_{i-1}-sum_{l-1}-(sum_r-sum_i)$。

当总和为 $0$ 时，$sum_{i-1}+sum_i=sum_r+sum_{l-1}$。

用 set 存 $sum_{i-1}+sum_i$ 的位置，二分找到 $i$。

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
        int n,q;string s;cin>>n>>q>>s;
        map<int,set<int>> mp;
        VI sum(n+1);
        for(int i=0;i<SZ(s);i++) {
            sum[i+1]=sum[i]+(i&1?-1:1)*(s[i]=='+'?1:-1);
            mp[sum[i+1]+sum[i]].insert(i+1);
        }
        while(q--) {
            int l,r;cin>>l>>r;
            if(sum[r]-sum[l-1]==0) cout<<0<<'\n';
            else if((r-l+1)&1) {
                cout<<1<<'\n';
                cout<<*mp[sum[r]+sum[l-1]].lower_bound(l)<<'\n';
            }
            else {
                cout<<2<<'\n';
                cout<<l<<' '<<*mp[sum[r]+sum[l]].lower_bound(l+1)<<'\n';
            }
        }
    }
    return 0;
}
```