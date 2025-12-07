---
title: "Dec Course Selection Test 2025 Editorial"
nav_order: 3
---

# Problem 1 : Evenorodd_ex  
**Link:** https://codebreaker.xyz/problem/evenorodd_ex  

Syntax problem.  
A number is even if:

```cpp
number % 2 == 0
```

and odd if:

```cpp
number % 2 != 0
```

---

# Problem 2 : FunnyBalls  
**Link:** https://codebreaker.xyz/problem/funnyballs  

I really liked this problem tbh.

## Observation 1 — Always pick the K highest balls  
The distance constraint does not matter for maximizing score.  
So simply sort all balls in **decreasing** order and select the largest K.

## Observation 2 — Output in zigzag order  
To satisfy the strictly decreasing distance condition, output chosen indices in:

- smallest, largest  
- 2nd smallest, 2nd largest  

This guarantees valid decreasing distances.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;

bool cmp(pair<long long,long long> a, pair<long long,long long> b){
    if (a.first == b.first) return a.second < b.second;
    return a.first > b.first;
}

int main() {
    ios_base::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);

    long long n, k;
    cin >> n >> k;
    vector<pair<long long,long long>> s(n);

    for (int i = 0; i < n; i++) {
        cin >> s[i].first;
        s[i].second = i;
    }

    sort(s.begin(), s.end(), cmp);

    long long ans = 0;
    for (int i = 0; i < k; i++) ans += s[i].first;

    vector<pair<long long,long long>> chosen;
    for (int i = 0; i < k; i++)
        chosen.push_back({s[i].second, s[i].first});

    cout << ans << "\n";

    sort(chosen.begin(), chosen.end());

    for (int i = 0; i < chosen.size() / 2; i++) {
        cout << chosen[i].first + 1 << " ";
        cout << chosen[chosen.size() - i - 1].first + 1 << " ";
    }

    if (chosen.size() % 2 != 0)
        cout << chosen[chosen.size() / 2].first + 1;
}
```

---

# Problem 3 : Photofinish  
**Link:** https://codebreaker.xyz/problem/photofinish  

## Observation  
If person **A** is behind person **B** in *both* photos, then A never overtook B.  
So A can **never** win.

We track the smallest “second-photo position” seen so far — this gives the number of possible winners.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios_base::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);

    long long n;
    cin >> n;
    vector<long long> a(n), pos(n);

    for (int i = 0; i < n; i++) {
        cin >> a[i];
        a[i]--;
        pos[a[i]] = i;
    }

    long long bestsecond = LLONG_MAX;
    long long ans = 0;

    for (int i = 0; i < n; i++) {
        if (pos[i] <= bestsecond) {
            ans++;
            bestsecond = pos[i];
        }
    }

    cout << ans;
}
```

---

# Problem 4 : Reinforcement Learning  

I **LOVE THIS PROBLEM**.  
I'm honestly so proud that I AC’d it.

The problem statement was extremely long and intimidating, but ironically the core idea is pretty clean:

- No greedy works → too many conditions  
- Multi-state Dijkstra is the only real approach  
- Since the answer is a **maximum**, use **binary search the answer (BSTA)**  

The *real* challenge is not the theory — it's the **implementation**, which was a complete nightmare.

I literally RTE’d and MLE’d countless times before randomly changing things and somehow AC’ing with pragmas 🤣🤣🤣  
My solution barely squeezed within the time limit, so I'm genuinely unsure if that's intended.

I’ll add the full explanation + code once I rewrite and clean it.

