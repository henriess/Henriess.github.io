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
**Link:** https://codebreaker.xyz/problem/reinforcement
\

I LOVE THIS PROBLEM. I'm so proud that I actually AC'd it. This problem was so deceivingly hard. The problem statement literally took my soul away given how much information it had. Funny enough, the problem was actually surprisingly easy to theory. Unironically the complicated problem statement actually gave a very straightforward solution. Since the problem is so complicated, there is no straightforward greedy so multi-state dijkstra is the only way to handle it. And simply bsta since its a maximising problem. Implementation on the other hand was a nightmare. I literally RTE'd and MLE'd so many times before randomly changing stuff and Ac'd using pragma's 🤣🤣🤣. My solution barely squeezed within the time limit tho, so I wonder if it is intended.

- No greedy works since too many conditions  
- Multi-state Dijkstra is the only approach that can handle all the information
- Since the answer is a **maximum**, use **bsta**

## Observation 1 — The maximum non-negative integer \(k\) is monotonic

If a value of \(k\) is feasible, then any smaller value of \(k\) is also feasible.  
Therefore, the answer is **monotonic**, and we should immediately think of bsta

The question is: **How do we check feasibility for a given \(k\)?**

---

## Observation 2 — The problem becomes a shortest-path (Dijkstra) problem

Each cell contributes either:

- a **penalty** (adding \(1\) or \(k\)), or  
- a **reward** (subtracting \(r\)).

Thus the total cost behaves exactly like a shortest-path problem:

$$
\text{dist}[i][j] = \text{minimum cost to reach cell }(i, j).
$$

So we run **Dijkstra** on the grid.

---

## Observation 3 — Convert arrows into integers for easy rotation

Arrows ↑ ↓ ← → rotate many times, so convert them into integers:

```cpp
map<char, int> dir = {
    {'^', 0}, {'>', 1}, {'v', 2}, {'<', 3}
};
```

A rotation becomes:

$$
(\text{direction} + \text{rotations}) \bmod 4.
$$

---

## Observation 4 — To keep track of the direction changes, add rotation states to the Dijkstra

When leaving a **red** cell, all arrows in **blue** coloured cells rotate.  
When leaving a **blue** cell, all arrows in **red** coloured cells rotate.

Therefore the Dijkstra state must include both rotation counters:

$$
\text{dist}[i][j][\text{redrot}][\text{bluerot}]
$$

Where:

$$
\begin{aligned}
\text{redrot} &= \text{number of times red cells rotated} \\
\text{bluerot} &= \text{number of times blue cells rotated}
\end{aligned}
$$


### Why modulo 4?

Arrows cycle every four rotations:

$$
U \to R \to D \to L \to U.
$$

Thus we only need:

```cpp
redrot %= 4;
bluerot %= 4;
```

Without this:

- State size becomes

$$
n \times m \times 150^2 \times 150^2 \quad \text{(MLE)}
$$


With modulo:

$$
n \times m \times 4 \times 4 \quad \text{(safe)}
$$

---
 
## Observation 5 — How to determine answers -1 and -2

We need to distinguish two special cases explicitly.

### Case 1 — Output **-1**  
If the goal is unreachable even when \(k = 0\), then it is **never** reachable.

### Case 2 — Output **-2**  
If the goal is still reachable even with extremely large \(k\), then no finite \(k\) is a valid upper bound.

To detect this:

1. **Run Dijkstra with \(k = 0\)**  
   - If the destination is unreachable → answer = **-1**

2. **Run Dijkstra with a very large \(k\)**  
   - If the destination is still reachable → answer = **-2**

Otherwise, the smallest feasible \(k\) from binary search is the final answer.

---



