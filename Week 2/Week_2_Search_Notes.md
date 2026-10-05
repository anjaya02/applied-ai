# Applied AI - Lecture 2: Search
## Simple Notebook Notes

---

## 1. What is Search?

In AI, **search** means trying different possible states until we find the goal.

The AI starts from an **initial state** and explores possible next states.

Example:

If we want to travel from **Arad to Bucharest**, the AI can explore different cities and roads until it finds a route to Bucharest.

### Basic search process

1. Start from the initial state.
2. Choose one node to explore.
3. Generate its possible next states.
4. Check whether one of them is the goal.
5. Continue until the goal is found.

### Important word: Expand

**Expand a node** = find the possible next states from that node.

Example:

```text
      A
     / \
    B   C
```

Expanding **A** creates **B and C**.

**★ Exam:** Different search algorithms mainly differ in **which node they choose to expand next**. Applied AI - Lecture 2 Search

---

# 2. State vs Node

These two words can be confusing.

### State

A **state** describes the current situation of the problem.

Example:

In a route problem:

```text
Current city = Sibiu
```

Sibiu is a state.

### Node

A **node** stores a state plus extra information needed by the search algorithm.

A node can store:

- State
- Parent
- Children
- Action
- Depth
- Path cost \(g(n)\)

### Simple difference

**State = situation**

**Node = state + search information**

**★ Exam:** A state itself does not have a parent, depth or path cost. A node stores these things. Applied AI - Lecture 2 Search

---

# 3. Depth-First Search (DFS)

DFS searches **deep into one branch first**.

It keeps going down until it cannot continue, then comes back and tries another branch.

Example:

```text
       A
      / \
     B   C
    / \
   D   E
```

DFS may search:

```text
A → B → D → E → C
```

It goes deeper before moving sideways.

### Data structure used

DFS uses:

\[
\boxed{LIFO}
\]

**LIFO = Last In, First Out**

Think of a stack of plates.

The last plate placed on top is the first one removed.

### Easy memory trick

> **DFS = Deep First**

**★ Exam**

DFS expands the **deepest unexpanded node** first. Applied AI - Lecture 2 Search

---

# 4. Breadth-First Search (BFS)

BFS searches **level by level**.

It first checks all nodes near the start before going deeper.

Example:

```text
       A
      / \
     B   C
    / \ / \
   D  E F  G
```

BFS order:

```text
A → B → C → D → E → F → G
```

### Data structure used

BFS uses:

\[
\boxed{FIFO}
\]

**FIFO = First In, First Out**

Think of a queue.

The person who comes first is served first.

### Easy memory trick

> **BFS = Broad / level-by-level**

**★ Exam**

BFS expands the **shallowest unexpanded node** first.

Shallowest simply means **closest to the starting node**. Applied AI - Lecture 2 Search

---

# 5. How Do We Judge a Search Algorithm?

We mainly check four things.

### 1. Completeness

Question:

> If a solution exists, will the algorithm definitely find it?

If yes → **Complete**

---

### 2. Time Complexity

Question:

> How much work does the algorithm need to do?

Usually measured by the number of nodes generated or expanded.

---

### 3. Space Complexity

Question:

> How much memory does the algorithm need?

---

### 4. Optimality

Question:

> Does the algorithm find the best / lowest-cost solution?

If yes → **Optimal**

---

## Important symbols

### \(b\) = branching factor

Maximum number of children a node can have.

Example:

```text
      A
   /  |  \
  B   C   D
```

Here:

\[
b=3
\]

---

### \(d\) = depth of the shallowest solution

How many levels down the closest solution is.

---

### \(m\) = maximum depth

The deepest the search space can go.

It may sometimes be infinite.

---

### \(l\) = depth limit

Maximum depth that the algorithm is allowed to search.

---

### \(C^*\)

Cost of the **best possible solution path**.

**★ Exam:** Know what **complete, optimal, time and space** mean. Applied AI - Lecture 2 Search

---

# 6. Uninformed Search

**Uninformed Search** means the algorithm does not have extra information telling it where the goal is.

It only knows the basic problem information.

Examples:

- Breadth-First Search
- Uniform-Cost Search
- Depth-First Search
- Depth-Limited Search
- Iterative Deepening Search

Simple idea:

> Uninformed search is searching **without a clue about which direction is better**. Applied AI - Lecture 2 Search

---

# 7. Big-O Notation

Big-O tells us how the amount of work grows when the problem becomes bigger.

Common examples:

```text
O(1)
O(log n)
O(n)
O(n log n)
O(n²)
O(2ⁿ)
O(n!)
```

The lower ones are normally better.

For example:

\[
O(n)
\]

usually grows much more slowly than:

\[
O(2^n)
\]

Search algorithms often have complexities such as:

\[
O(b^d)
\]

or

\[
O(b^m)
\]

These can grow extremely quickly.

### Simple idea

If:

\[
b=10
\]

and:

\[
d=5
\]

then roughly:

\[
10^5=100,000
\]

possible nodes may be involved.

So even a small increase in depth can make search much harder.

---

# 8. Breadth-First Search Properties

### Complete?

**Yes**, if \(b\) is finite.

Meaning:

If a solution exists, BFS will eventually find it.

---

### Time

\[
O(b^{d+1})
\]

This can become very large.

---

### Space

\[
O(b^{d+1})
\]

BFS keeps many nodes in memory.

Therefore:

> **The biggest problem with BFS is memory usage.**

---

### Optimal?

**Yes, if every step has the same cost.**

For example, if every move costs 1.

### ★ Remember

BFS:

```text
Complete = Yes
Optimal = Yes, if equal step cost
Main problem = Memory
```

Applied AI - Lecture 2 Search

---

# 9. Depth-First Search Properties

### Complete?

Usually **No**.

Why?

DFS could keep following an infinitely deep branch.

Example:

```text
A → B → C → D → E → ...
```

It may never return to search another branch containing the solution.

Loops can also cause problems.

---

### Time

\[
O(b^m)
\]

If the search space is very deep, DFS can take a long time.

---

### Space

\[
O(bm)
\]

This is much better than BFS.

DFS does not need to keep every node in memory.

---

### Optimal?

**No.**

DFS may find a long/expensive solution before finding a better one.

### ★ Remember

```text
DFS
Complete = No
Optimal = No
Good thing = uses less memory
```

Applied AI - Lecture 2 Search

---

# 10. How Can We Improve Search?

The lecture gives three ideas:

- Limit the depth.
- Use Iterative Deepening.
- Use heuristics.

---

## Heuristic

A **heuristic** is a useful estimate or clue that helps the algorithm decide which direction looks better.

Simple meaning:

> **Heuristic = educated guess**

It does not have to know the exact answer.

It gives the algorithm a clue about where the goal may be.

Example:

If you are travelling to Bucharest, the **straight-line distance to Bucharest** can be used as a clue.

Applied AI - Lecture 2 Search

---

# 11. Depth-Limited Search (DLS)

Depth-Limited Search is basically:

\[
\boxed{\text{DFS + depth limit}}
\]

We choose a maximum depth:

\[
l
\]

Example:

If:

\[
l=2
\]

the algorithm can search:

```text
Depth 0
Depth 1
Depth 2
```

but it cannot go to depth 3.

---

## Possible results

DLS can return three things.

### Solution

The goal was found.

### Failure

No solution was found in the searched space.

### Cutoff

The algorithm reached the depth limit.

This means:

> "I had to stop here. There may still be a solution deeper down."

### ★ Important difference

**Failure**

= no solution found.

**Cutoff**

= stopped because of depth limit.

There may still be a solution below the limit. Applied AI - Lecture 2 Search

---

# 12. Iterative Deepening Search (IDS)

Iterative Deepening repeatedly performs Depth-Limited Search.

First:

\[
l=0
\]

Then:

\[
l=1
\]

Then:

\[
l=2
\]

Then:

\[
l=3
\]

and so on.

Example:

### Limit 0

```text
A
```

### Limit 1

```text
    A
   / \
  B   C
```

### Limit 2

```text
      A
     / \
    B   C
   / \ / \
  D  E F  G
```

It continues increasing the limit until it finds the solution.

### Why is this useful?

IDS gets advantages from both BFS and DFS.

Like BFS:

- It can find shallow solutions.

Like DFS:

- It does not use huge amounts of memory.

**★ Exam:** Iterative Deepening uses **linear space** and does not add much extra search time. Applied AI - Lecture 2 Search

---

# 13. Uninformed Search Comparison

| Algorithm | Complete? | Time | Space | Optimal? |
|---|---|---|---|---|
| BFS | Yes | \(b^{d+1}\) | \(b^{d+1}\) | Yes |
| Uniform-Cost | Yes | \(b^l\) | \(b^l\) | Yes |
| DFS | No | \(b^m\) | \(bm\) | No |
| Depth-Limited | Yes if \(l \ge d\) | \(b^l\) | \(bl\) | No |
| Iterative Deepening | Yes | \(b^d\) | \(bd\) | Yes |

This is the comparison shown in the lecture. Applied AI - Lecture 2 Search

### Easy way to remember

**BFS**
→ good solution finding  
→ bad memory

**DFS**
→ good memory  
→ may go the wrong way forever

**Iterative Deepening**
→ combines useful parts of BFS + DFS

---

# 14. Repeated States

Sometimes the search reaches the same state again.

Example:

```text
A → B → C → A → B → C ...
```

This creates a loop.

If the algorithm does not notice repeated states, it may waste a huge amount of time.

**★ Important:** Not detecting repeated states can turn a simple problem into a much larger search problem. Applied AI - Lecture 2 Search

---

# 15. Graph Search

Graph Search solves the repeated-state problem.

It keeps a set called:

\[
\boxed{CLOSED}
\]

`CLOSED` stores states that have already been explored.

### Simple process

```text
Take a node.

Is it the goal?
→ Yes: return solution.

Already in CLOSED?
→ Yes: don't explore it again.

Not in CLOSED?
→ Add it to CLOSED.
→ Expand it.
```

### Tree Search vs Graph Search

**Tree Search**

May visit the same state again and again.

**Graph Search**

Remembers visited states and avoids repeating work.

**★ Exam:** Graph Search can be **much more efficient** than Tree Search. Applied AI - Lecture 2 Search

---

# 16. Informed Search

Now we give the algorithm extra information to help it find the goal.

This information normally comes from a:

\[
\boxed{\text{heuristic}}
\]

---

# 17. Best-First Search

Best-First Search gives each node a value showing how **promising** it looks.

Simple meaning:

> Choose the node that currently looks best.

The lecture discusses two important Best-First methods:

- Greedy Search
- A* Search Applied AI - Lecture 2 Search

---

# 18. Heuristic Function

A heuristic function is written:

\[
h(n)
\]

It estimates the cost from node \(n\) to the goal.

In the Romania example:

\[
h_{SLD}(n)
\]

means:

**Straight-Line Distance from city \(n\) to Bucharest.**

Example:

```text
Sibiu → Bucharest straight-line distance = 253
```

Therefore:

\[
h(Sibiu)=253
\]

---

# 19. Greedy Best-First Search

Greedy Search uses only:

\[
\boxed{h(n)}
\]

It chooses the node with the **smallest heuristic value**.

Simple meaning:

> Choose the node that currently looks closest to the goal.

### Example

From Arad:

```text
Sibiu      h = 253
Timisoara  h = 329
Zerind     h = 374
```

Which one will Greedy choose?

\[
\boxed{Sibiu}
\]

because:

\[
253 < 329 < 374
\]

### Important

Greedy does **not** look at how much distance we already travelled.

It only looks at the estimated distance remaining.

### Easy memory trick

\[
\boxed{\text{Greedy = h only}}
\]

Applied AI - Lecture 2 Search

---

# 20. Greedy Search Properties

### Complete?

Usually **No**.

It can get stuck in a loop.

Example from lecture:

```text
Iasi → Neamt → Iasi → Neamt → ...
```

Repeated-state checking can help in finite spaces.

---

### Time

\[
O(b^m)
\]

A good heuristic can make it much faster in practice.

---

### Space

\[
O(b^m)
\]

It may need to keep many nodes.

---

### Optimal?

**No.**

Why?

The place that **looks closest** to the goal is not always on the **cheapest route**.

### ★ Remember

\[
\boxed{\text{Greedy chooses lowest }h}
\]

and:

```text
Greedy is NOT always optimal.
```

Applied AI - Lecture 2 Search

---

# 21. A* Search

A* uses two pieces of information.

### \(g(n)\)

Actual cost from the starting point to node \(n\).

Simple meaning:

> **How much have I already spent/travelled?**

---

### \(h(n)\)

Estimated cost from node \(n\) to the goal.

Simple meaning:

> **How much do I think is left?**

---

### \(f(n)\)

Estimated total cost of the complete path.

\[
\boxed{f(n)=g(n)+h(n)}
\]

Simple meaning:

\[
\text{Total estimate}
=
\text{cost already used}
+
\text{estimated cost remaining}
\]

### ★ Most important formula

\[
\boxed{f(n)=g(n)+h(n)}
\]

Applied AI - Lecture 2 Search

---

# 22. A* Example

Starting from Arad:

\[
g(Arad)=0
\]

\[
h(Arad)=366
\]

Therefore:

\[
f=0+366=366
\]

After expanding Arad:

### Sibiu

Road cost:

\[
g=140
\]

Estimated remaining distance:

\[
h=253
\]

Therefore:

\[
f=140+253=393
\]

---

### Timisoara

\[
f=118+329
\]

\[
f=447
\]

---

### Zerind

\[
f=75+374
\]

\[
f=449
\]

A* chooses **Sibiu** because:

\[
393
\]

is the lowest \(f\).

### Final good route

The example eventually finds:

```text
Arad
↓
Sibiu
↓
Rimnicu Vilcea
↓
Pitesti
↓
Bucharest
```

Cost:

\[
\boxed{418}
\]

---

# 23. Greedy vs A*

This is very important.

## Greedy

Uses:

\[
\boxed{h(n)}
\]

Question Greedy asks:

> "Which node looks closest to the goal?"

---

## A*

Uses:

\[
\boxed{g(n)+h(n)}
\]

Question A* asks:

> "Considering what I have already spent AND what is probably left, which path looks cheapest?"

### ★ Easiest way to remember

```text
Greedy = h

A* = g + h
```

---

# 24. Why Can A* Be Better Than Greedy?

Imagine two routes.

### Route A

Already travelled:

\[
g=100
\]

Remaining estimate:

\[
h=20
\]

So:

\[
f=120
\]

### Route B

Already travelled:

\[
g=20
\]

Remaining estimate:

\[
h=40
\]

So:

\[
f=60
\]

Greedy only looks at \(h\).

So Greedy chooses Route A because:

\[
20<40
\]

But A* looks at total cost.

So A* chooses Route B because:

\[
60<120
\]

This is why **Greedy can make bad choices**.

---

# 25. Admissible Heuristic

This word sounds difficult, but the idea is simple.

**Admissible heuristic** means:

> The heuristic is allowed to guess too low, but it must NEVER guess too high.

Formula:

\[
\boxed{h(n)\leq h^*(n)}
\]

where:

- \(h(n)\) = estimated cost
- \(h^*(n)\) = real/true cost to the goal

### Example

Suppose real distance to the goal is:

\[
100km
\]

These estimates are admissible:

```text
80 km ✓
90 km ✓
100 km ✓
```

But:

```text
120 km ✗
```

because it overestimates the true cost.

### Goal state

For the goal:

\[
\boxed{h(G)=0}
\]

because there is no distance left.

### ★ Very important

> An admissible heuristic **never overestimates** the real cost.

And:

> **A* using an admissible heuristic is optimal.**

Applied AI - Lecture 2 Search

---

# 26. Properties of A*

### Complete?

**Yes**, under the conditions given in the lecture.

---

### Time

Can still be expensive and may grow exponentially.

But a good heuristic can reduce the amount of searching a lot.

---

### Space

A* stores many generated nodes.

So:

> **Memory is one of the main problems of A*.**

---

### Optimal?

**Yes**, when the required heuristic condition is satisfied.

### ★ Remember

```text
A*
Complete = Yes
Optimal = Yes
Main issue = Memory
```

Applied AI - Lecture 2 Search

---

# 27. Final Informed Search Summary

## Heuristic

\[
h(n)
\]

= estimated cost to goal.

---

## Greedy

\[
\boxed{h(n)}
\]

Chooses:

> smallest \(h\)

Not always optimal.

---

## A*

\[
\boxed{g(n)+h(n)}
\]

Chooses:

> smallest \(f\)

where:

\[
f(n)=g(n)+h(n)
\]

With an admissible heuristic, A* is optimal. Applied AI - Lecture 2 Search

---

# ⭐ Must-Know Exam Points

1. **State = situation. Node = state + search information.**
2. **Expand = generate the next possible nodes.**
3. **DFS = deepest first = LIFO.**
4. **BFS = shallowest first = FIFO.**
5. **Complete = will it definitely find a solution if one exists?**
6. **Optimal = will it find the cheapest/best solution?**
7. \(b\) = branching factor.
8. \(d\) = depth of shallowest solution.
9. \(m\) = maximum depth.
10. \(l\) = depth limit.
11. **BFS main weakness = high memory use.**
12. **DFS main advantage = low memory use.**
13. **Depth-Limited Search = DFS + depth limit.**
14. **Iterative Deepening = limits 0, 1, 2, 3...**
15. **Graph Search uses CLOSED to avoid repeated states.**
16. **Heuristic = useful estimate / educated guess.**
17. **Greedy chooses lowest \(h\).**
18. **A* chooses lowest \(g+h\).**
19. \(g\) = cost already travelled.
20. \(h\) = estimated cost left.
21. \(f=g+h\) = estimated total cost.
22. **Admissible heuristic = never overestimates.**
23. \(\boxed{h(n)\leq h^*(n)}\)
24. \(\boxed{h(G)=0}\)
25. **A* + admissible heuristic = optimal.**

