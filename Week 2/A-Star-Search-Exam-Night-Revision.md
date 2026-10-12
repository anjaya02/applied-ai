# A* Search — Exam-Night Revision Notes

> **Goal:** Quickly revise the exam facts needed to solve A* search questions, especially questions with a city map, road distances, and a heuristic table.

## 1. What is A* Search?

**A* (A-star)** is an **informed search algorithm** that uses a **heuristic** (an estimate of the remaining cost) to help find a lowest-cost path to a goal.

**Most important formula:**

\[
f(n) = g(n) + h(n)
\]

| Symbol | Exam meaning | In simple English |
|---|---|---|
| `n` | Current node/state | The city we are evaluating |
| `g(n)` | Cost from **start** to `n` along the current best-known path | Actual distance travelled so far |
| `h(n)` | Estimated cost from `n` to **goal** | Estimated distance still left |
| `f(n)` | Estimated total path cost **through `n`** | `g(n) + h(n)` |

**Memory trick:** `g = gone so far`; `h = how much is left (estimated)`; `f = full estimated cost`.

## 2. How to Read an A* Map Question

1. **Find START and GOAL.** Do not automatically assume Arad is the start just because it appears on a familiar Romania map.
2. **Read road/edge numbers** to calculate `g(n)` (the **actual** cost from START).
3. **Read the heuristic table** to get `h(n)` (the **estimated** remaining cost to GOAL).
4. **Add:** `f(n) = g(n) + h(n)`.
5. **For the next expansion**, choose the **frontier/open-list node with the smallest `f(n)`**.
6. Repeat until the goal is selected for expansion (under the usual A* goal-test rule).

**Very important:** If the GOAL is Bucharest, all values in a *straight-line distance to Bucharest* table are `h(n)` estimates **from each listed city to Bucharest**, not from the starting city.

If the destination changes, the old heuristic table may no longer be valid.

## 3. Solved Example: Sibiu → Bucharest

**START = Sibiu** | **GOAL = Bucharest**

Route through Pitesti:

```text
Sibiu --80--> Rimnicu Vilcea --97--> Pitesti --101--> Bucharest
```

The heuristic table gives:

```text
h(Sibiu)          = 253
h(Rimnicu Vilcea) = 193
h(Pitesti)        =  98
h(Bucharest)      =   0
```

| City | `g(n)` calculation | `h(n)` | `f(n) = g + h` |
|---|---:|---:|---:|
| Sibiu | `0` | 253 | **253** |
| Rimnicu Vilcea | `80` | 193 | **273** |
| Pitesti | `80 + 97 = 177` | 98 | **275** |
| Bucharest (via Pitesti) | `80 + 97 + 101 = 278` | 0 | **278** |

### Typical exam questions

**Q: What is `f(Pitesti)`?**

```text
g(Pitesti) = 80 + 97 = 177
h(Pitesti) = 98
f(Pitesti) = 177 + 98 = 275
```

**Answer: `275`.**

**Q: What is `g(Bucharest)` along this route?**

```text
g(Bucharest) = 80 + 97 + 101 = 278
```

**Answer: `278`.** Since Bucharest is the goal, `h(Bucharest) = 0`, so `f(Bucharest) = 278` too.

**Exam trap:** Pitesti → Bucharest costs **101 on the road**, but `h(Pitesti)` is **98 in the heuristic table**. Use **98 for `h`**, and **101 for actual `g` calculations**.

## 4. Which Node Does A* Expand Next?

From Sibiu, compare `f` for its immediate neighbours:

| Candidate | `g(n)` | `h(n)` | `f(n)` |
|---|---:|---:|---:|
| Arad | 140 | 366 | **506** |
| Rimnicu Vilcea | 80 | 193 | **273** |
| Fagaras | 99 | 176 | **275** |
| Oradea | 151 | 380 | **531** |

**Next expanded: Rimnicu Vilcea**, because **273 is the smallest `f`**.

Then keep comparing **all discovered but unexplored frontier nodes**, not just the children of whichever node you most recently expanded.

> **Tie rule:** If two frontier nodes have the same smallest `f` (e.g., Pitesti and Fagaras both have `275`), the expansion order depends on the tie-breaking rule or the implementation. Don't claim one must come first unless the exam provides a tie rule.

**One optimal route in this example:**

`Sibiu → Rimnicu Vilcea → Pitesti → Bucharest`, with actual total cost **278**.

**Difference to remember:** The **order of nodes expanded** is *not necessarily* the same as the **final path** returned.

## 5. Facts Examiners Like to Test

| Fact | Correct answer |
|---|---|
| `g(start)` | **0** (no distance travelled yet) |
| `h(goal)` | **0** (no remaining cost) |
| `f(goal)` | **`g(goal)`**, because `h(goal) = 0` |
| What is the heuristic? | An **estimate** of remaining cost to goal |
| Which node does A* expand? | The **frontier node with the lowest `f(n)`** |
| Does A* use actual + estimated cost? | **Yes**: `g + h` |
| Is `h(n)` the road distance shown on the edge? | **Not necessarily**; use the specified heuristic table |
| Does A* always follow the smallest road edge? | **No**; it compares `f(n)` |
| Can A* compare nodes from different branches? | **Yes**; they can all be on the frontier |
| Is A* automatically guaranteed to be optimal? | **Only when the required assumptions hold** |

## 6. Important Theory Terms

- **Informed search:** Uses extra knowledge (a heuristic) to guide which node to explore.
- **Heuristic `h(n)`:** Estimated remaining cost to the goal.
- **Admissible heuristic:** **Never overestimates** the true cheapest remaining cost. In symbols: `h(n) ≤ h*(n)`, where `h*(n)` is the real cheapest cost from `n` to the goal.
- **Consistent heuristic (monotone):** For every edge from `n` to `m`, `h(n) ≤ c(n,m) + h(m)`, where `c(n,m)` is the edge cost. Consistency implies admissibility when `h(goal)=0` and the usual assumptions apply.
- **Optimal:** Finds a **lowest-total-cost solution**, not merely the path with the fewest nodes.
- **Complete:** Finds a solution if one exists, under appropriate conditions (e.g., finite branching and step costs bounded below by a positive amount).
- **Frontier / open list:** Nodes discovered but not yet expanded; A* chooses the one with lowest `f`.
- **Explored / closed list:** Nodes already expanded; implementation details matter if a better route to a node is later found.

### Is A* optimal?

- **A* tree search:** Optimal with an **admissible** heuristic under standard assumptions.
- **A* graph search without reopening closed nodes:** A **consistent** heuristic guarantees optimality under standard assumptions.
- **A* graph search that properly reopens nodes when a better path is found:** Can retain optimality with an admissible heuristic under standard assumptions.
- A* may use substantial **time and memory**, especially for large search spaces.

### Time and space complexity (common introductory exam answer)

In terms of branching factor **`b`** and solution depth **`d`**, A* may require **exponential time and space** in the worst case. You may see this roughly written as **`O(b^d)`** in introductory comparisons, but the exact bound depends on the heuristic, edge costs, and what depth/cost parameter is assumed. Its performance is strongly influenced by heuristic quality.

## 7. A* vs Other Search Algorithms

| Algorithm | Chooses next node using | Quick distinction |
|---|---|---|
| **BFS** | Shallowest node first | Level by level; shortest path in **number of edges** when edge costs are equal |
| **DFS** | Deepest available branch | Go deep first; not generally optimal |
| **Uniform-Cost Search (UCS)** | Smallest `g(n)` | Actual cost so far only |
| **Greedy Best-First Search** | Smallest `h(n)` | Estimated distance remaining only |
| **A\*** | Smallest **`g(n) + h(n)`** | Balances cost already paid and estimate remaining |

**Special case:** If `h(n) = 0` for all nodes, A* behaves like **Uniform-Cost Search** (assuming matching tie-breaking and search rules).

## 8. Common Mistakes to Avoid

- ❌ Calculating `g(Pitesti)` from **Arad** when the question says the start is **Sibiu**.
- ❌ Treating a road edge cost (101) as a heuristic (`h(Pitesti) = 98`).
- ❌ Assuming `h(n)` is actual distance instead of an estimate.
- ❌ Using the smallest `h(n)` to select a node in A*. That is **Greedy Best-First Search**, not A*.
- ❌ Comparing only children of the current node. Compare **all nodes on the frontier**.
- ❌ Assuming the first route discovered is automatically the best route.
- ❌ Mixing up the **node expansion order** with the **final solution path**.
- ❌ Assuming an arbitrary heuristic always guarantees an optimal A* solution.

## 9. 30-Second Checklist Before Answering

- [ ] Did I identify **START** and **GOAL** correctly?
- [ ] Did I calculate `g` **from START**, using **actual edge costs**?
- [ ] Did I read `h` **from the heuristic table**, towards **GOAL**?
- [ ] Did I calculate **`f = g + h`** correctly?
- [ ] If choosing the next node, did I pick the **lowest `f` across the frontier**?
- [ ] If there's a tie, did I check whether a **tie-breaking rule** is given?
- [ ] Did I distinguish **estimated total cost** from **actual final path cost**?

---

### Final memory line

**`g` = distance already travelled from start. `h` = estimated distance still left to goal. `f` = `g + h`. Expand the frontier node with the lowest `f`.**
