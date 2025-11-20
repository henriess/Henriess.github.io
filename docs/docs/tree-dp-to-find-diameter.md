---
title: "Tree DP to Find Diameter"
nav_order: 3
---

# Tree DP to Find Diameter

This is my first post after a gruelling A-Level period 😭.  
Unfortunately, I don't find DP on trees trivial, so this post is mainly to teach how it works.
 
For every node \( i \), compute the largest distnance from \( i \) to any other node in the tree.

To do this, we maintain 2 dp arrays:

### `down[i]`
The distance from node \( i \) to the **farthest node in its own subtree**.

### `up[i]`
The distance from node \( i \) to the **farthest node *outside* its subtree**.

Once we know both, the answer for a node is simply:

\[
\text{maxDist}(i) = \max(\text{down}[i],\ \text{up}[i])
\]

---

# ## 1. Computing `down[]` — bottom-up DFS

`down[i]` is easy to compute.  
For each child \( x \), the longest path from \( i \) downwards is:

\[
\text{down}[i] = \max(\text{down}[i],\ \text{down}[x] + 1)
\]

Code:

```cpp
void dfs(long long u, long long p) {
    for (auto x : adjlist[u]) {
        if (x == p) continue;

        dfs(x, u);
        down1[u] = max(down1[u], down1[x] + 1);
    }
}

## 2. Computing `up[]` — rerooting (top-down DP)

The `up` array is more unintuitive than `down`.  
`up[u]` stores the length of the longest path from `u` to a node **outside** `u`'s subtree.

To compute `up[u]`, for each node `u` we need to know the two largest downward distances among its children:

- `best` → the largest value of `down[child] + 1`
- `secondbest` → the second largest value of `down[child] + 1`

We track two values because when we are computing `up[x]` for a child `x`:

- If `x` is the child that gave the `best` value, we must use `secondbest` (to avoid going back into `x`'s own subtree).
- Otherwise, we can use `best`.

For a child `x` of `u`:

- Start with `up[x] = up[u] + 1` (going further up through the parent).
- Then, if there is a good downward path from some *other* child of `u`, we also consider:
  - `best + 1` if that best path is not from `x`'s subtree, or
  - `secondbest + 1` if `x` contributed the best and we must avoid it.


```cpp
vector<long long> up1;

void dfs2(long long u,long long p) { // rerooting dfs 
    long long best = -1;
    long long secondbest = -1;

    // find the best and second best downward distances from u's children
    for (auto x : adjlist[u]) {
        if (x == p) {
            continue;
        }
        long long dist = down1[x] + 1;
        if (dist > best) {
            secondbest = best;
            best = dist;
        }
        else if (dist <= best && dist > secondbest) {
            secondbest = dist;
        }
    }

    // use these to compute up[] for each child
    for (auto x : adjlist[u]) {
        if (x == p) {
            continue;
        }

        // path going further up through parent
        up1[x] = up1[u] + 1;

        // path going sideways into a sibling subtree
        if (best != -1 && down1[x] + 1 != best) { 
            // if the best distance is not in x's subtree, use it
            up1[x] = max(up1[x], best + 1);
        }
        else if (secondbest != -1 && down1[x] + 1 == best) {
            // else, use the second best distance
            up1[x] = max(up1[x], secondbest + 1);
        }

        // recurse down
        dfs2(x,u);
    }
}
