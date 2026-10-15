# Applied AI: Agents and Environments
**Lecture 4 | Week 4 Notes | PDF: 38 slides**

## 1. Intelligent Agents and Environments (Slides 3–5)

**Agent:** A system that receives information from its **environment** through **sensors** and acts on the environment through **actuators**.

**Simple meaning:** An agent observes what is happening and chooses an action.

- **Environment:** The world or surroundings in which the agent operates.
- **Sensor:** A device or input mechanism that receives information.
- **Percept:** The information received by the agent at a particular time.
- **Percept sequence:** The history of percepts received so far.
- **Actuator:** The mechanism used to carry out an action.
- **Action:** What the agent actually does.

**Example: Vacuum robot**
- Sensor detects dirt → percept: `[A, Dirty]`.
- Agent decides to clean → actuator operates suction → action: `Suck`.

**Agent = Architecture + Program**
- **Architecture:** Hardware/computing platform on which the agent runs.
- **Agent program:** Instructions that implement the agent's behaviour.

**Agent function:**

\[
f:P^*\rightarrow A
\]

- **f:** Agent function.
- **P:** Set of possible percepts.
- **P\*:** Set of possible percept histories (sequences).
- **A:** Set of possible actions.

**Simple meaning:** For any history of observations, the agent function specifies the action to take.

**★ Exam:** The **agent function** describes the behaviour; the **agent program** implements that behaviour on an architecture.

**Characteristics of an intelligent agent:**
1. **Autonomy:** Operates without constant human instructions.
2. **Reactivity:** Responds to changes in the environment.
3. **Proactiveness:** Takes initiative to work toward a goal.
4. **Social Ability:** Can interact/communicate with people or other agents (not needed by every agent).

Examples: Robots, software agents (softbots), virtual assistants, thermostats.

**★ Remember:** Reactivity = *respond to changes*. Proactiveness = *work toward a goal*.

---

## 2. Vacuum-Cleaner World (Slides 6–7)

A simple environment with **two squares: A and B**. Each square is **Clean** or **Dirty**. The robot is in one square at a time.

- **Percept:** `[location, status]`, e.g. `[A, Dirty]`.
- **Actions:** `Left`, `Right`, `Suck`, `NoOp` (do nothing).

**Reflex-Vacuum-Agent rules:**

```text
IF status = Dirty     → Suck
ELSE IF location = A  → Right
ELSE IF location = B  → Left
```

| Current percept | Selected action |
|---|---|
| `[A, Dirty]` | Suck |
| `[A, Clean]` | Right |
| `[B, Dirty]` | Suck |
| `[B, Clean]` | Left |

**Problem:** After both squares become clean, the robot may keep moving between A and B. The program uses only the present percept and does not remember that both squares are clean.

**★ Exam:** The challenge is to produce useful/rational behaviour with a **small program**, rather than a huge table listing every possible percept history.

---

## 3. Rationality and Performance Measures (Slides 8–10)

**Rational Agent:** Chooses the action expected to **maximize its performance measure**, using the available percept history and built-in knowledge.

**Simple meaning:** Makes the best *expected* decision based on what it knows.

**Rationality depends on four things:**
1. **Performance measure:** What counts as success.
2. **Prior knowledge:** What the agent already knows about the environment.
3. **Available actions:** What the agent can do.
4. **Percept sequence:** What the agent has observed so far.

**Performance measure:** A fixed way of evaluating the outcomes produced by an agent.

Vacuum-world examples from the lecture:
- One point for each square cleaned within time **T** (a time limit).
- One point for each clean square per time step, minus one point per move.
- A penalty if more than **k** squares are dirty (**k** = a chosen number).

**Good measure:** Keep squares clean while minimizing time and energy. Merely visiting many squares is not enough.

**Important distinctions:**
- **Rational ≠ Omniscient:** Does not know everything; observations may be incomplete.
- **Rational ≠ Clairvoyant:** Cannot know future events with certainty.
- **Rational ≠ Successful:** A good decision may still fail because outcomes are uncertain.

**Example:** A robot correctly chooses `Suck`, but the motor unexpectedly fails. The choice can still have been rational.

**★ Exam:** Rationality is about **expected performance**, not guaranteed success. Rational agents may need **exploration, learning and autonomy**.

---

## 4. PEAS: Describing a Task Environment (Slides 11–15)

**PEAS** helps specify the task of an agent before designing it.

- **P = Performance Measure:** How is success judged?
- **E = Environment:** Where does the agent operate?
- **A = Actuators:** How does it perform actions?
- **S = Sensors:** How does it get information?

| Agent | Performance Measure (P) | Environment (E) | Actuators (A) | Sensors (S) |
|---|---|---|---|---|
| **Automated taxi** | Safety, destination, profit, legality, comfort | Streets, traffic, pedestrians, weather | Steering, accelerator, brakes, horn, display | Video, GPS, gauges, accelerometers, engine sensors |
| **Internet shopping agent** | Price, quality, appropriateness, efficiency | Web sites, vendors, shippers | Display results, follow URLs, fill forms | HTML pages: text, graphics, scripts |
| **ChatGPT assistant** | Accuracy, relevance, response time, user satisfaction | Current/future WWW sites (lecture's description) | Display answers, plugins to interact with web/databases | Text, graphics, voice |

**Appropriateness:** How suitable a product is for the user's actual needs.

**★ Remember:** A **sensor receives** information; an **actuator performs** an action. For software agents, these can be software inputs and outputs, not physical parts.

---

## 5. Environment Types (Slides 16–17)

**Why classify environments?** The kind of environment largely determines the design of a suitable agent.

| Classification | First type | Opposite type |
|---|---|---|
| **Observability** | **Fully Observable:** All relevant current-state information is available. Example: Chess. | **Partially Observable:** Some relevant information is hidden. Example: Poker (opponents' cards). |
| **Predictability** | **Deterministic:** Current state + action completely determine next state. Example: Chess move. | **Stochastic:** Next state is not completely predictable. Example: Stock market. |
| **Decision dependence** | **Episodic:** Each decision episode is independent. Example: Independent quiz questions. | **Sequential:** Current decisions affect later situations. Example: Football. |
| **Change over time** | **Static:** Environment does not change while the agent thinks. Example: Chess without a clock. | **Dynamic:** Environment can change while the agent thinks. Example: Driving. |
| **Values of states/actions** | **Discrete:** Separate, countable states/actions. Example: Tic-tac-toe. | **Continuous:** Values can vary over a range. Example: Steering angle. |
| **Number of agents** | **Single-Agent:** One decision-making agent. Example: Sudoku solver. | **Multi-Agent:** Several interacting agents. Example: Stock trading systems. |

**Semi-Dynamic:** The environment state does not change while the agent thinks, but time affects its performance. Example: **Chess with a clock**.

**Do not confuse:**
- **Stochastic:** Is the next outcome uncertain?
- **Dynamic:** Can the world change while the agent is deciding?
- **Continuous:** Can state/action values vary over a range?
- **Sequential:** Do decisions affect what happens later?

**Taxi driving:** Partially Observable, Stochastic, Sequential, Dynamic, Continuous, Multi-Agent.

Other environments shown in the lecture's classification table include crossword puzzles, poker, backgammon, medical diagnosis, image analysis, part-picking robots, refinery controllers and English tutors.

**★ Exam:** The lecture describes the **real world** in general as partially observable, stochastic, sequential, dynamic, continuous and multi-agent.

---

## 6. Simple Reflex Agents (Slide 18)

**Simple Reflex Agent:** Chooses an action using the **current percept** and fixed **condition-action rules**.

**Condition-action rule:** `IF condition is true → THEN perform action`.

**Main steps:**
1. Receive a percept from sensors.
2. Check which rule matches the current situation.
3. Perform the corresponding action through actuators.

**Example:** `IF Dirty → Suck`; otherwise move to the other square.

- **Strength:** Simple and direct decision-making when the current percept is enough.
- **Weakness:** No memory of earlier percepts; can behave poorly if important information is hidden.

**★ Remember:** Simple Reflex = **Current Percept + Condition-Action Rules**.

---

## 7. Goal-Based Agents and Romania Example (Slides 19–21)

**Goal-Based Agent:** Chooses actions by considering a **goal** and what might happen after each action.

**Simple meaning:** Asks, *Which action will help me reach the desired outcome?*

The agent uses information about:
- Its **state** (what the world is like now).
- **How the world evolves** (how situations change).
- **What actions do** (their possible effects).
- Its **goal** (desired final state).

**Main steps:** Identify current state → Set goal → Predict outcomes → Select actions that can reach the goal.

**Romania route-finding example:**
- Current city: **Arad**.
- Goal: **Be in Bucharest** before the flight.
- States: Cities.
- Actions: Drive between connected cities.
- Example solution: **Arad → Sibiu → Fagaras → Bucharest**.
- Distance: **140 + 99 + 211 = 450 km**.

This is a valid example route, **not necessarily the shortest route**.

**Simple Reflex vs. Goal-Based:**
- **Simple Reflex:** Follows a rule based on what it senses now.
- **Goal-Based:** Considers future results and plans to achieve a goal.

**★ Exam:** Goal-based agents can use **search and planning** to find a sequence of actions leading to a goal.

---

## 8. Problem Formulation (Slide 22)

**Problem Formulation:** Defines a problem clearly so an agent can search for a solution.

A problem has **four components**:

**1. Initial State:** The starting situation. Example: `At Arad`.

**2. Successor Function:** Returns available **action–state pairs** from a given state.

\[
S(x)=\text{set of (action, resulting state) pairs}
\]

- **S:** Successor function.
- **x:** Current state.
- **S(x):** Which actions can be taken and where they lead.

Example: From Arad, possible next cities include Zerind, Sibiu and Timisoara.

**3. Goal Test:** Checks whether the goal has been reached.
- **Explicit goal:** `x = At Bucharest`.
- **Implicit goal:** `NoDirt(x)` (no dirt remains).

**4. Path Cost:** Sum of the costs of actions along a path.

\[
c(x,a,y)\geq 0
\]

- **c(x, a, y):** Cost of taking action **a** in state **x** to reach state **y**.
- **≥ 0:** The lecture assumes the cost of each step is not negative.

**Example:**
- Arad → Sibiu = 140 km.
- Sibiu → Fagaras = 99 km.
- Fagaras → Bucharest = 211 km.
- **Total path cost = 140 + 99 + 211 = 450 km.**

Possible cost measures: Distance, execution time, number of actions.

**Solution:** A **sequence of actions** from the initial state to a goal state.

**★ Remember:** Start = Initial State; Options = Successor Function; Finished? = Goal Test; Total expense = Path Cost.

---

## 9. Utility-Based Agents (Slides 23–25)

**Utility-Based Agent:** Chooses an action by evaluating how desirable its possible outcomes are, using a **utility function**.

**Utility:** A numerical score representing how good an outcome is.

**Why needed?** Several actions may reach the same goal, but differ in safety, cost, speed or comfort.

**Goal-Based vs. Utility-Based:**
- **Goal-Based:** *Does this action achieve the goal?*
- **Utility-Based:** *Which possible outcome is better?*

**Main steps:** Predict outcomes → Calculate/estimate utility → Choose the action with highest **expected utility**.

### A. Autonomous Vehicles

\[
U=w_1(Safety)+w_2(TravelTime)+w_3(FuelEfficiency)+w_4(PassengerComfort)
\]

- **U:** Overall utility score.
- **w₁–w₄:** Weights showing how important each factor is.
- **Safety, TravelTime, FuelEfficiency, PassengerComfort:** Scores for these factors.

**Weight:** A number showing the importance of a factor.

**Tiny example (simplified to two factors):**

\[
U=3(Safety)+1(Comfort)
\]

- Route A: Safety 9, Comfort 7 → **U = 3(9) + 7 = 34**.
- Route B: Safety 6, Comfort 9 → **U = 3(6) + 9 = 27**.
- **Choose A** because 34 > 27.

**Note on the slide:** If `TravelTime` is actual journey duration, giving it a positive weight would reward *longer* journeys. The factor must instead be a higher-is-better travel-time score, or have an appropriate negative weight.

### B. Financial Trading Systems

\[
U=ExpectedReturn-RiskAversion\times Risk
\]

- **ExpectedReturn:** Expected profit/return score.
- **RiskAversion:** How strongly the agent dislikes risk.
- **Risk:** Estimated risk score.

Example: `12 − 2 × 3 = 6` utility points.

**Meaning:** High returns may not be worth high risk.

### C. Recommendation Systems

\[
U=PredictedUserRating-w\times ItemPopularity
\]

- **PredictedUserRating:** Expected user rating.
- **w:** Importance assigned to popularity.
- **ItemPopularity:** Popularity score of an item.

Example: `9 − 0.5 × 4 = 7`.

**Note on the slide:** With a positive **w**, subtracting popularity **penalizes more popular items**. This is the lecture's formula; it is not a general rule that recommendation systems must penalize popularity.

### D. Healthcare Management

\[
U=w_1(PatientOutcomes)+w_2(ResourceUtilization)+w_3(WaitingTimes)
\]

- **PatientOutcomes:** Quality of patient results.
- **ResourceUtilization:** How effectively resources are used.
- **WaitingTimes:** Patient waiting-time measure.
- **w₁–w₃:** Weights for each factor.

**Note on the slide:** If `WaitingTimes` means actual time spent waiting, a positive weight rewards longer waits. Use a negative weight or a higher-is-better waiting-time score to reward shorter waits.

**★ Exam:** A rational agent **maximizes expected performance based on its knowledge**, rather than always achieving the optimal outcome.

---

## 10. Learning Agents (Slides 26–27)

**Learning Agent:** An agent that improves its behaviour through experience and feedback.

**Four components:**

| Component | Main job | Simple question |
|---|---|---|
| **Performance Element** | Chooses actions using current knowledge | What should I do? |
| **Critic** | Evaluates performance against a standard | How well did I do? |
| **Learning Element** | Uses feedback to improve the performance element | How can I improve? |
| **Problem Generator** | Suggests exploratory actions to learn new information | What new thing should I try? |

**Learning process:**
1. **Performance Element** chooses an action.
2. The environment produces a result.
3. **Critic** evaluates the result and gives feedback.
4. **Learning Element** uses feedback to improve the agent.
5. **Problem Generator** suggests trying less-explored possibilities to gain information.

**Example 1: Amazon Recommendation System**
- Performance Element → Recommends a product.
- Critic → Evaluates clicks, ratings and engagement.
- Learning Element → Learns from clicks/purchase history.
- Problem Generator → Suggests unfamiliar product categories to learn interests.

**Example 2: AlphaGo (Game-Playing AI)**
- Performance Element → Chooses a Go move.
- Critic → Evaluates play/game outcomes.
- Learning Element → Improves from past games.
- Problem Generator → Explores different moves and strategies.

**★ Exam:** **Critic evaluates**, **Learning Element improves**, **Problem Generator explores**. These are different tasks.

---

## 11. Problem Types (Slide 28)

Problem type depends on how much the agent knows and how predictable the world is.

**1. Single-State Problem**
- **Condition:** Deterministic and fully observable.
- **Meaning:** Agent knows exactly where it is and where actions will lead.
- **Solution:** A fixed sequence of actions.

**2. Sensorless Problem**
- **Condition:** Non-observable environment.
- **Meaning:** Agent cannot observe its current state.
- **Solution:** A sequence that works despite uncertainty, if one exists.
- **Example:** A two-room vacuum can use `Left → Suck → Right → Suck` without knowing its start location (assuming movements work as expected).

**3. Contingency Problem**
- **Condition:** Nondeterministic and/or partially observable.
- **Meaning:** Must adjust actions depending on new observations or results.
- **Solution:** **Contingent plan** (a plan with IF/THEN branches) or **policy** (rules for choosing actions in different situations).
- **Example:** If the first door is locked, try a different door.
- **Important:** Search/planning and execution are often **interleaved** (alternate: plan → act → observe → update plan).

**4. Exploration Problem**
- **Condition:** Unknown state space.
- **Meaning:** Agent must discover states and connections while acting.
- **Also called:** **Online** exploration/problem-solving.
- **Example:** A robot finding its way through an unfamiliar maze without a map.

**★ Do not confuse:**
- **Sensorless:** Cannot observe current state.
- **Contingency:** Can use observations/results to adjust plan.
- **Exploration:** Does not know the state space or connections in advance.
- **Single-state ≠ Single-agent:** The first concerns state certainty; the second concerns the number of agents.

---

## 12. Selecting a State Space and Abstraction (Slide 29)

**State Space:** The set of possible states considered in a problem.

**Problem:** The real world contains too many details to model everything.

**Abstraction:** Simplifying a problem by keeping information needed for the task and ignoring unnecessary details.

**Abstract State:** Represents a **set of relevant real-world states**.
- Example: `At Arad` represents many exact positions inside Arad.

**Abstract Action:** Represents many smaller real actions.
- Example: `Arad → Zerind` represents driving, turns, stops and possible detours.

**Abstract Solution:** A simplified plan representing real paths that solve the real-world problem.

**Guaranteed Realizability:** For an abstract action to be guaranteed, it must be possible to get from **any real state** represented by the starting abstract state to **some real state** represented by the destination abstract state.

**Example:** If `At Arad` includes a broken-down car that cannot travel, then `Arad → Zerind` is not guaranteed for every represented real situation.

**★ Remember:** Each abstract action should be **easier to handle than the original detailed problem**. Otherwise, abstraction has not simplified planning.

---

## 13. State-Space Examples (Slides 30–32)

### A. Vacuum World State-Space Graph

- **States:** Robot location and dirt status of A and B.
- **Actions:** `Left`, `Right`, `Suck`, `NoOp`.
- **Goal Test:** No dirt remains.
- **Path Cost:** 1 per action; 0 for `NoOp`.

**Number of possible states:**
- Robot position: 2 choices.
- A: Clean/Dirty = 2 choices.
- B: Clean/Dirty = 2 choices.

\[
2\times2\times2=8\text{ states}
\]

**Example solution:** Start at A, both dirty:

`Suck → Right → Suck`

Total path cost = **1 + 1 + 1 = 3**.

**State-Space Graph:**
- **Nodes:** States.
- **Arrows:** Actions leading from one state to another.
- **Self-loop:** An action leaves the state unchanged, e.g. `Left` when already at A.

### B. 8-Puzzle

A 3 × 3 sliding puzzle with 8 numbered tiles and one blank square.

- **States:** Positions of numbered tiles and the blank.
- **Actions:** Move the **blank** Left, Right, Up or Down, when allowed.
- **Goal Test:** Tiles match the specified goal arrangement.
- **Path Cost:** 1 per move.

Start from the lecture:

```text
7 2 4
5 _ 6
8 3 1
```

Goal:

```text
1 2 3
4 5 6
7 8 _
```

Since the blank starts in the centre, it can move in **four directions**.

**Example:** Moving blank Right swaps it with **tile 6**, producing:

```text
7 2 4
5 6 _
8 3 1
```

**★ Remember:** Count moves of the **blank**, not arbitrary movement of numbered tiles.

### C. Robotic Assembly

- **States:** Real-valued robot joint angles and positions of object parts.
- **Actions:** Continuous movements of robot joints.
- **Goal Test:** Assembly completed; the robot is **not part of the assembled object**.
- **Path Cost:** Time needed to execute the assembly.

**Comparison:** 8-Puzzle uses **discrete** positions/moves. Robotic assembly involves **continuous** positions/movements.

**Lecture poll (Slide 33):** The **successor function** describes actions available from a state **and their resulting states**. Of the slide's options, **“Actions the agent can take”** is the closest answer, but it is an incomplete description.

---

## 14. Current Research, Future Directions and Multi-Agent Systems (Slides 34–35)

**Current research areas:**
- **Multi-Agent Systems:** Several interacting intelligent agents.
- **Human-Agent Interaction:** How people and agents communicate/work together.
- **Agent Learning:** Improving agent decisions through experience.

**Open problems:**
- **Scalability:** Can a system still work well as its size or workload grows?
- **Robustness:** Can it handle errors, disturbances and unexpected situations?
- **Commonsense Reasoning:** Can it use everyday knowledge in ordinary situations?

**Possible future applications:** Space exploration, nanotechnology and human augmentation (technology that extends human abilities).

**Multi-Agent System (MAS):** A system containing multiple intelligent agents, each with its own goals and capabilities, that interact with one another.

**Applications:**
- **Traffic Management:** Coordinate autonomous vehicles and reduce congestion.
- **Smart Grids:** Balance electricity generation and demand.
- **Robotics:** Several robots complete manufacturing or logistics tasks.
- **Gaming:** Multiple AI-controlled characters interact.

**Main challenges:**
1. **Communication and Coordination:** Agents must exchange information and handle possibly conflicting goals.
2. **Decision-Making:** Agents must choose actions in dynamic and uncertain situations.
3. **Learning:** Agents must learn from other agents and the environment.

**★ Remember:** Multi-agent systems may involve **cooperation** or **competition**.

---

## 15. Ethical and Societal Implications + Final Review (Slides 36–38)

**Five major ethical/social concerns:**

1. **Algorithmic Bias and Fairness:** Avoid unfair decisions that disadvantage certain people or groups.
2. **Transparency and Explainability:** Make it possible to understand why an agent made a decision.
3. **Privacy and Security:** Protect personal information and prevent unauthorized access.
4. **Accountability and Legal Frameworks:** Decide who is responsible for an agent's actions and how rules apply.
5. **Societal Impact and Workforce Displacement:** Consider the effects of AI on people, jobs and society.

**Example:** An AI hiring system should not unfairly reject qualified applicants, and its decisions should be explainable.

### ★ Final Exam Checklist

- **Agent = Architecture + Program**.
- **Agent Function:** `f: P* → A`.
- **Sensors = Input; Actuators = Output/Action mechanism**.
- **Rational Agent:** Maximizes **expected** performance using available evidence.
- **PEAS:** Performance Measure, Environment, Actuators, Sensors.
- **Six environment comparisons:** Observable, Deterministic, Episodic, Static, Discrete, Single-Agent, each with an opposite type.
- **Simple Reflex:** Current percept + condition-action rules.
- **Goal-Based:** Actions selected to achieve a goal.
- **Utility-Based:** Compare desirability of possible outcomes.
- **Learning Agent:** Performance Element, Critic, Learning Element, Problem Generator.
- **Problem Formulation:** Initial State, Successor Function, Goal Test, Path Cost.
- **Problem Types:** Single-State, Sensorless, Contingency, Exploration.
- **Abstraction:** Keep useful details, remove unnecessary detail.
- **Vacuum World:** 8 states for 2 locations and 2 possible dirt conditions per location.
- **8-Puzzle:** Blank moves; path cost is 1 per move.
- **Robotic Assembly:** Continuous states and actions; cost may be execution time.
- **Multi-Agent Systems:** Interaction, communication, coordination and learning.
- **Ethics:** Fairness, explainability, privacy, accountability and job impacts.

**Lecture's final summary also names “reflex with state” as an agent architecture.** Unlike the simple reflex agent, a reflex agent with state maintains an internal representation of information from the past. The lecture does not provide a separate detailed slide explaining this design.

**End of Lecture 4.**
