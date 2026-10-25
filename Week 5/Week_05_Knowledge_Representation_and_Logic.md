# Week 5 - Knowledge Representation, Logic and Logic Programming

## 1. Knowledge Representation

### 1.1 What is Knowledge Representation?

**Knowledge Representation (KR)** is how an AI system stores facts, rules, and relationships so it can **reason** (use known information to reach conclusions).

The lecture introduces four main approaches:

| Approach | Simple meaning | Important types |
|---|---|---|
| **Logical representation** | Write knowledge using precise logical statements. | **Propositional Logic** (true/false facts); **First-Order Logic** (objects and relationships). |
| **Rule-based representation** | Use conditions and rules to decide what follows or what action to take. | **Production rules** (`IF ... THEN ...`); **Constraint Satisfaction Problems (CSPs)** (find values that obey all constraints). |
| **Graphical representation** | Use nodes and links to show relationships. | **Semantic Networks** (concept links); **Bayesian Networks** (directed links for uncertainty); **Markov Networks** (undirected links for dependencies). |
| **Structured representation** | Organize knowledge into predefined structures. | **Frames** (slots and values); **Ontologies** (formal descriptions of concepts and relationships). |

## 2. Foundations of Logic

### 2.1 Logic, Syntax and Semantics

**Logic** is a formal language used to represent information and draw conclusions.

- **Syntax:** Rules for writing a valid sentence. *How the sentence is written.*
- **Semantics:** The meaning of a sentence and when it is true. *What the sentence means.*

Example:

- `$x+2 \geq y$` has valid arithmetic syntax.
- `x² + y >` is not a complete, valid sentence.
- `$x+2 \geq y$` is **true** when `$x=7, y=1$`, but **false** when `$x=0, y=6$`.

**World:** One possible situation or assignment of values in which sentences can be evaluated.

### 2.2 Knowledge Base and Entailment

A **Knowledge Base (KB)** is a collection of statements that the system knows.

**Entailment** means a conclusion must be true whenever the KB is true:

$$KB \models \alpha$$

- `$KB$`: Knowledge Base.
- `$\alpha$` (alpha): The statement we want to check or prove.
- `$\models$`: **Entails** (logically guarantees).

**Simple meaning:** There is **no possible world** in which all KB statements are true but `$\alpha$` is false.

Example: If the KB says `It is raining`, then it entails `It is raining OR the grass is wet`. The OR statement must be true because its first part is true.

★ Exam: **Entailment is about truth in all possible models**, not just one example where the conclusion happens to be true.

## 3. Propositional Logic

### 3.1 Propositions and Logical Connectives

**Propositional Logic (PL)** represents whole statements as symbols, such as `$P$` and `$Q$`. Each statement is **true** or **false**.

| Symbol | Technical term | Simple meaning |
|---|---|---|
| `$\neg P$` | **Negation** | NOT P |
| `$P \land Q$` | **Conjunction** | P AND Q; both must be true |
| `$P \lor Q$` | **Disjunction** | P OR Q; at least one must be true |
| `$P \Rightarrow Q$` | **Implication** | IF P, THEN Q |
| `$P \Leftrightarrow Q$` | **Biconditional** | P IF AND ONLY IF Q; they have the same truth value |

If `$P$` and `$Q$` are valid sentences, combining them using these connectives also produces valid sentences.

### 3.2 Models and Truth Values

A **model** gives truth values to proposition symbols. A model of a particular sentence makes that sentence true.

With `$n$` proposition symbols, there are:

$$2^n \text{ possible truth assignments}$$

- `$n$`: Number of different proposition symbols.
- Example: 3 symbols `$P,Q,R$` give `$2^3=8$` possible assignments.

**Truth table:** Shows the result of each connective for every combination.

| P | Q | `$\neg P$` | `$P\land Q$` | `$P\lor Q$` | `$P\Rightarrow Q$` | `$P\Leftrightarrow Q$` |
|---|---|---|---|---|---|---|
| F | F | T | F | F | T | T |
| F | T | T | F | T | T | F |
| T | F | F | F | T | **F** | F |
| T | T | F | T | T | T | T |

★ Exam: An **implication** `$P\Rightarrow Q$` is **false only when P is true and Q is false**.

Example of evaluating a sentence:

If `$P_1=T, P_2=T, P_3=F$`, then

$$P_1\land(\neg P_2\lor\neg P_3)
=T\land(F\lor T)=T$$

### 3.3 Inference by Enumeration

**Enumeration** means checking every possible truth assignment (usually using a truth table).

To test `$KB\models\alpha$`:

1. List all possible models.
2. Find the rows where **KB is true**.
3. Check whether `$\alpha$` is also true in **every** such row.

Example: `$KB=P\land Q$`.

- `$KB\models P$` is **yes**: whenever both P and Q are true, P is true.
- `$KB\models\neg P$` is **no**: the true KB row has P true, not false.

**Properties:** Enumeration is **complete** (can determine the correct answer) but **exponential** (the number of cases grows like `$2^n$`).

### 3.4 Logical Equivalence

Two statements are **logically equivalent** if they have the same truth value in every model:

$$\alpha\equiv\beta
\quad\text{iff}\quad
(\alpha\models\beta)\land(\beta\models\alpha)$$

**Important equivalence laws:**

| Law | Formula |
|---|---|
| **Commutativity** | `$P\land Q\equiv Q\land P$`; `$P\lor Q\equiv Q\lor P$` |
| **Associativity** | `$(P\land Q)\land R\equiv P\land(Q\land R)$`; also for OR |
| **Double negation** | `$\neg\neg P\equiv P$` |
| **Contraposition** | `$P\Rightarrow Q\equiv\neg Q\Rightarrow\neg P$` |
| **Implication elimination** | `$P\Rightarrow Q\equiv\neg P\lor Q$` |
| **Biconditional elimination** | `$P\Leftrightarrow Q\equiv(P\Rightarrow Q)\land(Q\Rightarrow P)$` |
| **De Morgan's laws** | `$\neg(P\land Q)\equiv\neg P\lor\neg Q$`; `$\neg(P\lor Q)\equiv\neg P\land\neg Q$` |
| **Distributivity** | `$P\land(Q\lor R)\equiv(P\land Q)\lor(P\land R)$`; `$P\lor(Q\land R)\equiv(P\lor Q)\land(P\lor R)$` |

**Simple meaning:** These laws let us rewrite a logical expression **without changing its meaning**.

### 3.5 Validity and Satisfiability

- **Valid:** True in **all** possible models. Example: `$A\lor\neg A$`.
- **Satisfiable:** True in **at least one** model. Example: `$A\lor B$`.
- **Unsatisfiable:** True in **no** models. Example: `$A\land\neg A$`.

**Deduction theorem connection:**

$$KB\models\alpha
\quad\text{iff}\quad
(KB\Rightarrow\alpha)\text{ is valid}$$

**Proof by contradiction / refutation connection:**

$$KB\models\alpha
\quad\text{iff}\quad
KB\land\neg\alpha\text{ is unsatisfiable}$$

**Simple meaning:** Assume your conclusion is false. If that assumption clashes with the KB, your conclusion must follow from the KB.

Example: KB says *all birds have feathers* and *Tweety is a bird*. To prove *Tweety has feathers*, temporarily assume Tweety **does not** have feathers. This contradicts the KB, so the original conclusion follows.

★ Remember: **Valid = all models; satisfiable = at least one; unsatisfiable = none.**

## 4. Inference and Proof Methods

### 4.1 Two Ways to Prove Statements

**Inference** means deriving new statements from known statements.

1. **Inference rules:** Apply valid logical rules to old sentences to produce new ones. A **proof** is a sequence of such steps. Rules often need a special format, such as **Horn clauses** or **CNF**.
2. **Model checking:** Check possible models. **Truth-table enumeration** examines all truth assignments and is exponential. **Heuristic search** (for example, hill climbing) may be faster, but can be **incomplete** (miss an answer).

**Soundness:** A method never proves a conclusion that is **not entailed** by the KB.

**Completeness:** A method can find a proof for **every conclusion that is entailed** by the KB.

★ Exam: **Sound = no wrong proofs. Complete = no logically provable answers missed.**

### 4.2 Horn Clauses

A **Horn clause** is a logical clause with **at most one positive literal**. It can often be written as an IF-THEN rule, such as:

$$A\land B\Rightarrow L$$

This means: if both A and B are true, L is true.

Horn clauses are important because **forward chaining** and **backward chaining** work efficiently with them.

### 4.3 Forward Chaining (FC)

**Forward Chaining** is **data-driven**: start with known facts and use rules to discover new facts.

**How it works:**

1. Look for a rule whose **premises** (conditions before `⇒`) are all known to be true.
2. **Fire the rule:** Add its **conclusion** to the KB.
3. Repeat until the query is found or no more new facts can be added.

Lecture example:

```text
P => Q
L AND M => P
B AND L => M
A AND P => L
A AND B => L
Known facts: A, B
Goal: Q
```

Following the useful rules:

1. A and B are known, so derive **L**.
2. B and L are known, so derive **M**.
3. L and M are known, so derive **P**.
4. P is known, so derive **Q**. Goal proved.

**Strength:** Automatically discovers consequences of available facts.

**Weakness:** May derive many facts that are not needed to answer the query.

### 4.4 Backward Chaining (BC)

**Backward Chaining** is **goal-driven**: start with the fact you want to prove and work backwards.

**How it works:**

1. If the goal is already a known fact, it succeeds.
2. Otherwise, find a rule that **concludes** the goal.
3. Turn that rule's premises into smaller goals (**subgoals**).
4. Prove all required subgoals. If the needed rule path succeeds, the goal succeeds.

Using the same example:

- To prove **Q**, prove **P** (`P ⇒ Q`).
- To prove **P**, prove **L and M** (`L ∧ M ⇒ P`).
- To prove **M**, prove **B and L**.
- **L** can be proved from known facts **A and B**.
- Therefore M, P, and finally Q are proved.

**Avoid loops:** Do not keep trying a subgoal already on the current **goal stack** (the goals being worked on).

**Avoid repeated work:** Remember subgoals already proved or already failed.

### 4.5 Forward Chaining vs. Backward Chaining

| Feature | Forward Chaining | Backward Chaining |
|---|---|---|
| Starts with | Known **facts** | A specific **query/goal** |
| Direction | Facts → conclusions | Goal → required facts |
| Also called | **Data-driven** | **Goal-driven** |
| Main weakness | May calculate irrelevant facts | Must handle loops and repeated subgoals |
| Best when | We want to discover what follows from data | We want to answer a particular question |

★ Exam: The lecture states that **FC and BC are complete for Horn clauses** and can work in **linear time** with suitable implementations. BC may search much less than the full KB when only a small part is relevant.

**Memory trick:** **Forward = Facts first. Backward = Goal first.**

## 5. Resolution and Conjunctive Normal Form

### 5.1 Literals, Clauses and CNF

- **Literal:** A proposition or its negation, such as `$A$` or `$\neg A$`.
- **Clause:** Literals joined by OR, such as `$A\lor\neg B$`.
- **Conjunctive Normal Form (CNF):** Clauses joined by AND.

Example of CNF:

$$ (A\lor\neg B)\land(B\lor\neg C\lor\neg D) $$

**Simple meaning:** CNF is an **AND of OR groups**. Resolution uses clauses in this form.

### 5.2 Resolution Rule

**Resolution** combines two clauses that contain **complementary literals** (a symbol and its opposite).

$$\frac{\alpha\lor\beta \qquad \neg\beta\lor\gamma}{\alpha\lor\gamma}$$

- `$\alpha,\beta,\gamma$`: Logical statements or remaining groups of literals.
- `$\beta$` and `$\neg\beta$`: Complementary literals; they cancel.
- **Resolvent:** The new clause `$\alpha\lor\gamma$`.

Small example:

$$ (A\lor B),\quad \neg B
\quad\Longrightarrow\quad A $$

★ Exam: Resolution is **sound and complete for propositional logic** (used as a refutation proof method).

### 5.3 Converting a Sentence into CNF

Lecture example:

$$B_1\Leftrightarrow(P_1\lor P_2)$$

1. **Remove the biconditional** (`⇔`):

   $$(B_1\Rightarrow(P_1\lor P_2))\land((P_1\lor P_2)\Rightarrow B_1)$$

2. **Remove implications** using `$X\Rightarrow Y\equiv\neg X\lor Y$`:

   $$(\neg B_1\lor P_1\lor P_2)\land(\neg(P_1\lor P_2)\lor B_1)$$

3. **Move NOT inwards** using De Morgan's laws:

   $$(\neg B_1\lor P_1\lor P_2)\land((\neg P_1\land\neg P_2)\lor B_1)$$

4. **Distribute OR over AND** and flatten:

   $$(\neg B_1\lor P_1\lor P_2)\land(\neg P_1\lor B_1)\land(\neg P_2\lor B_1)$$

The final expression is CNF because it is an **AND of clauses**.

★ Remember: **Remove ⇔ → remove ⇒ → push ¬ inward → distribute ∨ over ∧.**

### 5.4 Resolution Algorithm (Proof by Contradiction)

To check whether `$KB\models\alpha$`:

1. Convert the KB statements to **CNF clauses**.
2. Add `$\neg\alpha$` (the opposite of the query).
3. Choose two clauses with complementary literals and **resolve** them.
4. Add any new resolvent and continue with new pairs.
5. If you obtain the **empty clause** `$\Box$` (a contradiction), the query **is entailed**.
6. If no further new clauses can be produced and there is no contradiction, the query **is not entailed** (for propositional resolution).

**Resolution example:**

Given `$A\lor B$`, `$A\Rightarrow C$`, and `$B\Rightarrow C$`, prove `$C$`.

| Step | Clause | Why? |
|---|---|---|
| 1 | `$A\lor B$` | Given |
| 2 | `$\neg A\lor C$` | Convert `$A\Rightarrow C$` |
| 3 | `$\neg B\lor C$` | Convert `$B\Rightarrow C$` |
| 4 | `$\neg C$` | Negate the goal C |
| 5 | `$B\lor C$` | Resolve steps 1 and 2 |
| 6 | `$\neg A$` | Resolve steps 2 and 4 (extra useful clause) |
| 7 | `$\neg B$` | Resolve steps 3 and 4 |
| 8 | `$C$` | Resolve steps 5 and 7 |
| 9 | `$\Box$` | Resolve C (step 8) with ¬C (step 4) |

**Conclusion:** `$KB\models C$`. Assuming `$\neg C$` caused a contradiction.

## 6. Strengths and Limits of Propositional Logic

### 6.1 Advantages

- **Declarative:** We write **what is true**, not a full procedure for using it.
- Supports **partial, OR, and negated information**.
- **Compositional:** The meaning of `$P\land Q$` comes from the meanings of P, Q and AND.
- **Context-independent:** A proposition has the same meaning regardless of the surrounding situation (unlike many natural-language sentences).

### 6.2 Disadvantage

**Limited expressive power:** PL cannot easily express general rules about **objects and their relationships**.

Example: To express *a pit causes a breeze in adjacent squares*, PL may need a separate sentence for each square. **First-Order Logic** can express general relationships using variables.

## 7. First-Order Logic (FOL)

### 7.1 How FOL Extends Propositional Logic

**First-Order Logic (FOL)** represents more than complete true/false facts. It can describe:

- **Objects:** People, houses, numbers, colours, etc.
- **Relations:** `Brother(x,y)`, `BiggerThan(x,y)`, `King(x)`.
- **Functions:** `FatherOf(x)`, `LeftLegOf(x)`, `Sqrt(x)`; a function gives an object/value for its input.

**Simple meaning:** PL says something **is true**; FOL can say **who/what it is true about**.

### 7.2 What Different Logics Describe

**Ontological commitment** = what kinds of things the language assumes exist.

**Epistemological commitment** = what kind of truth or belief status the language uses.

| System | What it represents | Truth / belief description in the lecture |
|---|---|---|
| **Propositional Logic** | Facts | True / false / unknown |
| **First-Order Logic** | Facts, objects, relations | True / false / unknown |
| **Temporal Logic** | Facts, objects, relations, **times** | True / false / unknown |
| **Probability Theory** | Facts | **Degree of belief** (how strongly something is believed) |
| **Fuzzy Logic** | Facts with **degrees of truth** | Value within a known interval |

### 7.3 Basic Syntax of FOL

| Element | Example | Meaning |
|---|---|---|
| **Constant** | `KingJohn`, `2` | Names a particular object |
| **Predicate** | `Brother`, `>` | Describes a property or relation |
| **Function** | `Sqrt`, `LeftLegOf` | Returns a value/object |
| **Variable** | `x`, `y` | Stands for an object |
| **Connectives** | `$\land,\lor,\neg,\Rightarrow,\Leftrightarrow$` | Combine statements |
| **Equality** | `$=$` | Two terms refer to the same object |
| **Quantifiers** | `$\forall,\exists$` | All objects / at least one object |

**Term:** A constant, variable, or function applied to terms. Example: `LeftLegOf(Richard)`.

**Atomic sentence:** A single predicate with terms or an equality, such as `Brother(KingJohn, Richard)` or `$x=y$`.

**Complex sentence:** Atomic sentences joined using connectives, for example:

$$Sibling(KingJohn,Richard)\Rightarrow Sibling(Richard,KingJohn)$$

### 7.4 Truth, Models and Interpretation in FOL

A **FOL model** contains at least one object and relationships among objects.

An **interpretation** decides what each symbol refers to:

- **Constant symbol → object**.
- **Predicate symbol → property/relation**.
- **Function symbol → function on objects**.

An atomic predicate is true when its objects really belong to the named relation in that model.

Example: If `Richard` refers to Richard the Lionheart, `John` to King John, and they have the brother relation, then `Brother(Richard,John)` is true.

The lecture's picture has **5 objects**, **2 binary relations** (`brother`, `on-head`), **3 unary relations** (`person`, `king`, `crown`) and **1 unary function** (`left-leg`).

- **Unary:** Involves one object.
- **Binary:** Involves two objects.

### 7.5 Why FOL Model Checking Is Harder

PL can enumerate a finite set of truth assignments for its proposition symbols.

FOL may have many possible **domain sizes** (number of objects), relations, and ways constants refer to objects. Domain size may grow from `$1$` without a fixed upper limit.

**Simple meaning:** Trying every possible FOL model is much harder than checking a PL truth table.

### 7.6 Universal Quantification (`∀`)

**Universal quantifier** `$\forall$` means **for all / every**.

Example: Everyone at Berkeley is smart.

$$\forall x\,[At(x,Berkeley)\Rightarrow Smart(x)]$$

- `$x$`: An object/person being considered.
- `$\forall x$`: Check **every** possible x.
- The implication says: **if** x is at Berkeley, **then** x is smart.

It works like an **AND** of the statement checked for every object.

★ Remember: Universal statements often use **implication (`⇒`)** to express rules about a group.

### 7.7 Existential Quantification (`∃`)

**Existential quantifier** `$\exists$` means **there exists at least one**.

Example: Someone at Stanford is smart.

$$\exists x\,[At(x,Stanford)\land Smart(x)]$$

This is true if **at least one** object/person is both at Stanford **and** smart.

It works like an **OR** of the statement checked for possible objects.

★ Remember: Existential statements often use **AND (`∧`)** to say one object satisfies several conditions.

### 7.8 Order and Properties of Quantifiers

**Quantifiers of the same kind can swap order:**

$$\forall x\,\forall y\,P(x,y)\equiv\forall y\,\forall x\,P(x,y)$$

$$\exists x\,\exists y\,P(x,y)\equiv\exists y\,\exists x\,P(x,y)$$

**Mixed quantifiers usually cannot swap:**

- `$\exists x\,\forall y\,Loves(x,y)$`: **One particular person** loves everyone.
- `$\forall y\,\exists x\,Loves(x,y)$`: **Every person** is loved by at least one person (not necessarily the same one).

These mean different things.

**Quantifier duality:** Each quantifier can be expressed using the other and NOT.

$$\forall x\,P(x)\equiv\neg\exists x\,\neg P(x)$$

$$\exists x\,P(x)\equiv\neg\forall x\,\neg P(x)$$

Examples:

- **Everyone likes ice cream** = **There is nobody who does not like ice cream**.
- **Someone likes broccoli** = **It is not true that everyone dislikes broccoli**.

★ Exam: **Quantifier order matters when `∀` and `∃` are mixed.**

### 7.9 Writing Relationships as FOL Sentences

**Brothers are siblings:**

$$\forall x,y\,[Brother(x,y)\Rightarrow Sibling(x,y)]$$

**Sibling is symmetric** (the relationship works both ways):

$$\forall x,y\,[Sibling(x,y)\Leftrightarrow Sibling(y,x)]$$

**A mother is a female parent:**

$$\forall x,y\,[Mother(x,y)\Leftrightarrow(Female(x)\land Parent(x,y))]$$

Here `Mother(x,y)` means **x is the mother of y**.

**First cousin:** x and y are first cousins if a parent `$p$` of x has a sibling `$ps$` who is a parent of y.

$$\begin{aligned}
\forall x,y\,[FirstCousin(x,y)\Leftrightarrow\exists p,ps\,(&Parent(p,x)\\
&\land Sibling(ps,p)\\
&\land Parent(ps,y))]
\end{aligned}$$

### 7.10 Equality in FOL

**Equality** `$t_1=t_2$` is true if both terms refer to **the same object**.

Example: `$2=2$` is valid with the usual identity interpretation.

**Full-sibling relationship** in the lecture: x and y are different people who share **two distinct parents**.

$$\begin{aligned}
\forall x,y\,[Sibling(x,y)\Leftrightarrow (&\neg(x=y)\land\exists m,f\,[\neg(m=f)\\
&\land Parent(m,x)\land Parent(f,x)\\
&\land Parent(m,y)\land Parent(f,y)])]
\end{aligned}$$

**Important slide clarification:** The lecture asks why even `$1=2$` can be **satisfiable** in FOL. That only makes sense when `1` and `2` are treated as **uninterpreted constant names**, which could name the same object. Under the **usual arithmetic meaning** of the numbers, `$1=2$` is false. The slide also lists `$\forall x\,[Sqrt(x)\times Sqrt(x)=x]$` as satisfiable; whether it holds depends on the chosen domain and interpretation.

## 8. Logic Programming

### 8.1 Computation as Logical Inference

**Logic programming** means solving a problem by writing knowledge and asking the system to infer answers.

| Logic programming | Ordinary programming |
|---|---|
| Identify the problem. | Identify the problem. |
| Gather information. | Gather information. |
| Encode knowledge in a **KB**. | Work out and program the **solution steps**. |
| Enter problem facts. | Enter input data. |
| **Ask queries** and let inference find answers. | Run the program on the data. |
| Debug incorrect or missing facts/rules. | Debug procedural/programming errors. |

**Simple meaning:** In logic programming, you mainly say **what is true** and **what you want to know**. In ordinary programming, you write more of **how to calculate it**.

## 9. Prolog Systems

### 9.1 Prolog and Its Rules

**Prolog** is a logic programming language based mainly on **backward chaining with Horn clauses**.

General rule form:

```prolog
head :- condition1, condition2, ... .
```

- `head`: The conclusion to prove.
- `:-`: Read as **IF**.
- Commas `,`: Mean **AND** between conditions.
- `.`: Ends the rule.
- A **fact** has no conditions, for example `person(joe).`

Lecture example:

```prolog
trades(X) :- person(X), item(Y), sells(X,Y,Z), has_relation(X,Z).
```

**Meaning:** `trades(X)` succeeds if the listed conditions can all be satisfied with matching values for variables.

### 9.2 How Prolog Finds Answers

- **Unification:** Find matching values for variables so terms agree. Example: match `person(X)` with `person(joe)` to get `X = joe`.
- **Backward chaining:** Start from a query and search for facts/rules that can prove it.
- **Depth-first, left-to-right search:** Try one proof path deeply, with rule conditions checked from left to right.
- **Backtracking:** If a choice fails, return and try another matching choice.
- **Efficient clause retrieval:** Find potentially matching rules without scanning every rule each time.
- **Built-in predicates:** Perform operations such as arithmetic; example `X is Y*Z+3` evaluates the expression on the right.

**Closed-world assumption / negation as failure:** If Prolog cannot prove something from its available rules and facts, `not` may succeed for that goal.

```prolog
alive(X) :- not dead(X).
```

For query `alive(joe)`, this rule succeeds if `dead(joe)` **cannot be proved**.

★ Exam: **Failure to prove `dead(joe)` is not the same as logically proving that Joe is alive.** This is the special *negation as failure* behaviour.

### 9.3 Prolog Example: Depth-First Search

```prolog
dfs(X) :- goal(X).
dfs(X) :- successor(X,S), dfs(S).
```

1. First, test whether X is already a **goal**.
2. Otherwise, find a **successor** S of X.
3. Recursively run `dfs(S)`.
4. If a path fails, Prolog can backtrack to another successor.

`successor(X,S)` can produce different values of S through backtracking; no explicit loop over successors is needed in this example.

### 9.4 Prolog Example: Appending Lists

```prolog
append([], Y, Y).
append([X|L], Y, [X|Z]) :- append(L, Y, Z).
```

- `[]`: Empty list.
- `[X|L]`: A list with first item X and remaining list L.
- First rule: Appending an empty list to Y gives Y.
- Second rule: Keep the first item X, then append the remaining list L to Y.

For the query:

```prolog
?- append(A, B, [1,2]).
```

Prolog can find these answers:

| A | B |
|---|---|
| `[]` | `[1,2]` |
| `[1]` | `[2]` |
| `[1,2]` | `[]` |

**Simple meaning:** Prolog can use the same rule not only to **join lists**, but also to **work backwards and find ways to split a list**.

★ Remember: **Prolog = Horn clauses + backward chaining + unification + depth-first search + backtracking.**
