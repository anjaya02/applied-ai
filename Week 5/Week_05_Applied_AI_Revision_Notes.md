# Applied AI — Week 05 Revision Notes

**Lecture 5: Knowledge Representation, Logic and Inference**  
**Purpose:** Understand and remember the important examinable ideas without rereading every slide.  
**Priority:** Based on the lecture and the previous-exam questions we reviewed. Past questions show what *has* appeared, not everything that *can* appear.

---

## 1. Knowledge Representation (KR)

**Why do we need it?** An AI system needs a way to store knowledge so it can use that knowledge to answer questions and make decisions.

- **Knowledge Base (KB):** A collection of facts and rules.
- **Inference:** Using existing facts and rules to derive a conclusion.
- **Logical representation:** Knowledge written using logic, such as propositional or first-order logic.
- Other representation types mentioned in the lecture:
  - **Rule-based:** IF–THEN / production rules (and constraint satisfaction problems).
  - **Graphical:** Semantic networks, Bayesian networks, Markov networks.
  - **Structured:** Frames and ontologies.

**Exam memory:** Facts tell us what is known; rules tell us what follows if certain conditions hold.

## 2. Propositional Logic (PL)

PL represents statements that are either **TRUE** or **FALSE**.

Example:

- `P`: It is raining.
- `Q`: The road is wet.
- `P ⇒ Q`: If it is raining, the road is wet.

### Main symbols

| Symbol | Name | Everyday meaning |
|---|---|---|
| `¬P` | NOT (negation) | P is not true |
| `P ∧ Q` | AND (conjunction) | Both P and Q |
| `P ∨ Q` | OR (disjunction) | At least one is true (possibly both) |
| `P ⇒ Q` | Implication | If P, then Q |
| `P ⇔ Q` | Biconditional | P if and only if Q |

### Implication and biconditional — common traps

| P | Q | `P ⇒ Q` | `P ⇔ Q` |
|---|---|---|---|
| T | T | T | T |
| T | F | **F** | F |
| F | T | T | F |
| F | F | T | T |

- `P ⇒ Q` is **false only if P is true but Q is false**. A rule that says “if P then Q” is broken only in that situation.
- `P ⇔ Q` is true when P and Q have the **same** truth value.
- **Unknown is not the same as false.** If a KB contains a trusted rule `P ⇒ Q` and the fact `P`, we can infer `Q`, even if Q was initially unknown. This is **Modus Ponens**.
- An implication **does not say P is true**. `P ⇒ Q` by itself cannot prove Q.

### Syntax, semantics, models

- **Syntax:** What expressions are allowed; how statements are written.
- **Semantics:** What those expressions mean and when they are true.
- **Model:** An assignment of truth values to propositions.
- With `n` different propositions there are `2^n` possible truth assignments. Example: `P, Q, R` gives `2³ = 8` models.

## 3. Entailment, Validity and Satisfiability

### Entailment

`KB ⊨ α` means **the KB entails α**: in every situation where the KB is true, α must also be true.

Example: If the KB contains `P ∧ Q`, then `KB ⊨ P`.

**Do not confuse** `⇒` (a connective inside a logical sentence) with `⊨` (a statement about what logically follows from a KB).

### Valid versus satisfiable

| Term | Exact meaning | Example |
|---|---|---|
| **Valid / tautology** | True in **every** model | `P ∨ ¬P` |
| **Satisfiable** | True in **at least one** model | `P ∧ Q` |
| **Unsatisfiable / contradiction** | True in **no** model | `P ∧ ¬P` |

**Important:** Every valid sentence is satisfiable, but a satisfiable sentence need not be valid.

### Refutation (proof by contradiction)

To prove `KB ⊨ Q`:

1. Keep all KB statements.
2. Temporarily add `¬Q` (pretend the conclusion is false).
3. If this makes the statements **unsatisfiable** (a contradiction), Q follows from the KB.

Equivalent idea: `KB ⊨ Q` exactly when `KB ∧ ¬Q` is unsatisfiable.

## 4. Useful Logical Equivalences

These help simplify sentences and convert to **Conjunctive Normal Form (CNF)**.

| Law | Equivalent forms |
|---|---|
| Double negation | `¬¬P ≡ P` |
| Implication elimination | `P ⇒ Q ≡ ¬P ∨ Q` |
| Biconditional elimination | `P ⇔ Q ≡ (¬P ∨ Q) ∧ (¬Q ∨ P)` |
| De Morgan 1 | `¬(P ∧ Q) ≡ ¬P ∨ ¬Q` |
| De Morgan 2 | `¬(P ∨ Q) ≡ ¬P ∧ ¬Q` |
| Contraposition | `P ⇒ Q ≡ ¬Q ⇒ ¬P` |
| Distributivity (important) | `(A ∧ B) ∨ C ≡ (A ∨ C) ∧ (B ∨ C)` |

Also recognise **commutativity** (`P ∨ Q ≡ Q ∨ P`, similarly AND) and **associativity** (regroup the same operator without changing meaning).

## 5. CNF — Very High Exam Priority

**CNF = Conjunctive Normal Form.** It means **AND (`∧`) of clauses**, where each clause is an **OR (`∨`) of literals** (a proposition or its negation).

Example CNF: `(¬P ∨ Q) ∧ (P ∨ R)`.

### Standard method

1. Remove `⇔` (replace with two implications).
2. Remove `⇒` (use `¬P ∨ Q`).
3. Move `¬` inward with De Morgan and cancel `¬¬`.
4. Distribute OR across AND if necessary.
5. Write the answer as AND-separated OR clauses.

### Previous exam: 6 marks — Question 1

Convert `(P ⇔ Q) ∧ R` into CNF.

1. `P ⇔ Q` becomes `(P ⇒ Q) ∧ (Q ⇒ P)`.
2. Remove implications: `(¬P ∨ Q) ∧ (¬Q ∨ P)`.
3. Keep the extra `∧ R`.

**Answer:** `(¬P ∨ Q) ∧ (¬Q ∨ P) ∧ R`.

### Previous exam: 6 marks — Question 2

Convert `(P ⇔ Q) ∨ R` into CNF.

1. Replace biconditional: `[(¬P ∨ Q) ∧ (¬Q ∨ P)] ∨ R`.
2. **Distribute the outside OR to both AND groups**:
   - First group becomes `(¬P ∨ Q ∨ R)`.
   - Second group becomes `(¬Q ∨ P ∨ R)`.

**Answer:** `(¬P ∨ Q ∨ R) ∧ (¬Q ∨ P ∨ R)`.

**Exam trap:** `(... ) ∧ R` is already CNF after removing `⇔`, while `(... ) ∨ R` needs **distributivity**.

## 6. Propositional Resolution — Very High Exam Priority

**Why use it?** To prove whether a conclusion logically follows from a KB by using contradiction.

### Resolution rule

Take two OR clauses containing opposite literals. Cancel the pair and combine what remains.

Example:

- Clause 1: `P ∨ Q`
- Clause 2: `¬P ∨ R`
- Cancel `P` and `¬P`.
- **Resolvent:** `Q ∨ R`.

You can cancel complementary literals **only when they occur in two clauses used in the resolution step**.

### Prove a conclusion with Resolution

1. Convert the KB into CNF clauses.
2. Add the **negation of the conclusion** as a new clause.
3. Pick clauses with a complementary pair (`P` and `¬P`).
4. Resolve and add the new clause.
5. Continue until you get the **empty clause `□`** (contradiction), or no useful clauses remain.
6. If you derive `□`, the conclusion is proved. If not, do **not** automatically claim the conclusion is true.

### Tiny successful proof

KB: `P ⇒ Q` and the fact `P`. Prove Q.

- Convert rule: `¬P ∨ Q`.
- Fact: `P`.
- Negate goal: `¬Q`.
- Resolve `(¬P ∨ Q)` with `P` to get `Q`.
- Resolve `Q` with `¬Q` to get `□`.
- Therefore **Q is entailed**.

### Previous exam: 7 marks — The important trap

Given:

- `P ⇒ R`
- `P ⇒ Q`
- `R ⇒ S`

Options included `S`, `Q ∧ R`, `None of the other options`, and `¬P ∧ S`.

**Correct option: None of the other options.**

Why? The KB **never states that P is true**. Every rule is conditional. The rules let us infer `P ⇒ S`, but they **do not** let us conclude `S` or `Q` is true.

CNF clauses:

- `¬P ∨ R`
- `¬P ∨ Q`
- `¬R ∨ S`

We can resolve the first and third to derive `¬P ∨ S`, meaning `P ⇒ S`. We **still cannot infer S**, because no fact gives us P.

**Exam trick:** A rule's condition is **not** automatically a fact.

## 7. Forward Chaining versus Backward Chaining

Both use rules to infer conclusions, especially rules such as `A ∧ B ⇒ C` (both A and B are needed).

| Feature | Forward Chaining | Backward Chaining |
|---|---|---|
| Starts at | Known facts | Goal/query |
| Direction | Facts → conclusions | Goal → required supporting facts |
| Name | **Data-driven** | **Goal-driven** |
| Useful when | Discovering what follows from available data | Answering a specific question |
| Possible issue | Derives irrelevant information | Repeated subgoals / loops |

### Actual lecture KB example

Rules:

1. `P ⇒ Q`
2. `L ∧ M ⇒ P`
3. `B ∧ L ⇒ M`
4. `A ∧ P ⇒ L`
5. `A ∧ B ⇒ L`

**Known facts:** `A`, `B`. **Goal:** `Q`.

**Forward:** `A, B` → infer `L` (rule 5) → infer `M` (rule 3) → infer `P` (rule 2) → infer `Q` (rule 1).

**Backward:** Need `Q` → need `P` → need `L` and `M` → prove `L` using facts `A`, `B` → prove `M` using `B`, `L` → prove `P` → prove `Q`.

**Exam trap:** Do not apply `A ∧ B ⇒ L` if only A is known. AND needs **every input condition**.

## 8. First-Order Logic (FOL)

**Why FOL?** Propositional logic treats whole statements as P, Q, etc. It cannot easily express general statements about **objects** and their **relationships**. FOL can.

### Building blocks

| Term | Meaning | Example |
|---|---|---|
| Constant | A specific object | `Anjaya` |
| Variable | A placeholder for an object | `x`, `y` |
| Predicate / relation | A claim that can be true or false | `Student(Anjaya)`, `Parent(A,B)` |
| Function | Returns an object | `MotherOf(Anjaya)` |
| Equality | Two terms refer to the same object | `x = y` |
| Connectives | NOT, AND, OR, implication, biconditional | `¬, ∧, ∨, ⇒, ⇔` |

An **atomic sentence** is a basic claim such as `Student(Anjaya)`. A **complex sentence** combines such claims using connectives and/or quantifiers.

### Quantifiers — important

**Universal `∀` = for all/every.**

“All students have IDs”:

`∀x (Student(x) ⇒ HasID(x))`

Meaning: For *any x*, **if** x is a student, x has an ID.

**Existential `∃` = there exists / at least one.**

“Some students like AI”:

`∃x (Student(x) ∧ Likes(x, AI))`

Meaning: There is at least one x who **is a student AND likes AI**.

**Exam memory:**

- **All → `∀` + usually `⇒`** when restricting a statement to a group.
- **Some → `∃` + usually `∧`** when describing a group member.
- `∀x P(x)` says every object satisfies P.
- `∃x P(x)` says at least one object satisfies P.

### Quantifier negation (useful exam pattern)

- `¬∀x P(x) ≡ ∃x ¬P(x)` — “Not everyone” means “Someone doesn't.”
- `¬∃x P(x) ≡ ∀x ¬P(x)` — “Nobody” means “Everyone doesn't.”

The **domain** tells us which objects x can represent. An **interpretation/model** tells us which objects constants refer to and which predicates are true.

### FOL relation example from lecture

`∀x∀y (Brother(x,y) ⇒ Sibling(x,y))`

Meaning: Every brother is a sibling. The reverse is not necessarily true.

### Other logics mentioned

Recognise the names and purpose; do not spend much revision time on them unless a tutorial or lecturer emphasises them:

- **Temporal logic:** Reasoning about time.
- **Probabilistic logic:** Reasoning with uncertainty.
- **Fuzzy logic:** Reasoning with degrees of truth.

## 9. Logic Programming and Prolog

**Logic Programming:** Write facts and rules; let the system infer answers to queries.

**Prolog** is a logic-programming language. It typically reasons **backward** from a query.

### Syntax

```prolog
parent(anne, bob).
parent(bob, carol).

grandparent(X, Z) :- parent(X, Y), parent(Y, Z).

?- grandparent(anne, carol).
```

- `parent(anne, bob).` = a **fact**.
- `grandparent(X, Z) :- ... .` = a **rule**.
- `:-` means **IF**. The head (conclusion) comes **first**.
- Comma `,` between rule conditions means **AND**.
- Names beginning with **lowercase** are atoms/constants (`anne`).
- Names beginning with **uppercase** are variables (`X`, `Y`).
- `?- ... .` is a **query**.
- The query above succeeds: Anne → Bob → Carol.

### Important execution terms

| Term | Meaning |
|---|---|
| **Horn clause** | A clause with at most one positive literal; supports rule-based inference |
| **Unification** | Matching terms and finding variable values. `parent(anne,Y)` matches `parent(anne,bob)` with `Y=bob` |
| **DFS / left-to-right** | Prolog generally tries subgoals from left to right, following one possibility deeply |
| **Backtracking** | If a route fails or more answers are requested, try an alternative match |
| **Closed-world assumption** | Treat what cannot be established from the knowledge base as false for this reasoning setting |
| **Negation as failure** | Failure to prove a goal can make a negated goal succeed; this is **not** proof of its real-world opposite |

**Exam trap:** A query can succeed by matching a **fact directly**; not every query needs a rule.

Example:

```prolog
likes(anne, tea).
?- likes(anne, tea).   % true: the fact exists
?- likes(anne, coffee).% fails: no matching proof here
```

The second result does **not** prove Anne dislikes coffee in real life.

## 10. Inference Quality: Soundness and Completeness

- **Soundness:** A method only derives conclusions logically supported by its premises. “Anything it proves is justified.”
- **Completeness:** A method can derive every conclusion that logically follows within the method's intended setting. “It can find all justified answers in that setting.”
- **Resolution** is sound and complete for propositional logic.
- **Forward/Backward Chaining** are complete for their relevant propositional Horn-clause setting.

## 11. What to Prioritise Before the Exam

**Highest priority — direct questions identified in the reviewed past papers:**

1. **CNF:** Implication/biconditional removal and **distributivity** (6-mark examples).
2. **Resolution:** Complementary literals, negated goal, empty clause; watch for missing facts (7-mark example).
3. **PL true/false traps:** `P ∧ ¬P` is **not** a tautology; PL is **not** expressive enough for every AI statement.

**Also examinable from the slides (even if not clearly seen in those samples):**

4. Entailment, validity, satisfiability, contradictions.
5. Forward vs backward chaining and short traces.
6. FOL symbols, `∀` versus `∃`, translating English sentences.
7. Prolog facts/rules/queries, unification, backtracking, closed-world assumption.
8. Soundness versus completeness, basic KR methods.

**Lower-priority slide detail:** Repeated animation frames, exhaustive model diagrams, external website/book recommendations and long Prolog list-processing demonstrations. Learn the underlying idea first; return to these only if your tutor highlights them.

## 12. Five Fast Self-Checks

Try these **before** looking at the answers.

1. Is `P ∧ ¬P` valid, satisfiable, or unsatisfiable?
2. Convert `P ⇒ Q` into an OR clause.
3. If the KB has `P ⇒ Q` but **no fact P**, can you conclude Q?
4. Translate “Some students like AI” into FOL.
5. In Prolog, what does `head :- a, b.` mean?

### Answers

1. **Unsatisfiable:** P and not-P cannot both hold.
2. `¬P ∨ Q`.
3. **No.** An implication does not establish its antecedent.
4. `∃x (Student(x) ∧ Likes(x, AI))`.
5. **If a AND b, then head.** Both conditions must succeed.

---

### One-minute memory sheet

- `⇒` false only when **T ⇒ F**; `⇔` true when both sides match.
- `KB ⊨ Q`: Every model satisfying KB also satisfies Q.
- Valid = **always** true; satisfiable = **sometimes** true; unsatisfiable = **never** true.
- CNF = **AND of OR clauses**; distribute OR across AND.
- Resolution = **cancel opposite literals**; add `¬goal`; empty clause `□` proves the goal.
- Forward = **facts → goal**; Backward = **goal → facts**.
- FOL: **∀ = all**, **∃ = some**; all usually uses `⇒`, some usually uses `∧`.
- Prolog: **head `:-` conditions**, backward chaining, unification, DFS/backtracking.
- Sound = only correct conclusions; complete = all entailed conclusions findable in its setting.

*Prepared as an exam-focused learning and revision aid from Applied AI Lecture 5 and the previous-exam questions reviewed in our study session.*
