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
