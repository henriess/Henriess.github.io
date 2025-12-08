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

Amazing Problem!. I'm so proud that I actually AC'd it. This problem was so deceivingly hard. The problem statement literally took my soul away given how much information it had. Funny enough, the problem was actually surprisingly easy to theory. Unironically the complicated problem statement actually gave a very straightforward solution. Since the problem is so complicated, there is no straightforward greedy so multi-state dijkstra is the only way to handle it. And simply bsta since its a maximising problem. Implementation on the other hand was a nightmare. I literally RTE'd and MLE'd so many times before randomly changing stuff and Ac'd using pragma's 🤣🤣🤣. My solution barely squeezed within the time limit tho, so I wonder if it is intended.

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
h \times w \times hw \times hw \quad \text{(MLE)}
$$


With modulo:

$$
h \times w \times 4 \times 4 \quad \text{(safe)}
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

## Code
{% raw %}
```cpp
#include <bits/stdc++.h>
using namespace std;
#pragma GCC target ("avx2")
#pragma GCC optimization ("O3")
#pragma GCC optimization ("unroll-loops")
vector<vector<pair<char,char>>> grid;
long long ans = 0;
long long h,w,a,b;
long long r;
vector<vector<vector<vector<long long>>>> dist;//Max net reward to node i with j red rotations and k blue rotations 
vector<pair<long long,long long>> moves = {{0,1},{0,-1},{1,0},{-1,0}};
bool isvalid(long long i,long long j){
	if (i>=h|| i<0 || j>=w || j<0){
		return false;
	}	
	return true;
}
unordered_map<char,long long> um;
bool check(long long mid){
	dist.assign(h + 5,vector<vector<vector<long long>>>(w + 5,vector<vector<long long>>(5,vector<long long>(5, LLONG_MAX))));
	priority_queue<vector<long long>,vector<vector<long long>>,greater<vector<long long>>> pq;
	dist[a][b][0][0] = 0;
	pq.push({0,a,b,0,0});//start point is a,b with net reward 0 at the start node and 0 red rotations and blue rotations so far 
	while (!pq.empty()){
		long long cr = pq.top()[1];//current row 
		long long cc = pq.top()[2];//current col 
		long long distance = pq.top()[0];// distance covered 
		long long redrotations = pq.top()[3];
		long long bluerotations = pq.top()[4];
		
		pq.pop();
		if (dist[cr][cc][redrotations][bluerotations] < distance){
			continue;
		}
		//leave the cell
		if (grid[cr][cc].second == 'R'){
			//blue cells will rotate their direction by 90 
			bluerotations += 1;
			bluerotations = bluerotations % 4;
		}
		else{
			redrotations += 1;
			redrotations = redrotations % 4;
		}
		for(auto move : moves){
			long long nr = cr + move.first;//new row
			long long nc = cc + move.second; // newcol
			
			if (!isvalid(nr,nc)){
				continue;
			}
			long long weight = 0;
			//check if move corresponds to the direction arrow is pointing 
			char arrow = 'a';
			long long save = um[grid[cr][cc].first];//number representation of the arrow 
			//handle the direction change 
			if (grid[cr][cc].second == 'R'){
				save += redrotations;
				save = save % 4;
			}
			else{
				save += bluerotations;
				save = save % 4;
			}
			
			for (auto [key,val] : um){
				if (save == val){
					arrow = key;
				}
			}
			bool direction = true;
			if (arrow == '>'){
				if (move.first == 0 && move.second == 1){
					//add -1 to the penalty 
					weight = 1;
				}
				else{
					weight = mid;
					direction = false;
				}
			}
			if (arrow == '<'){
				if (move.first == 0 && move.second == -1){
					//add -1 to the penalty
					weight = 1;
				}
				else{
					weight = mid;
					direction = false;
				}
			}
			if (arrow == 'v'){
				if (move.first == 1 && move.second == 0){
					weight = 1;
				}
				else{
					weight = mid;
					direction = false;
				}
			}
			if (arrow == '^'){
				if (move.first == -1 && move.second == 0){
					weight = 1;
				}
				else{
					weight = mid;
					direction = false;
				}
			}
			long long newdist = distance + weight;
				
			if (newdist < dist[nr][nc][redrotations][bluerotations]){
				dist[nr][nc][redrotations][bluerotations] = newdist;
				pq.push({newdist,nr,nc,redrotations,bluerotations});
			}
		}
	}
	for(int i = 0;i<h;i++){
		for(int j = 0;j<w;j++){
			long long best = LLONG_MAX;
			for(int k = 0;k<=3;k++){
				for(int z = 0;z <=3;z++){
					best = min(best, dist[i][j][k][z]);
				}
			}
			if (best >= r){
				return false;
			}
		}
	}
	return true;
}
void bsta(){
	long long lb = 0;
	long long ub = r;
	long long mid = -1;
	while (lb <= ub){
		mid = (lb + ub) / 2;
		if (check(mid)){
			//increase 
			lb = mid + 1;
			ans = max(ans,mid);
		}
		else{
			ub = mid - 1;
		}
	}
}
int main() {
    ios_base::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
	cin >> h >> w >> a >> b >> r;
	grid.resize(h+5,vector<pair<char,char>>(w+5));


	for(int i = 0;i<h;i++){
		string s;cin >> s;
		for(int j = 0;j<w;j++){
			grid[i][j].first = s[j];
		}
	}
	for(int i = 0;i<h;i++){
		string s2;cin >> s2;
		for(int j = 0;j<w;j++){
			grid[i][j].second = s2[j];
		}
	}
	//map each direction to a number 
	um['^'] = 0;
	um['>'] = 1;
	um['v'] = 2;
	um['<'] = 3;
	if (!check(0)){
		cout << -1;
		return 0;
	}
	if (check(1e9)){
		cout << -2;
		return 0;
	}
	bsta();
	
	cout << ans;
}
```


