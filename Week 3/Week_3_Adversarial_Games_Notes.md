# Applied AI – Week 3: Adversarial Games

**Lecture:** Lecture 3 – Adversarial Games  
**Purpose:** Notebook-ready exam revision notes

---

## Section 1: Introduction to Adversarial Search

### 1.1 What is Adversarial Search?

**Adversarial Search** is a search method used in games where players compete against each other.

- Each player tries to achieve the best possible outcome.
- The opponent's moves affect our decisions.
- We must consider what the opponent might do next.

**Simple meaning:** We cannot choose a move by thinking only about ourselves. We must also think about the opponent's possible replies.

### 1.2 Games vs. Normal Search Problems

**Normal Search:**
- Find a sequence of actions to reach a goal.
- The environment does not deliberately try to defeat us.

**Adversarial Search:**
- The opponent may choose different moves.
- The solution is a **strategy**, not just one fixed sequence of actions.

**Strategy:** A plan that specifies what move to make for every possible opponent reply.

**★ Exam:** In adversarial games, the solution is a **strategy**, because the opponent's actions are unpredictable.

**Time limits:** We often cannot search the entire game tree. Therefore, we must **approximate** (estimate) the best move.

---

## Section 2: Types of Games

Games can be classified in different ways.

### 2.1 Deterministic vs. Stochastic Games

**Deterministic Games**
- No randomness or chance.
- The same move from the same state always gives the same result.
- Examples: Chess, Tic-Tac-Toe.

**Stochastic Games (Chance Games)**
- Include randomness, such as dice or shuffled cards.
- The outcome is not fully controlled by the players.
- Example: Backgammon.

**★ Remember:** Deterministic = No chance. Stochastic = Chance involved.

### 2.2 Number of Players

- **One-player:** One player makes decisions.
- **Two-player:** Two players compete or interact.
- **Multiplayer:** More than two players.

### 2.3 Zero-Sum vs. Cooperative Games

**Zero-Sum Game:**
- One player's gain is the other player's loss.
- The players have opposing goals.

Example: If MAX receives a utility of +5, MIN receives −5.

**Cooperative Game:**
- Players can work together to achieve a shared benefit.

### 2.4 Perfect vs. Imperfect Information

**Perfect Information:**
- Players can see the entire current game state.
- Nothing about the current position is hidden.
- Example: Chess.

**Imperfect Information:**
- Some information is hidden from a player.
- Example: Poker, where opponents' cards are hidden.

**★ Exam:** Perfect information is about what players **can see or know**, not whether the game contains randomness.

### 2.5 Game Classification

| | Deterministic | Chance |
|---|---|---|
| **Perfect Information** | Chess, Checkers, Go, Othello | Backgammon, Monopoly |
| **Imperfect Information** | Battleships, Blind Tic-Tac-Toe | Bridge, Poker, Scrabble |

**★ Remember:** A game can have perfect information and still contain chance.

---

## Section 3: Two-Player Zero-Sum Games

These are the main games studied when introducing **Minimax**.

### 3.1 Main Characteristics

- **Two players:** MAX and MIN.
- **Deterministic:** No randomness.
- **Turn-taking:** Players move one after another.
- **Perfect information:** Both players know the current game state.
- **Zero-sum:** What benefits one player harms the other.

Examples: Tic-Tac-Toe, Chess and Go.

### 3.2 MAX and MIN

**MAX (Maximizing Player):**
- Tries to get the **highest utility value**.
- Chooses the maximum available value.

**MIN (Minimizing Player):**
- Tries to make MAX's utility as low as possible.
- Chooses the minimum available value.

**Utility:** A numerical score showing how good a game outcome is.

For example, from MAX's perspective:

| Outcome | Utility |
|---|---:|
| MAX wins | +1 |
| Draw | 0 |
| MAX loses | −1 |

**★ Exam:** In a zero-sum game, the utility values of the two players are opposites.

Example: MAX utility = −8 means MIN utility = +8.

### 3.3 Game Tree

A **Game Tree** shows the possible moves and resulting game states.

- **Root Node:** The starting state of the tree.
- **Branch:** A possible move.
- **Child Node:** The state reached after a move.
- **Terminal State:** A state where the game ends.
- **Utility Value:** The final score assigned to a terminal state.

Small example:

```text
                 MAX
              /       \
            MIN       MIN
           /   \     /   \
          3     5   2     8
```

MIN chooses 3 on the left and 2 on the right. MAX then chooses 3.

**★ Important:** The root is not always MAX in every possible game tree. It depends on whose turn it is. In the lecture's main Minimax examples, the root is MAX.

---

## Section 4: Minimax Algorithm

### 4.1 What is Minimax?

**Minimax** is an algorithm used to choose the best move in a two-player, deterministic, perfect-information game.

It assumes:
- MAX tries to maximize utility.
- MIN tries to minimize MAX's utility.
- Both players make the best possible decisions.

**Simple meaning:** MAX chooses a move while assuming MIN will make the strongest possible reply.

**★ Exam:** Minimax gives the best achievable payoff against an opponent who also plays optimally.

### 4.2 Components of a Game

| Component | Simple explanation |
|---|---|
| **S₀** | Initial state, where the game starts |
| **PLAYER(s)** | Which player moves in state `s` |
| **ACTIONS(s)** | All legal moves available in state `s` |
| **RESULT(s, a)** | New state after taking action `a` in state `s` |
| **TERMINAL-TEST(s)** | Checks whether the game has ended |
| **UTILITY(s, p)** | Final utility of state `s` for player `p` |

Here, `s` = game state, `a` = action or move, and `p` = player.

### 4.3 How Minimax Works

1. Build the game tree by considering possible moves.
2. Continue until terminal states are reached.
3. Assign utility values to terminal states.
4. Work **from the bottom to the top**.
5. At a MIN node, choose the **smallest** child value.
6. At a MAX node, choose the **largest** child value.
7. Select the root move with the best Minimax value.

### 4.4 Worked Example (Lecture Example)

```text
                       MAX = 3
                    /     |     \
                A1 /   A2 |  A3  \
                  /       |       \
              MIN=3    MIN=2    MIN=2
             / | \     / | \     / | \
            3 12  8   2  4  6  14 5  2
```

**Step 1: MIN chooses the smallest value.**
- Left: min(3, 12, 8) = **3**
- Middle: min(2, 4, 6) = **2**
- Right: min(14, 5, 2) = **2**

**Step 2: MAX chooses the largest value.**

$$
\max(3,2,2)=\boxed{3}
$$

**Answer:** MAX selects **A1**, with utility 3.

**★ Remember:** Minimax calculations move **bottom to top**, but actual gameplay moves from the root downward.

### 4.5 Minimax Formula

$$
\operatorname{Minimax}(s)=
\begin{cases}
\operatorname{Utility}(s), & \text{if terminal}\\
\max\limits_a\operatorname{Minimax}(\operatorname{Result}(s,a)), & \text{if MAX}\\
\min\limits_a\operatorname{Minimax}(\operatorname{Result}(s,a)), & \text{if MIN}
\end{cases}
$$

Where:
- `s` = current game state.
- `a` = a legal action.
- `Utility(s)` = final score of a terminal state.
- `max` = choose the highest child value.
- `min` = choose the lowest child value.
- `Result(s,a)` = state after taking action `a`.

**Simple meaning:** Stop and return the score if the game is finished. Otherwise, calculate the best child value according to whose turn it is.

### 4.6 Minimax Algorithm: Main Steps

**MAX-VALUE(state)**
1. If the state is terminal, return its utility.
2. Start with $v=-\infty$ (negative infinity).
3. Calculate the value of every child using MIN-VALUE.
4. Update $v$ with the highest value found.
5. Return $v$.

**MIN-VALUE(state)**
1. If the state is terminal, return its utility.
2. Start with $v=+\infty$ (positive infinity).
3. Calculate the value of every child using MAX-VALUE.
4. Update $v$ with the lowest value found.
5. Return $v$.

**Infinity:** Used as an initial value so that the first real value can replace it.

**★ Remember:** MAX-VALUE calls MIN-VALUE, and MIN-VALUE calls MAX-VALUE because players take turns.

### 4.7 Properties of Minimax

**Completeness: YES, if the game tree is finite.**

Complete means the algorithm is guaranteed to find a solution when the finite tree can be fully searched.

**Optimality: YES, against an optimal opponent.**

Optimal means it chooses the best move under the assumption that the opponent also plays perfectly.

Against a weaker opponent, Minimax may achieve a better result, but it does not specifically try to exploit mistakes.

**Time Complexity:**

$$
\boxed{O(b^m)}
$$

**Space Complexity:**

$$
\boxed{O(bm)}
$$

Where:
- $b$ = **branching factor**, the average number of possible moves from a state.
- $m$ = **maximum depth**, the maximum number of moves from the root to a terminal state in the search tree.
- $O$ = Big-O notation, describing how the required time or memory grows.

**Simple meaning:**
- Time grows extremely fast when the tree becomes deeper.
- Depth-first exploration allows Minimax to use much less memory than storing the whole tree.

For chess:
- $b \approx 35$
- $m \approx 100$

Searching the full game tree is practically impossible.

**★ Exam:** Minimax is complete for finite trees and optimal against an optimal opponent, but its main weakness is its extremely high **time complexity**.

---

## Section 5: Alpha-Beta Pruning (α–β Pruning)

### 5.1 What is Alpha-Beta Pruning?

**Alpha-Beta Pruning** is an improvement to Minimax that avoids searching branches that cannot affect the final decision.

**Pruning:** Skipping part of the game tree because we already know it cannot change the result.

**Simple meaning:** Do not waste time checking moves that cannot change which move MAX will choose.

### 5.2 Why Do We Need It?

Normal Minimax:
- Searches every relevant path to the full search depth.
- Becomes very slow for large game trees.

Alpha-Beta Pruning:
- Skips unnecessary leaves and entire subtrees.
- Allows deeper searches with the available time.
- Produces the **same final Minimax result** when searching the same complete tree.

**★ Exam:** Alpha-Beta Pruning changes the amount of searching, **not the final Minimax value**.

### 5.3 Alpha (α) and Beta (β)

**Alpha (α):**
- The best value found so far for MAX along the current search path.
- Starts at $-\infty$.
- Can only increase.

**Beta (β):**
- The best value found so far for MIN along the current search path.
- Starts at $+\infty$.
- Can only decrease.

**★ Remember:**
- α = MAX's best value so far.
- β = MIN's best value so far.

### 5.4 Pruning Condition

$$
\boxed{\alpha\geq\beta}
$$

When this condition is reached, the remaining branches at that point can be skipped.

More specifically:
- **At a MAX node:** Stop searching remaining children if $v\geq\beta$.
- **At a MIN node:** Stop searching remaining children if $v\leq\alpha$.

Here, $v$ is the best value found so far at the current node.

### 5.5 Alpha-Beta Pruning Example

Use the same game tree from Section 4.

**Step 1: Examine the left subtree.**

MIN chooses:

$$
\min(3,12,8)=3
$$

MAX now has $\alpha=3$.

**Step 2: Examine the middle subtree.**

Its first child is 2. MIN's value is now **at most 2**, because MIN always chooses the smallest value.

But MAX already has an alternative worth 3. Therefore, MAX will never choose this middle branch.

**Prune the remaining values 4 and 6.**

**Step 3: Examine the right subtree.**

$$
\min(14,5,2)=2
$$

**Final decision:**

$$
\max(3,\leq2,2)=\boxed{3}
$$

MAX still chooses **A1**.

**★ Exam:** Pruning the values 4 and 6 does not affect the result because the middle MIN node already has a value of at most 2.

### 5.6 Alpha-Beta Algorithm: Key Steps

1. Start with $\alpha=-\infty$ and $\beta=+\infty$.
2. Search the game tree using Minimax.
3. At MAX nodes, update α when a better value is found.
4. At MIN nodes, update β when a lower value is found.
5. When $\alpha\geq\beta$, prune the remaining branches at that point.
6. Return the chosen Minimax value.

**★ Remember:** Alpha-Beta is **Minimax + Pruning**.

### 5.7 Properties of Alpha-Beta Pruning

**Correctness:** Same final result as Minimax.

**Move Ordering:** The order in which moves are examined affects how much pruning happens.
- Good moves examined first → more pruning.
- Poor move ordering → less pruning.

**Time Complexity with Perfect Ordering:**

$$
\boxed{O(b^{m/2})}
$$

Where:
- $b$ = branching factor.
- $m$ = maximum depth.

**Perfect ordering:** The algorithm examines the most useful moves first, allowing the maximum benefit from pruning.

Compared with Minimax $O(b^m)$, this can approximately **double the searchable depth** for the same search effort.

**Metareasoning:** Reasoning about which calculations are worth doing.

**Simple meaning:** The algorithm saves time by deciding which branches do not need further investigation.

**★ Exam:** Alpha-Beta is more efficient than Minimax when pruning is effective, but it cannot make extremely large game trees fully searchable.

---

## Section 6: Resource Limits – Limiting Depth

### 6.1 Why Limit Search Depth?

Large games have too many possible moves. Even Alpha-Beta Pruning may not reach the end of the game within the available time.

**Solution:** Stop searching after a chosen depth and estimate how good the position is.

### 6.2 Depth-Limited Search

Two main changes are made to Minimax:

**1. Use CUTOFF-TEST instead of TERMINAL-TEST.**
- `TERMINAL-TEST` checks whether the game has actually finished.
- `CUTOFF-TEST` checks whether we should stop searching, for example, because the depth limit has been reached.

**2. Use EVAL instead of UTILITY at cutoff states.**
- `UTILITY` gives the final value of a finished game.
- `EVAL` estimates the value of a position before the game finishes.

**Simple meaning:** Stop searching at a reasonable depth and estimate the value of the unfinished position.

### 6.3 Example from the Lecture

Suppose:
- Available time = 100 seconds.
- Search speed = $10^4$ nodes/second.

Total nodes that can be explored:

$$
100\times10^4=10^6
$$

That is **1,000,000 nodes per move**.

With Alpha-Beta Pruning and chess branching factor $b\approx35$:

$$
35^{8/2}\approx1.5\times10^6
$$

This gives roughly **depth 8** as a practical estimate.

### 6.4 Advantages and Limitations

**Advantages:**
- Search finishes within available resources.
- Makes large games practical.
- Allows the algorithm to choose a move without reaching the end of the game.

**Limitations:**
- May miss important moves beyond the depth limit.
- Depends on the accuracy of the evaluation function.
- Does not guarantee the same perfect-play result as full Minimax.

**★ Exam:** Depth-limited search uses **CUTOFF-TEST + EVAL**.

---

## Section 7: Evaluation Functions

### 7.1 What is an Evaluation Function?

An **Evaluation Function (Eval)** estimates how good a non-terminal game state is.

**Non-terminal state:** A state where the game has not ended.

**Simple meaning:** It gives an estimated score without playing the game until the end.

### 7.2 Evaluation Function Formula

The lecture uses a **Linear Weighted Sum of Features**:

$$
\boxed{Eval(s)=w_1f_1(s)+w_2f_2(s)+\cdots+w_nf_n(s)}
$$

Where:
- $s$ = current game state.
- $f_i(s)$ = a feature measured from state $s$.
- $w_i$ = weight showing the importance of that feature.
- $n$ = number of features used.
- $Eval(s)$ = estimated value of the position.

**Feature:** A measurable part of the game state, such as the number of queens.

**Weight:** A number showing how important a feature is.

**Linear weighted sum:** Multiply each feature by its weight, then add the results.

### 7.3 Simple Chess Example

The lecture gives:

$$
f_1(s)=\text{White Queens}-\text{Black Queens}
$$

With $w_1=9$.

Suppose White has 1 queen and Black has 0 queens:

$$
f_1(s)=1-0=1
$$

Weighted contribution:

$$
9\times1=\boxed{+9}
$$

If White has 0 queens and Black has 1 queen:

$$
9\times(0-1)=\boxed{-9}
$$

Here, positive values favour White, and negative values favour Black for this feature.

This is only **one feature's contribution**, not the full chess evaluation.

**★ Exam:** Evaluation functions provide **estimated values**, while Utility functions provide **final game outcomes**.

---

## Section 8: Deterministic Games in Practice

The lecture gives examples of AI systems that became very strong at deterministic games.

### 8.1 Important Examples

**Chess – Deep Blue**
- Defeated world chess champion Garry Kasparov in 1997.
- Searched approximately 200 million positions per second.
- Used sophisticated evaluation functions and deeper search for selected moves.

**Checkers – Chinook**
- Ended Marion Tinsley's long reign as world champion in 1994.
- Used an **endgame database** containing perfect-play information for positions with 8 or fewer pieces.

**Othello**
- Computer players became extremely strong compared with human champions.

**Go**
- Has a very large branching factor, $b>300$.
- Earlier programs used **pattern knowledge bases** to suggest promising moves.
- Deep learning later greatly improved computer performance.

**★ Remember:** Deep Blue defeated Kasparov in **1997**. Large branching factors make exhaustive search difficult.

---

## Section 9: Nondeterministic Games (Games of Chance)

### 9.1 What are Nondeterministic Games?

**Nondeterministic Games** contain random events that influence the game.

Examples:
- Dice rolling in Backgammon.
- Card shuffling in card games.

**Simple meaning:** The result depends on both player decisions and chance.

### 9.2 Chance Nodes

A **Chance Node** represents a random event.

Unlike MAX and MIN:
- MAX chooses the highest value.
- MIN chooses the lowest value.
- CHANCE calculates the expected value using probabilities.

**Expected Value:** The average outcome when different results have different probabilities.

### 9.3 Expectiminimax Algorithm

**Expectiminimax** extends Minimax to handle games containing chance.

It uses three types of nodes:
1. **MAX Node:** Choose the highest value.
2. **MIN Node:** Choose the lowest value.
3. **CHANCE Node:** Calculate the expected value.

**★ Exam:** Expectiminimax = Minimax + Chance Nodes.

### 9.4 Chance Node Formula

$$
\boxed{V=\sum_{i=1}^{n}P_iV_i}
$$

Where:
- $V$ = expected value of the chance node.
- $P_i$ = probability of outcome $i$.
- $V_i$ = value of outcome $i$.
- $n$ = number of possible outcomes.

**Simple meaning:** Multiply each possible result by its probability, then add them.

### 9.5 Worked Example (Lecture Example)

```text
                      MAX = 3
                    /         \
              CHANCE=3      CHANCE=−1
               /    \         /    \
             0.5    0.5     0.5    0.5
              |      |       |      |
            MIN=2  MIN=4   MIN=0  MIN=−2
            /  \    /  \    /  \    /  \
           2    4  7    4  6    0  5   −2
```

**Step 1: Calculate MIN values.**
- $\min(2,4)=2$
- $\min(7,4)=4$
- $\min(6,0)=0$
- $\min(5,-2)=-2$

**Step 2: Calculate CHANCE values.**

Left chance node:

$$
(0.5\times2)+(0.5\times4)=3
$$

Right chance node:

$$
(0.5\times0)+(0.5\times-2)=-1
$$

**Step 3: MAX chooses the highest value.**

$$
\max(3,-1)=\boxed{3}
$$

**Final Answer:** MAX chooses the **left branch**.

**★ Remember:** At a chance node, do not choose the highest or lowest outcome. Calculate the **expected value**.

When the probabilities are equal, the expected value is the ordinary average.

---

## Section 10: Games of Imperfect Information

### 10.1 What is Imperfect Information?

In **Imperfect Information Games**, players do not know the full game state.

Example: Poker, where opponents' cards are hidden.

**Simple meaning:** The player must make decisions without knowing everything.

### 10.2 Handling Hidden Information

The lecture presents this general approach:

1. Identify possible hidden states (for example, possible card deals).
2. Assign a probability to each possibility.
3. Calculate the value of each possible action.
4. Calculate the expected value across those possibilities.
5. Choose the action with the highest expected value.

Because information is missing, game-specific **heuristics** and other techniques are needed.

**Heuristic:** A useful rule or estimate that helps make decisions without knowing or calculating everything exactly.

### 10.3 Bridge Example

Bridge programs approximate this process by:
1. Generating 100 possible card deals that match the bidding information.
2. Estimating how many tricks each action wins.
3. Choosing the action with the highest average number of tricks.

**★ Exam:** In imperfect-information games, decisions depend on the **information available to the player**, not on hidden information the player cannot see.

**Important difference:**
- **Chance:** The result depends on a random event.
- **Imperfect Information:** The player does not know the complete state.

These are different problems, although a game can contain both.

---

## Section 11: AI Game Systems and Key Lessons

### 11.1 Successful AI Game Systems

| AI System | Game | Main achievement or method |
|---|---|---|
| **QUACKLE** | Scrabble | Defeated a former world champion |
| **Libratus** | Poker | Defeated four top players in no-limit Texas Hold'em |
| **Pluribus** | Poker | Defeated five simultaneous opponents |
| **TD-Gammon** | Backgammon | World-class performance using depth-2 search and strong evaluation |
| **AlphaStar** | StarCraft II | Used deep learning and reinforcement learning to defeat expert gamers |

### 11.2 Main Lessons About AI

**1. Perfect search is often impossible.**  
Large game trees require approximation.

**2. Not every branch needs to be searched.**  
Alpha-Beta Pruning avoids unnecessary calculations.

**3. Uncertainty makes decisions harder.**  
Chance and hidden information make it difficult to assign exact values to states.

**4. Available information matters.**  
An optimal decision must use what the player actually knows.

---

## Section 12: Final Algorithm Comparison

| Algorithm | How it decides | Main limitation |
|---|---|---|
| **Minimax** | MAX chooses highest, MIN chooses lowest | Searches too many nodes |
| **Alpha-Beta Pruning** | Minimax, but skips irrelevant branches | Efficiency depends on move ordering |
| **Depth-Limited Minimax** | Stops early and uses Eval | Estimates may be inaccurate |
| **Expectiminimax** | MAX, MIN and probability-weighted CHANCE values | Chance creates additional possible outcomes |

**Why they are different:** Minimax solves the basic adversarial decision problem. Alpha-Beta makes Minimax faster. Depth limiting makes searching practical when resources are restricted. Expectiminimax allows the search to include randomness.

### ★ Final Exam Memory Tricks

- **MAX = Maximum**
- **MIN = Minimum**
- **Minimax = Bottom to Top**
- **Alpha-Beta = Skip unnecessary branches**
- **α = MAX's best so far**
- **β = MIN's best so far**
- **Prune when α ≥ β**
- **Depth Limit = CUTOFF-TEST + EVAL**
- **Expectiminimax = MAX + MIN + CHANCE**
- **Chance Node = Expected Value**

---

## Notes About Possible Slide Errors

1. **Slides 14 and 22:** The pseudocode uses `Result(a, state)`, while the earlier definition is `RESULT(s, a)`. The consistent notation is `RESULT(state, action)`.
2. **Slide 22:** The comment beside the MAX-node pruning condition refers to MIN not choosing a value greater than *alpha*. The condition actually checks against **beta** ($v\geq\beta$). The comment appears incorrect.
3. **Slides 16 and 24:** Slide 16 describes approximately halving the search exponent as an average improvement. Slide 24 attaches the $O(b^{m/2})$ result to **perfect ordering**. Do not assume this best-case improvement is guaranteed for every ordering.
4. **Slide 21:** Some values overlap in the final Alpha-Beta animation. The correct root Minimax result is **3**.

*Notes compiled from Applied AI, Lecture 3 – Adversarial Games. Repeated animations, polls, and non-essential slide filler have been omitted.*
