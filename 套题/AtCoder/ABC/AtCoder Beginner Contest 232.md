---
title: AtCoder Beginner Contest 232
date: 2022-01-06 23:19:56
tags: []
categories: 套题
mathjax: true
---

| Solved |   A   |   B   |   C   |   D   |   E   |   F   |   G   |   H   |
| :----: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
|  5/8   |   O   |   O   |   O   |   O   |   Ø   |   -   |   -   |   -   |

# 题目链接

<https://atcoder.jp/contests/abc232>

<!--more-->

# Problem A. QQ solver

**题意：**

**思路：**

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
    string s;
    cin >> s;
    cout << (s[0] - '0') * (s[2] - '0') << '\n';
    return 0;
}
```

# Problem B. Caesar Cipher

**题意：**

**思路：**

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
    string a, b;
    cin >> a >> b;
    int t = ((a[0] - b[0]) % 26 + 26) % 26;
    for (int i = 1; i < SZ(a); i++) {
        if (((a[i] - b[i])% 26 + + 26) % 26 != t) {
            return cout << "No" << '\n', 0;
        }
    }
    cout << "Yes" << '\n';
    return 0;
}
```

# Problem C. Graph Isomorphism

**题意：**

**思路：**

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
const int N = 10;
int n, m, a[N];
bool f1[N][N], f2[N][N];
bool check() {
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= n; j++) {
            if (!f1[i][j]) {
                continue;
            }
            if (!f2[a[i]][a[j]]) {
                return false;
            }
        }
    }
    return true;
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cin >> n >> m;
    for (int i = 1; i <= n; i++) {
        a[i] = i;
    }
    for (int i = 1; i <= m; i++) {
        int a, b;
        cin >> a >> b;
        f1[a][b] = f1[b][a] = true;
    }
    for (int i = 1; i <= m; i++) {
        int a, b;
        cin >> a >> b;
        f2[a][b] = f2[b][a] = true;
    }
    do {
        if (check()) {
            return cout << "Yes" << '\n', 0;
        }
    } while(next_permutation(a + 1, a + 1 + n));
    cout << "No" << '\n';
    return 0;
}
```

# Problem  D. Weak Takahashi

**题意：**

**思路：**

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
const int N = 105;
int mx[N][N];
string s[N];
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int n, m;
    cin >> n >> m;
    for (int i = 0; i < n; i++) {
        cin >> s[i];
    }
    int res = 0;
    mx[0][0] = 1;
    for (int i  = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (s[i][j] == '#') {
                continue;
            }
            if (i - 1 >= 0 && mx[i - 1][j] >= 1) {
                mx[i][j] = max(mx[i][j], mx[i - 1][j] + 1);
            }
            if (j - 1 >= 0 && mx[i][j - 1] >= 1) {
                mx[i][j] = max(mx[i][j], mx[i][j - 1] + 1);
            }
            res = max(res, mx[i][j]);
        }
    }
    cout << res << '\n';
    return 0;
}
```

# Problem E. Rook Path

**题意：**

$H*W$ 的二维方格中，起始点为 $(x_1,y_1)$

**思路：**

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
const LL MOD = 998244353;
const int N = 1e6 + 5;
LL fac[N], invfac[N], dp1[N][2], dp2[N][2];
LL qpow(LL a, LL b) {
    LL ret = 1;
    while (b) {
        if(b & 1) {
            ret = ret * a % MOD;
        }
        a = a * a % MOD;
        b >>= 1;
    }
    return ret;
}
void init(int n) {
    fac[0] = 1;
    for (int i = 1; i <= n; i++) {
        fac[i] = fac[i - 1] * i % MOD;
    }
    invfac[n] = qpow(fac[n], MOD - 2);
    for (int i = n - 1; ~i; i--) {
        invfac[i] = invfac[i + 1] * (i + 1) % MOD;
    }
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    init(1e6);
    int n, m, k, x1, y1, x2, y2;
    cin >> n >> m >> k >> x1 >> y1 >> x2 >> y2;
    dp1[0][0] = (x1 == x2);
    dp1[0][1] = (x1 != x2);
    for (int i = 1; i <= k; i++) {
        dp1[i][0] = dp1[i - 1][1];
        dp1[i][1] = (dp1[i - 1][1] * (n - 2) % MOD + dp1[i - 1][0] * (n - 1)) % MOD;
    }
    dp2[0][0] = (y1 == y2);
    dp2[0][1] = (y1 != y2);
    for (int i = 1; i <= k; i++) {
        dp2[i][0] = dp2[i - 1][1];
        dp2[i][1] = (dp2[i - 1][1] * (m - 2) % MOD + dp2[i - 1][0] * (m - 1)) % MOD;
    }
    LL res = 0;
    for (int i = 0; i <= k; i++) {
        res = (res + dp1[i][0] * dp2[k - i][0] % MOD * fac[k] % MOD * invfac[i] % MOD * invfac[k - i]  % MOD) % MOD;
    }
    cout << res << '\n';
    return 0;
}
```

# Problem F. Simple Operations on Sequence

**题意：**

长度为 $n$ $(n\le 18)$ 的数组 $A,B$， 将 $A$ 操作变成 $B$，有如下两种操作：

1. 对一个元素 $+1$ 或 $-1$，花费 $x$；
2. 交换相邻两个元素，花费 $y$。

求最小花费。

**思路：**

显然这两种操作是独立的，即通过交换得到一个排列，在执行操作 $1$。

最小交换次数为逆序对个数。

因为 $n$ 很小，用状压 DP 解决。


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
const int N = 20;
int n, a[N], b[N], popcount[1 << N];
LL x, y, dp[1 << N];
int f(int a, int b) {
    int ret = 0;
    for (int i = 0; i < n; i++) {
        if (a & (1 << i) && i > b) {
            ++ret;
        }
    }
    return ret;
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cin >> n >> x >> y;
    for (int i = 0; i < n; i++) {
        cin >> a[i];
    }
    for (int i = 0; i < n; i++) {
        cin >> b[i];
    }
    for (int i = 1; i < 1 << n; i++) {
        popcount[i] = popcount[i & (i - 1)] + 1;
    }
    memset(dp, 0x3f, sizeof dp);
    dp[0] = 0;
    for (int i = 1; i < 1 << n; i++) {
        for (int j = 0; j < n; j++) {
            if (i & (1 << j)) {
                dp[i] = min(dp[i], dp[i ^ (1 << j)] + y * f(i, j) + x * abs(a[j] - b[popcount[i] - 1]));
            }
        }
    }
    cout << dp[(1 << n) - 1] << '\n';
    return 0;
}
```