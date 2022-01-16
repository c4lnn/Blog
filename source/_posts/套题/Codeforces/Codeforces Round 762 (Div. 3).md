---
title: Codeforces Round 762 (Div. 3)
date: 2021-12-28 00:44:48
tags: [贪心,并查集]
categories: 套题
mathjax: true
---

| Solved |   A   |   B   |   C   |   D   |   E   |   F   |   G   |   H   |
| :----: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
|  7/8   |   O   |   O   |   O   |   Ø   |   O   |   Ø   |   Ø   |   -   |

# 题目链接

<https://codeforces.com/contest/1619>

<!--more-->

# Problem A. Construct a Rectangle

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
    int T;
    cin >> T;
    while (T--) {
        string s;
        cin >> s;
        if (SZ(s) & 1) {
            cout << "NO" << '\n';
        } else {
            bool f = false;
            for (int i = 0; i < SZ(s) / 2; i++) {
                if (s[i] != s[i + SZ(s) / 2]) {
                    cout << "NO" << '\n';
                    f = true;
                    break;
                }
            }
            if (!f) {
                cout << "YES" << '\n';
            }
        }
    }
    return 0;
}
```

# Problem B. Berland Music

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
        VI a;
        for (int i = 1; i * i <= 1e9; i++) {
            a.PB(i * i);
        }
        for (int i = 1; i * i * i <= 1e9; i++) {
            a.PB(i * i * i);
        }
        sort(ALL(a));
        a.resize(unique(ALL(a)) - a.begin());
        int T;
        cin >> T;
        while (T--) {
            int n;
            cin >> n;
            cout << upper_bound(ALL(a), n) - a.begin() << '\n';
        }
        return 0;
    }
```

# Problem C. Wrong Addition

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
    int T;
    cin >> T;
    while (T--) {
        LL a, s, b = 0, power = 1;
        cin >> a >> s;
        bool f = false;
        while (s) {
            int x = a % 10, y = s % 10;
            a /= 10, s /= 10;
            if (x <= y) {
                b += (y - x) * power;
            } else {
                y += s % 10 * 10;
                s /= 10;
                if (x <= y && y - x <= 9) {
                    b += (y - x) * power;
                } else {
                    cout << -1 << '\n';
                    f = true;
                    break;
                }
            }
            power *= 10;
        }
        if (!f) {
            cout << (!a ? b : -1) << '\n';
        }
    }
    return 0;
}
```

# Problem D. New Year's Problem

**题意：**

$m$ 个商店，$n$ 个人， 第 $j$ 个人在第 $i$ 个商店得到的礼物的价值为 $p_{i,j}$。

可以选择 $n-1$ 个商店购买礼物，每个商店可以购买多份礼物，每个人只能获得一份礼物。

求这 $n$ 个人获得的礼物的最小值最大可能为多少。


**思路：**

二分答案。

答案合法的条件为至少有一个商店可以购买两份礼物且每个人都能获得礼物。

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
int m, n;
bool check(vector<VI> &p, int mid) {
    vector<bool> f(m + 1);
    bool ok = false;
    int cnt = 0;
    for (int j = 1; j <= n; j++) {
        bool fj = false;
        for (int i = 1; i <= m; i++) {
            if (p[i][j] >= mid) {
                if (!fj) {
                    fj = true;
                    ++cnt;
                }
                if (f[i]) {
                    ok = true;
                } else {
                    f[i] = true;
                }
            }
        }
    }
    if (cnt < n || !ok) return false;
    return true;
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int T;
    cin >> T;
    while (T--) {
        cin >> m >> n;
        vector<VI> p(m + 1, VI(n + 1));
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                cin >> p[i][j];
            }
        }
        int l = 0, r = 1e9;
        while (l < r) {
            int mid = l + r + 1 >> 1;
            if (check(p, mid)) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }
        cout << l << '\n';
    }
    return 0;
}
```

# Problem E. MEX and Increments

**题意：**

长度为 $n$ 的非负整数数组 $a$，每次操作能使其中一个元素 $+1$。

对于 $i$ 从 $0 \sim n$，求至少需要次操作使 $\operatorname{mex}(i)=i$。

**思路：**

因为每个值只要存在一次即可，那么剩余的数我们可以放入栈中，后面操作时每次都操作栈顶，这样的话操作次数最小。

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
        int n;
        cin >> n;
        VI cnt(n + 1), sum(n + 1);
        for (int i = 0; i < n; i++) {
            int x;
            cin >> x;
            if (x <= n) {
                ++cnt[x];
            }
        }
        sum[0] = cnt[0];
        for (int i = 1; i <= n; i++) {
            sum[i] = sum[i - 1] + cnt[i];
        }
        LL res = 0;
        bool f = false;
        stack<int> stk;
        for (int i = 0; i <= n; i++) {
            if (i && sum[i - 1] < i || f) {
                f = true;
                cout << -1 << " \n"[i == n];
            } else {
                cout << res + cnt[i] << " \n"[i == n];
                if (cnt[i] == 0) {
                    if (SZ(stk)) {
                        res += i - stk.top();
                        stk.pop();
                    } else {
                        f = true;
                    }
                } else {
                    for (int j = 2; j <= cnt[i]; j++) {
                        stk.push(i);
                    }
                }
            }
        }
    }
    return 0;
}
```

# Problem F. Let's Play the Hat?

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
void solve() {
    int n, m, k;
    cin >> n >> m >> k;
    int t = n % m, pre = 0;
    while (k--) {
        int l = pre + 1, r = pre;
        for (int i = 1; i <= t; i++) {
            cout << n / m + 1 << ' ';
            for (int j = 1; j <= n / m + 1; j++) {
                cout << ++r << " \n"[j == n / m + 1];
                if (r == n) {
                    r = 0;
                }
            }
            pre = r;
        }
        int x = r;
        for (int i = 1; i <= m - t; i++) {
            cout << n / m << ' ';
            for (int j = 1; j <= n / m; j++) {
                if (x == n) {
                    x = 0;
                }
                cout << ++x << " \n"[j == n / m];
            }
        }
    }
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int T;
    cin >> T;
    while (T--) {
        solve();
    }
    return 0;
}
```

# Problem G. Unusual Minesweeper

**题意：**

二维网格中有 $n$ 个雷，第 $i$ 个雷在 $t_i$ 秒爆炸，爆炸会带动垂直和水平距离 $k$ 内的雷爆炸。

每秒可以指定一个雷提前爆炸，求所有雷爆炸花费的最小时间。

**思路：**

分别考虑垂直和水平方向，维护连通块最早爆炸时间 $t$，按照 $t$ 从大到下指定连通块爆炸。

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
const int N = 2e5 + 5;
int x[N], y[N], t[N], fa[N];
int find(int x) {
    return x == fa[x] ? x : fa[x] = find(fa[x]);
}
void merge(int x, int y) {
    int fx = find(x), fy =find(y);
    if (fx == fy) {
        return;
    }
    fa[fx] = fy;
    t[fy] = min(t[fy], t[fx]);
}
void solve() {
    int n, k;
    cin >> n >> k;
    for (int i = 0; i < n; i++) {
        cin >> x[i] >> y[i] >> t[i];
        fa[i] = i;
    }
    VI a(n);
    iota(ALL(a), 0);
    sort(ALL(a), [&](int i, int j) {
        return x[i] != x[j] ? x[i] < x[j] : y[i] < y[j];
    });
    for (int i = 1; i < n; i++) {
        if (x[a[i]] == x[a[i - 1]] && y[a[i]] - y[a[i - 1]] <= k) {
            merge(a[i], a[i - 1]);
        }
    }
    sort(ALL(a), [&](int i, int j) {
        return y[i] != y[j] ? y[i] < y[j] : x[i] < x[j];
    });
    for (int i = 1; i < n; i++) {
        if (y[a[i]] == y[a[i - 1]] && x[a[i]] - x[a[i - 1]] <= k) {
            merge(a[i], a[i - 1]);
        }
    }
    VI v;
    for (int i = 0; i < n; i++) {
        if (fa[i] == i) {
            v.PB(t[i]);
        }
    }
    sort(ALL(v), greater<int>());
    int res = -1;
    for (auto x : v) {
        if (x > res) {
            ++res;
        }
    }
    cout << res << '\n';
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int T;
    cin >> T;
    while (T--) {
        solve();
    }
    return 0;
}
```