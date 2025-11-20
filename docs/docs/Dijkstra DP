---
title: "Dijkstra DP"
nav_order: 3
---

# Dijkstra DP

**Problem Links:**  
- Codebreaker: https://codebreaker.xyz/problem/cyberland_apio23  

A simple dijkstra would be insufficient as states would be needed to track the number of divide by 2 abilities used.

To modify the dijkstra, there would be 3 total transitions.

Let number of times divide by 2 ability is used be `k`.  
Let current node be `x`.  
Let previous node be `u`.  

---

## 1. If the country has no special ability

`dist}[x][k] = dist}[u][k] + edge_weight`

## 2. If the country has a divide-by-2 ability

`dist[x][k+1] = (dist[u][k] + edge_weight) / 2`


## 3. If the country allows passing time to become 0

`dist[x][k] = 0`

---

# Code

```cpp
void dijkstra(){
    dist[0][0] = 0;

    priority_queue<
        pair<double, pair<double, double>>,
        vector<pair<double, pair<double, double>>>,
        greater<pair<double, pair<double, double>>>
    > pq;

    pq.push({0,{0,0}}); // distance, num times divide by 2 is used, node

    while (!pq.empty()){
        double cn = pq.top().second.second;
        double w = pq.top().first;
        double num = pq.top().second.first;
        pq.pop();

        if (cn == h) continue;
        if (dist[cn][num] < w) continue;

        for(auto x : adjlist[cn]){
            double newnode = x.first;
            double weight = x.second;
            double newweight = w + weight;

            // Transition 1: normal edge
            if (dist[newnode][num] > newweight){
                dist[newnode][num] = newweight;
                pq.push({newweight,{num,newnode}});
            }

            // Transition 2: divide by 2 ability
            if (num + 1 <= k){
                if (abilities[newnode] == 2){
                    double temp = newweight / 2;
                    if (dist[newnode][num + 1] > temp){
                        dist[newnode][num + 1] = temp;
                        pq.push({temp,{num+1,newnode}});
                    }
                }
            }

            // Transition 3: reset to 0
            if (abilities[newnode] == 0){
                if (dist[newnode][num] > 0){
                    dist[newnode][num] = 0;
                    pq.push({0,{num,newnode}});
                }
            }
        }
    }
}
```
