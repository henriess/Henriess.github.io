---
title: "Singapore Informatics League 2025 Editorial"
nav_order: 3
---

So far, I only managed to solve **5 of the Level 3 and 4 problems**, so this editorial is temporary until I solve more.

---

# Problem 18 — Jokers  
**Link:** https://codebreaker.xyz/problem/jokers

This problem seemed pretty classic. I don't really see any mathematical property that would allow
us to choose the jokers optimally, so math is out of the option.

As always, when meeting a **minimize or maximize** problem, think of **binary search on the answer** first.

---

## 🔍 Observation 1 — The number of jokers is monotonic

If I pick more jokers, my score increases.  
Hence, I can binary search for the **minimum** number of jokers to pick.

- If my score is **lower** than the target, I increase the number of jokers.  
- If my score is **higher**, I can try fewer jokers.

This allows us to **fix the number of jokers** and perform a **greedy check**.

---

## 🔍 Observation 2 — Always take jokers with the highest mult and chip

This is self-explanatory — higher mult and chip gives a higher score.

Sort both joker lists in **decreasing order**.

To check whether a fixed number of jokers is feasible, we need to try all possibilities quickly.  
Prefix sums allow us to obtain sums in **O(1)** time.

- Create prefix sums of chip jokers and mult jokers after sorting.  
- When trying `x` chip jokers, we can compute the contribution of the remaining mult jokers instantly.

---

### **Overall time complexity:**  
`O(nlogn)`

---


# ✅ Code (C++)

```cpp
#include <bits/stdc++.h>
using namespace std;

long long ans = LLONG_MAX;
long long n, m, x;
vector<long long> a;
vector<long long> b;
vector<long long> pa;
vector<long long> pb;
long long x1 = 0;

// check if mid jokers is feasible
bool check(long long mid) {
    if (mid == 0) {
        return 1 >= x;
    }
    long long cur = 0;

    for (int i = 0; i < n; i++) {
        long long temp = pa[i] + 1;
        if (i + 1 > mid) break;

        long long rem = mid - (i + 1);
        if (rem > 0 && rem - 1 < m) {
            temp = temp * (pb[rem - 1] + 1);
        } else {
            temp = pa[i] + 1;
        }
        cur = max(cur, temp);
    }

    // case where we use 0 chip jokers and only mult jokers
    if (mid - 1 < m) {
        cur = max(cur, pb[mid - 1] + 1);
    }

    return cur >= x;
}

int main() {
    ios_base::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);

    long long finalans = 0;
    long long t; cin >> t;
    long long testcase = 1;

    while (t--) {
        cin >> n >> m >> x;
        ans = LLONG_MAX;

        a.resize(n);
        for (int i = 0; i < n; i++) cin >> a[i];
        b.resize(m);
        for (int i = 0; i < m; i++) cin >> b[i];

        sort(a.begin(), a.end(), greater<long long>());
        sort(b.begin(), b.end(), greater<long long>());

        pa.resize(n);
        pb.resize(m);

        pa[0] = a[0];
        for (int i = 1; i < n; i++)
            pa[i] = pa[i - 1] + a[i];

        pb[0] = b[0];
        for (int i = 1; i < m; i++)
            pb[i] = pb[i - 1] + b[i];

        long long lb = 0, ub = n + m, mid;

        while (lb <= ub) {
            mid = (lb + ub) / 2;
            if (check(mid)) {
                ub = mid - 1;
                ans = min(ans, mid);
            } else {
                lb = mid + 1;
            }
        }

        if (ans == LLONG_MAX)
            finalans += -1 * testcase;
        else
            finalans += ans * testcase;

        testcase++;
    }
    cout << finalans;
}
```
---

# Problem 19 — Musical Chairs  
**Link:** https://codebreaker.xyz/problem/musicalchairs

I found this problem absolutely cancerous given the number of indexing bugs I faced and spent almost an hour debugging and doubting if my greedy was wrong.  
I think the intended solution is O(n) but my solution is O(m)?

---

## Observation 1 — People with the same direction should move together

If 1 person moves, there will be space for the next person with the same direction.  
Hence, instead of caring about individual people, we compress consecutive people with the same direction into one segment and iterate through segments instead 

---

## Observation 2 — When two adjacent segments have opposing directions, only one should move

There is no point in having both segments move as the number of empty seats is the same.  
This brings the main greedy question: which segment should move?

---

## Observation 3 — When two adjacent segments have opposing directions, the segment with more people should move

More people → more moves → higher contribution to the answer.  
Thus, when R and L conflict, the segment with more people should move.

The implementation is pretty cancerous for my method, but the idea is straightforward I think?

---

# Implementation

---

## Step 1 — Form the segments

```cpp
long long n, m; 
cin >> n >> m;

vector<pair<long long, char>> people(m);
for (int i = 0; i < m; i++) {
    char d;
    long long c;
    cin >> d >> c;
    people[i] = {c, d};
}

sort(people.begin(), people.end());

vector<vector<long long>> compressed;
long long cur = 1;

for (int i = 1; i < m; i++) {
    if (people[i].second == people[i-1].second) {
        cur += 1;
    } else {
        compressed.push_back({cur, people[i-1].second, i - cur, i - 1});
        cur = 1;
    }
}

// push the last segment
compressed.push_back({cur, people[m-1].second, m - cur, m - 1});
```

---
# Step 2 : Move each person so each person in a segment is beside one another 
This allows for easy implementation 
```cpp
//within each segment, move the people so they are together 
		long long ans = 0;
		for(int i = 0;i<compressed.size();i++){
			long long start = compressed[i][2];
			long long end = compressed[i][3];
			if (compressed[i][1] == 'R'){
				long long moveto = people[end].first;
				long long temp = 0;
				for (int j = end;j>=start;j--){
					ans += moveto - people[j].first - temp;
					temp += 1;
				}
			}
			else{
				long long moveto = people[start].first;
				long long temp = 0;
				for(int j = start;j<=end;j++){
					ans += people[j].first - moveto - temp;
					temp += 1;
				}
			}
		}
```

---
# Step 3 : Observations 2 and 3, the main decision making process 

```cpp
for (int i = 0; i < compressed.size(); i++) {

    // leftmost segment moving left
    if (i == 0 && compressed[i][1] == 'L') {
        long long start = compressed[i][2];
        ans += compressed[i][0] * (people[start].first - 1);
    }

    // rightmost segment moving right
    else if (i == compressed.size() - 1 && compressed[i][1] == 'R') {
        long long end = compressed[i][3];
        ans += compressed[i][0] * (n - people[end].first);
    }

    // conflict: current R, next L
    else if (i < compressed.size() - 1 && compressed[i][1] == 'R') {
        if (compressed[i+1][1] == 'L') {

            // larger segment moves
            if (compressed[i][0] >= compressed[i+1][0]) {
                long long end = compressed[i][3];
                long long moveto = people[compressed[i+1][2]].first;

                ans += compressed[i][0] * (moveto - people[end].first - 1);
                i++; // skip next segment
            }
        }
    }

    // conflict: previous R, current L
    else if (i > 0 && compressed[i][1] == 'L') {
        if (compressed[i-1][1] == 'R') {

            if (compressed[i][0] >= compressed[i-1][0]) {
                long long start = compressed[i][2];
                long long moveto = people[compressed[i-1][3]].first;

                ans += compressed[i][0] * (people[start].first - moveto - 1);
            }
        }
    }
}
```
---
