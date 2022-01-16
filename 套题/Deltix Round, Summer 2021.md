---
title: Deltix Round, Summer 2021
date: 2021-10-01 19:44:29
tags: [ST 表]
categories: 套题
mathjax: true
---

| Solved |   A   |   B   |   C   |   D   |   E   |   F   |   G   |   H   |
| :----: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
|  5/8   |   Ø   |   Ø   |   Ø   |   Ø   |   Ø   |   -   |   -   |   -   |

# 题目链接

<https://codeforces.com/contest/1556>

<!--more-->

# Problem A. A Variety of Operations

**题意：**

$a,b$ 初始值为 $0$，每次选择一个正整数 $k$，可执行如下操作之一：

1. $a+k,b+k$。
2. $a+k,b-k$。
3. $a-k,b+k$。

最少执行几次操作使 $a=c,b=d$。

**思路：**

1. 当 $c=d$ 时，若 $c=0$，无需操作，否则执行一次操作 $1$。
2. 当 $c+d$ 为偶数时，执行一次操作 $1$，使 $a=b=(c+d)/2$，再进行一次操作2或3，共两次操作。
3. 当 $c+d$ 为奇数时，无解。

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
        int c,d;cin>>c>>d;
        if(!c&&!d) cout<<0<<'\n';
        else if(c==d) cout<<1<<'\n';
        else if((c+d)&1) cout<<-1<<'\n';
        else cout<<2<<'\n';
    }
    return 0;
}
```

# Problem B. Take Your Places!

**题意：**

长度为 $n$ 的 序列，每次可以交换两个相邻的元素，求最少交换多少次使序列中不存在相邻的两个元素奇偶性相同。

**思路：**

- 当奇数个数与偶数个数的差的绝对值大于 $1$ 时，无解。
- 当奇数个数大于等于偶数个数时，第一个数可以为奇数，按奇数的位置从小到大在下标为奇数的位置放奇数，奇数放完，偶数也放完了。
- 当奇数个数小于等于偶数个数时，第一个数可以为偶数，按偶数的位置从小到大在下标为奇数的位置放偶数，偶数放完，奇数也放完了。

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
        int n;cin>>n;
        VI a(n+1),odd,even;
        for(int i=1;i<=n;i++) cin>>a[i];
        for(int i=1;i<=n;i++) {
            if(a[i]&1) odd.PB(i);
            else even.PB(i);
        }
        if(abs(SZ(odd)-SZ(even))>=2) cout<<-1<<'\n';
        else {
            LL res=0x3f3f3f3f3f3f3f3f;
            if(SZ(odd)>=SZ(even)) {
                int pos=1;
                LL t=0;
                for(auto x:odd) t+=abs(x-pos),pos+=2;
                res=min(res,t);
            }
            if(SZ(even)>=SZ(odd)) {
                int pos=1;
                LL t=0;
                for(auto x:even) t+=abs(x-pos),pos+=2;
                res=min(res,t);
            }
            cout<<res<<'\n';
        }
    }
    return 0;
}
```

# Problem C. Compressed Bracket Sequence

**题意：**

给一个长度为 $n$ 的序列 $c$，$i$ 为奇数时，$c_i$ 为左括号的数量，$i$ 为偶数时，$c_i$ 为右括号的数量，求有多少合法括号序列。

- $1\le n\le 1000,1\le c_i\le 1e9$

**思路：**

枚举左括号，再向右遍历，遇到右括号时统计答案。

对于左端为 $i$，右端为 $i+2k+1$ 的括号序列，在统计合法序列时，对于 $[i+1,i+2k]$ 内的括号一定包含于合法序列中，而 $i$ 与 $i+2k+1$ 中应取括号数量为变量。

遇到左括号加上数量，遇到右括号减去数量，记前缀和为 $s$，$i$ 为奇数：

- 对于 $i$ 与 $i+1$，如 `(()` 这样的括号序列，左端的可选区间为 $[1,min(c_i,c_{i+1})]$，最终剩下 $l=s_{i+1}$ 个左括号。
- 对于 $i$ 与 $i+2k+1$，如 $k=1,c=\{3,1,1,3\}$： `((()()))` 这样的括号序列：

  左端在经过第一次右括号匹配后剩下 $l=s_2$ 个左括号，左端可选区间为$[0,l-s_4]$。

  若 $l-s_{i+2k+1}<0$，说明 $>i$ 的位置的左括号未匹配完，那么无法以 $i$ 为左端构成合法的括号序列。

  每次统计完答案后，要更新左端剩余括号数量：$l=min(l,s_{i+2k+1})$。

- 当 $s$ 为负时不合法，$s$ 取 $0$，统计完答案，直接退出，此时无法再向右延伸。

时间复杂度：$O(n^2)$。

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
const int N=1e3+5;
int a[N];
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int n;cin>>n;
    LL res=0;
    for(int i=1;i<=n;i++) cin>>a[i];
    for(int i=1;i<=n;i+=2) {
        LL sum=a[i],l=a[i];
        for(int j=i+1;j<=n;j+=2) {
            sum-=a[j];
            if(j==i+1) res+=min(c_i,c_j);
            else res+=max(0ll,l-max(0ll,sum)+1);
            if(sum<0) break;
            l=min(l,sum);
            sum+=a[j+1];
        }
    }
    cout<<res<<'\n';
    return 0;
}
```

# Problem D. Take a Guess

**题意：**
交互题，长度为 $n$ 的数组，可以作最多 $2n$ 次询问，每次可以询问 $a_i\&a_j$ 或 $a_i|a_j$，求第 $k$ 小。

**思路：**

$a+b=a\&b+a|b$

求出 $a_i+a_{i+1}$，共 $2n-2$ 次询问。

再求出 $a_1+a_3$，共 $2$ 次询问。

通过 $a_1+a_2+a_1+a_3-a_2-a_3$ 求出 $a_1$，这样整个数组都能求出来了。

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
const int N=1e4+5;
int a[N],sum[N];
int main() {
    int n,k;cin>>n>>k;
    for(int i=1;i<n;i++) {
        cout<<"and"<<' '<<i<<' '<<i+1<<endl;
        cin>>sum[i];
    }
    for(int i=1;i<n;i++) {
        cout<<"or"<<' '<<i<<' '<<i+1<<endl;
        int t;cin>>t;
        sum[i]+=t;
    }
    cout<<"and"<<' '<<1<<' '<<3<<endl;
    cin>>sum[0];
    cout<<"or"<<' '<<1<<' '<<3<<endl;
    int t;cin>>t;
    sum[0]+=t;
    a[1]=(sum[0]+sum[1]-sum[2])/2;
    for(int i=2;i<=n;i++) a[i]=sum[i-1]-a[i-1];
    sort(a+1,a+1+n);
    cout<<"finish"<<' '<<a[k]<<endl;
    return 0;
}
```

# Problem E. Equilibrium

**题意：**

长度为 $n$ 的数组 $a,b$，$q$ 次 询问 $l,r$，你需要计算最小操作数使 $a_i=b_i$ $(l\le i\le r)$。

每次操作为选择偶数个 $pos$ $(l\le pos \le r)$，使 $a_{pos_1},a_{pos_3},a_{pos_5},\dots$ 加一，$b_{pos_2},a_{pos_4},a_{pos_6},\dots$ 加一。

- $2\le n\le 1e5,1\le q\le 1e5,0\le a_i,b_i\le 1e9$。

**思路：**

记 $c_i=b_i-a_i$，若 $c_i$ 为正，表示需要给 $a_i$ 加，若 $c_i$ 为负，表示需要给 $b_i$ 加，记 $c$ 的前缀和为 $s$：

- 因为每次只能选偶数个 $pos$，所以给 $a$ 加的值等于给 $b$ 加的值，那么 $s_r-s_{l-1}\neq 0$ 时，无解。
- 当 $s_i-s_{l-1}<0$ 时，说明到当前位置要给 $b$ 加的操作数大于给 $a$ 加，那么此时操作必须以给 $b$ 加为开头，不符合操作，无解。
- 最少操作数为 $\max\{s_i-s_{l-1}\}$，模拟一下很容易理解。

ST 表查询区间最大值和最小值即可。

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
LL a[N],s[N],mx[25][N],mn[25][N];
int hightbit(int x) {return 31-__builtin_clz(x);}
void init(int n) {
    for(int i=1;i<=n;i++) mx[0][i]=mn[0][i]=s[i];
    int t=hightbit(n);
    for(int i=1;i<=t;i++)
        for(int j=1;j+(1<<i)-1<=n;j++) {
            mx[i][j]=max(mx[i-1][j],mx[i-1][j+(1<<(i-1))]);
            mn[i][j]=min(mn[i-1][j],mn[i-1][j+(1<<(i-1))]);
        }
}
LL query1(int l,int r) {
    int k=hightbit(r-l+1);
    return max(mx[k][l],mx[k][r-(1<<k)+1]);
}
LL query2(int l,int r) {
    int k=hightbit(r-l+1);
    return min(mn[k][l],mn[k][r-(1<<k)+1]);
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int n,q;cin>>n>>q;
    for(int i=1;i<=n;i++) cin>>a[i];
    for(int i=1;i<=n;i++) {
        int b;cin>>b;
        a[i]=b-a[i];
        s[i]=s[i-1]+a[i];
    }
    init(n);
    for(int i=1;i<=q;i++) {
        int l,r;cin>>l>>r;
        LL mxx=query1(l,r),mnn=query2(l,r);
        if(mnn-s[l-1]<0||s[r]-s[l-1]!=0) cout<<-1<<'\n';
        else cout<<mxx-s[l-1]<<'\n';
    }
    return 0;
}
```