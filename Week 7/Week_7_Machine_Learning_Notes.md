# Applied AI — Week 07: Machine Learning Fundamentals
## Night-before-exam notes | Learn by application and worked examples

**Source:** *7 Applied AI – Machine Learning.pdf*, Dr. Hamed Hamzeh, lecture Week 07 (slides 1–54; acknowledgement on PDF page 55).  
**Exam style:** Multiple-choice questions (MCQs).  
**How to use this note:** First read the **15-minute essentials**, then work through the examples **without looking at the answers**, then take the mini-mock at the end.

> **Scope:** All definitions, methods, and named algorithms are drawn from the Week 07 lecture. Small-number worked examples and practice MCQs are created for revision, not copied from the lecture. Relevant lecture-slide numbers are shown so you can check the original.

---

# PART A — 15-MINUTE ESSENTIALS (READ FIRST)

| If an MCQ says… | Think… | Key idea/formula |
|---|---|---|
| Labels/known correct outputs | **Supervised learning** | Classification or regression |
| No labels; discover groups | **Unsupervised learning** | K-means clustering |
| Actions, rewards, environment | **Reinforcement learning** | Agent learns a policy |
| Predict a category | **Classification** | Species, spam/ham |
| Predict a number | **Regression** | Petal width, house price |
| Feature/input/independent variable | **X** | Iris: sepal/petal measurements |
| Label/target/dependent variable | **y** | Iris species for classification |
| Fit scaler on entire dataset | **Data leakage** | Fit **only on training**; transform validation/test |
| Balanced proportions across sets | **Stratified split** | Preserve class ratios |
| Standardisation | **z-score** | `z = (x − mean) / standard deviation` |
| Min–max normalisation | **0 to 1** | `(x − min) / (max − min)` |
| Straight-line prediction | **Linear regression** | `ŷ = β₀ + β₁x` |
| Actual − predicted | **Residual** | `e = y − ŷ` |
| Mean of squared errors | **MSE** | `Σ(y − ŷ)² / n` |
| Mean absolute error | **MAE** | `Σ|y − ŷ| / n` |
| Root of MSE | **RMSE** | `√MSE` |
| Explained variation | **R²** | `1 − SS_res / SS_tot` |
| Gradient descent update | **Step opposite gradient** | `β ← β − α × gradient` |
| Too large learning rate | **Overshoot/diverge** | Loss may rise/explode |
| Node contains one class | **Pure; Gini = 0** | No need to split |
| Node contains mixed classes | **Higher Gini** | `1 − Σp²` |
| Compare candidate tree splits | **Lowest weighted Gini** | Equivalently highest Gini gain for same parent |
| `min_samples_split` | **Check parent node size** | Is parent large enough to split? |
| `min_samples_leaf` | **Check every child leaf size** | Each side must be big enough |
| `max_depth` | **Limit tree depth** | Controls complexity/overfitting |
| K = 3 | **3 clusters** | Not 3 points per cluster |
| Assign a K-means point | **Nearest centroid** | Euclidean distance |
| Update a centroid | **Mean of assigned points** | Average each coordinate independently |
| K-means objective | **Minimise WCSS/inertia** | Sum of **squared** distances |
| K selection | **Elbow method** | Bend where WCSS reduction slows |
| RL exploration | **Try new actions** | ε-greedy explores with probability ε |
| RL exploitation | **Use known good action** | Choose current best option |
| Rows of confusion matrix | **Actual** | Columns = predicted |
| Precision | **How trustworthy are positive predictions?** | `TP/(TP+FP)` |
| Recall | **How many real positives did we catch?** | `TP/(TP+FN)` |
| F1 | **Balance precision and recall** | `2PR/(P+R)` |
| Avoid false positives | **Prioritise precision** | E.g. don't hide legitimate email as spam |
| Avoid false negatives | **Prioritise recall** | E.g. don't miss disease in screening |

**One crucial habit:** In a calculation MCQ, write the formula, substitute the numbers, and only then look at the options. Never guess from the option wording.

---

# PART B — COMPLETE LECTURE NOTES WITH APPLICATIONS

## 1. What machine learning actually does (slides 4–10)

### Traditional programming vs machine learning

- **Traditional programming:** A human writes the rules. Example: `IF petal_length < 2.5 THEN species = Setosa`.
- **Machine learning:** Give the algorithm example flowers and their known species. It **learns** useful prediction rules from the examples.

### Tom Mitchell's T–E–P definition (slide 5)

A system learns if its performance **P** at a **task T** improves with **experience E**.

**Apply it to Iris:**

- **T (task):** Predict a flower's species.
- **E (experience):** 150 labelled Iris observations.
- **P (performance):** Accuracy on unseen flowers.

**MCQ trap:** The dataset is **E**, not **P**. Accuracy is **P**, not **T**.

### Know the Iris dataset (slides 6–8)

- **150 observations**, **4 numerical features**, **3 classes**, **50 flowers/class**.
- Four measurements (cm): **sepal length, sepal width, petal length, petal width**.
- Three species: **Setosa, Versicolor, Virginica**.
- The lecture reports **0 missing values**; its preprocessing example detects **1 duplicated row**.
- **One row** = one flower; **one feature column** = one measurable property.
- **X** = inputs (independent variables); **y** = target/label (dependent variable).
- Same dataset can be framed differently: species as **y** means *classification*; petal width as **y** means *regression*.

**Application:** You are given sepal length 5.1, sepal width 3.5, petal length 1.4, petal width 0.2. These are **X**. The known answer Setosa is **y**.

### Three families of ML (slides 9–10)

| Family | Learns from | Goal | Lecture example |
|---|---|---|---|
| **Supervised** | Features **and labels** `(X, y)` | Predict an output | Iris species, petal width, spam |
| **Unsupervised** | Features **without labels** `X` | Find hidden structure | K-means groups flowers |
| **Reinforcement** | States, actions, rewards | Find actions maximising reward | Games, robot navigation, traffic lights |

**Recognition MCQ:** "A program groups customers, but no customer-group labels are supplied." → **Unsupervised**.

---

## 2. ML pipeline, cleaning and leakage (slides 12–19)

### Pipeline in the correct order (slide 12)

1. **Define** the problem.
2. **Collect** data.
3. **Explore** (EDA): look at distributions, plots, patterns, unusual points.
4. **Clean**: missing values, duplicates, outliers, encoding.
5. **Engineer features**: create/transform/select inputs.
6. **Split and scale**: keep train, validation, test separate and fit preprocessing correctly.
7. **Train and tune** models.
8. **Evaluate and deploy**; monitor afterward.

**Real example:** To predict Iris species, inspect petal-length/petal-width plots first. Setosa separates well, while Versicolor and Virginica overlap (slide 13). This warns you **where a classifier may make mistakes**.

### Data cleaning (slide 14)

- **Missing values:** Drop affected rows or fill using **mean, median, or mode** (imputation).
- **Duplicates:** Remove/handle repeated records; duplicates can bias training or leak between sets.
- **Outliers:** Detect with **IQR rule** or **z-scores**; investigate, don't automatically delete.
- **Encoding:** Convert text categories to numbers, e.g. label encoding or one-hot encoding.

**IQR formula:**

```text
IQR = Q3 − Q1
Lower fence = Q1 − 1.5 × IQR
Upper fence = Q3 + 1.5 × IQR
Outlier if x < lower fence or x > upper fence
```

**Mini example (illustrative):** Q1 = 4, Q3 = 8 → IQR = 4. Bounds: `4 − 6 = −2`, `8 + 6 = 14`. A value of 16 is flagged; 12 is not.

**MCQ trap:** Detecting an outlier is **not the same** as proving it is an error.

### Train / validation / test (slide 15)

The lecture uses a **stratified 60/20/20** split of **150 flowers**:

| Set | Flowers | Purpose |
|---|---:|---|
| Training | **90** | Learn model parameters (e.g., slope, tree splits) |
| Validation | **30** | Compare models / tune hyperparameters (e.g., `max_depth`) |
| Test | **30** | Final unseen evaluation, kept untouched until the end |

**Stratified** = preserve class proportions in each split. With three equally represented Iris classes, each 30-flower set would ideally contain 10 from each species.

**MCQ traps:**

- Choosing the best `max_depth` by repeatedly checking **test** accuracy misuses the test set.
- **Validation** tunes; **test** is the final check.
- A split like `train_test_split(X, y, stratify=y, random_state=42)` uses `stratify` to maintain class proportions; `random_state` supports reproducibility.

### Feature engineering (slide 16)

- **Create:** `petal_area = petal_length × petal_width`.
- **Transform:** log, square root, polynomial terms.
- **Encode:** categories → numeric features.
- **Select:** retain informative features, discard noisy ones.

**Example:** Petal length 4 cm and width 1.5 cm → petal area = **6 cm²**.

### Why scaling matters (slides 17–19)

When inputs have different ranges, **distance-based** algorithms can let large-scale features dominate. Scaling also helps gradient-based training.

**Lecture's algorithm lists:**

| Scaling generally needed/helpful | Scaling generally not needed |
|---|---|
| KNN, **K-means**, SVM, gradient descent, neural networks, PCA | Decision trees, random forests, gradient-boosted trees, Naive Bayes |

**Standardisation (StandardScaler):**

```text
z = (x − μ) / σ
```

- `μ` = mean, `σ` = standard deviation.
- Resulting training feature has mean around 0, SD around 1; **not** confined to [0,1].
- **Apply:** x = 12, mean = 10, SD = 2 → z = `(12−10)/2 = 1`.

**Min–max normalisation (MinMaxScaler):**

```text
x' = (x − min) / (max − min)
```

- Maps the fitted range to **[0,1]**; sensitive to extreme values.
- **Apply:** x = 6, min = 2, max = 10 → `(6−2)/(10−2) = 0.5`.

**The most important leakage rule (slide 19):**

```python
scaler.fit_transform(X_train)  # learn mean/SD from TRAIN, then scale train
scaler.transform(X_val)        # only transform validation
scaler.transform(X_test)       # only transform test
```

Never compute scaling parameters from train+test together. **That leaks information about unseen data** into training. A scikit-learn `Pipeline` helps prevent this mistake.

**MCQ trap:** `fit_transform(test)` is **wrong** after fitting on training. Use `transform(test)`.

---

## 3. Supervised problems and linear regression (slides 21–27)

### Classification vs regression (slide 21)

- **Classification:** The answer is a **category** (Setosa/Versicolor/Virginica, spam/ham).
- **Regression:** The answer is a **continuous number** (petal width = 1.5 cm; temperature = 17.8 °C).

### Linear regression equation (slides 22–23)

```text
ŷ = β₀ + β₁x
```

- `ŷ` = predicted numerical value.
- `x` = input feature.
- `β₀` = intercept (prediction at x = 0).
- `β₁` = slope (change in prediction per +1 unit of x).
- Multiple features: `ŷ = β₀ + β₁x₁ + ... + βₙxₙ`.

**Lecture's fitted Iris line:**

```text
predicted petal width = −0.357 + 0.413 × petal length
```

**Worked question:** For petal length **4.5 cm**, predict width:

```text
ŷ = −0.357 + 0.413(4.5)
  = −0.357 + 1.8585
  = 1.5015 ≈ 1.50 cm
```

**Interpretation:** +1 cm of petal length corresponds to about **+0.413 cm predicted petal width**. The intercept can be outside a physically meaningful range yet still be needed to position the line.

### Residual: actual minus predicted (slide 24)

```text
Residual e = y − ŷ
```

- **Positive residual:** actual > predicted → **underprediction**.
- **Negative residual:** actual < predicted → **overprediction**.
- **Zero residual:** exact prediction.

**Apply:** Actual 2.1, predicted 1.8 → `e = +0.3` → model **underpredicted**.

**MCQ trap:** Reverse the subtraction and you reverse the sign. The lecture defines **actual minus predicted**.

### Mean squared error: MSE (slide 25)

```text
MSE = [Σ(yᵢ − ŷᵢ)²] / n
```

- Square each residual so positive and negative errors don't cancel.
- Large mistakes count disproportionately: an error of 2 contributes **4**.
- Smaller **MSE** is better.

**Worked example:** Actual `[2, 4, 6]`, predicted `[3, 4, 4]`:

| Case | y | ŷ | Residual y−ŷ | Squared |
|---|---:|---:|---:|---:|
| 1 | 2 | 3 | −1 | 1 |
| 2 | 4 | 4 | 0 | 0 |
| 3 | 6 | 4 | 2 | 4 |

`MSE = (1 + 0 + 4) / 3 = 5/3 ≈ 1.667`.

### Ordinary least squares (OLS): finding the best line (slides 26–27)

OLS finds the **slope and intercept that minimise squared errors**.

```text
β₁ = Σ[(xᵢ − x̄)(yᵢ − ȳ)] / Σ[(xᵢ − x̄)²]
β₀ = ȳ − β₁x̄
```

- `x̄` = mean of x; `ȳ` = mean of y.
- **Slope:** how x and y vary together, relative to variation in x.
- **Intercept:** adjust the line through `(x̄, ȳ)`.

**Lecture's 5-flower calculation (slide 27):**

```text
mean x = 3.88, mean y = 1.34
Σ[(x − mean x)(y − mean y)] = 5.604
Σ[(x − mean x)²]          = 10.748
slope β₁ = 5.604 / 10.748 = 0.521
intercept β₀ = 1.34 − 0.521(3.88) = −0.681
fitted line: ŷ = −0.681 + 0.521x
```

**Simpler made-up practice:** Points `(1,2), (2,4), (3,6)`. Means `x̄=2, ȳ=4`. Numerator `(-1)(-2)+0+1(2)=4`; denominator `1+0+1=2`. So `β₁=2`, `β₀=4−2×2=0`; prediction is `ŷ=2x`.

**Useful code recognition (slide 32):**

```python
model = LinearRegression()
model.fit(x_train, y_train)    # learns parameters
model.coef_[0]                 # slope
model.intercept_               # intercept
model.predict(x_test)          # predictions
# X[:, [2]] is 2-dimensional (n,1); X[:, 2] is 1-dimensional
```

---

## 4. Gradient descent and regression evaluation (slides 28–33)

### Gradient descent: walking downhill on loss (slide 28)

1. Start with initial parameters (for example `β₀ = β₁ = 0`).
2. Compute **gradient** (direction of fastest *increase* in loss).
3. Move a step in the **opposite direction**.
4. Repeat until loss stops decreasing (**convergence**).

**Update rule (slide 29):**

```text
new parameter = old parameter − learning_rate × gradient
β ← β − α ∂J/∂β
```

For linear regression MSE:

```text
∂J/∂β₀ = (2/n) Σ(ŷᵢ − yᵢ)
∂J/∂β₁ = (2/n) Σ[(ŷᵢ − yᵢ)xᵢ]
```

**Tiny worked update:** `β₁ = 2`, gradient = `+3`, learning rate `α = 0.1`:

```text
new β₁ = 2 − 0.1(3) = 1.7
```

If the gradient is negative, subtracting it **increases** the parameter; this still moves downhill.

**Learning-rate outcomes (slide 31):**

| Learning rate | What happens |
|---|---|
| Too small (`α = 0.002` in lecture) | Slow convergence |
| Suitable (`α = 0.02`) | Fast, steady loss reduction |
| Too large (`α = 0.07`) | Overshoots; loss may explode |

Gradient descent eventually approaches the same Iris regression solution as OLS (`β₀ ≈ −0.357`, `β₁ ≈ 0.413`; slides 29–30).

**MCQ trap:** A larger learning rate is **not always better**.

### Regression metrics (slide 33)

| Metric | Formula | What it tells you |
|---|---|---|
| **MAE** | `Σ|y−ŷ| / n` | Mean **absolute** error, in original units |
| **MSE** | `Σ(y−ŷ)² / n` | Mean **squared** error, punishes large misses |
| **RMSE** | `√MSE` | Error in original units, punishes big misses |
| **R²** | `1 − SS_res/SS_tot` | Fraction of target variation explained relative to mean baseline |

**Apply to earlier residuals `[−1,0,+2]`:**

```text
MAE  = (1+0+2)/3 = 1.0
MSE  = (1+0+4)/3 ≈ 1.667
RMSE = √1.667 ≈ 1.291
```

**R² interpretation from lecture:** R² = **0.928** means roughly **92.8% of variation** in test petal width is explained by the fitted line. `R² = 1` perfect; `R² = 0` no better than predicting the mean (as stated on the slide).

**Lecture's 30-test-flower results:** MAE **0.168 cm**, MSE **0.046**, RMSE **0.214 cm**, R² **0.928**.

**MCQ traps:**

- **MAE is not squared**; **RMSE is square-root of MSE**.
- MSE and RMSE must not be confused.
- A lower **error metric** generally means better predictions; a higher **R²** generally means better explanatory fit.

---

## 5. Decision trees and Gini (slides 34–40) — HIGH-PRIORITY CALCULATIONS

### A tree is a learned flowchart (slide 35)

- **Root node:** first question, contains all training data.
- **Internal node:** subsequent test of a feature/threshold.
- **Branch:** answer to that test (yes/no).
- **Leaf:** final prediction, usually **majority class**.
- A tree tests candidate features and thresholds, picks a favourable split, and repeats.

**Read this Iris tree:**

```text
Petal length ≤ 2.45?
├── YES  → Setosa
└── NO   → Petal width ≤ 1.75?
             ├── YES → Versicolor
             └── NO  → Virginica
```

**Application:** Petal length = 4.0, width = 1.2:

- `4.0 ≤ 2.45`? **No**.
- `1.2 ≤ 1.75`? **Yes**.
- Prediction = **Versicolor**.

### Gini impurity of one node (slide 36)

```text
Gini(node) = 1 − Σ(class proportion)²
           = 1 − (p₁² + p₂² + ...)
```

- `p` = number in a class / **total at that node**.
- **Gini = 0**: pure (one class only).
- Higher Gini: more mixed classes.
- **Two equal classes:** `1−(0.5²+0.5²)=0.5`.
- **Three equal classes:** `1−3(1/3)²=2/3≈0.667`.

**Worked example 1 (the practice you solved):** 6 Setosa, 4 Versicolor:

```text
total = 10
p1 = 6/10 = 0.6
p2 = 4/10 = 0.4
Gini = 1 − (0.6²+0.4²)
     = 1 − (0.36+0.16)
     = 0.48
```

**Worked example 2 (3 classes):** 2, 2, 2 in a six-flower node:

`Gini = 1 − [(2/6)²+(2/6)²+(2/6)²] = 1−3/9 = 0.667`.

**MCQ trap:** 0.5 is the **maximum for two classes**, but a three-class Iris root can have **0.667**.

### Weighted Gini: judging a proposed split (slide 37)

```text
Weighted Gini = (n_left/n_parent) × Gini_left
              + (n_right/n_parent) × Gini_right
```

**Why weighting?** A child with more flowers should influence the total more strongly.

**Worked example (your earlier practice):** Parent 20 flowers, left 5 flowers Gini 0, right 15 flowers Gini 0.4:

```text
Weighted Gini = (5/20)×0 + (15/20)×0.4
              = 0 + 0.75×0.4
              = 0.30
```

**Do not** simply average `0` and `0.4` to get `0.2` unless the child sizes are equal.

### Gini gain: how much impurity is removed? (slide 37)

```text
Gini Gain = Parent Gini − Weighted Gini
```

**Example:** Parent Gini = 0.60; after splitting weighted Gini = 0.25:

`Gain = 0.60−0.25 = 0.35`.

**Best-split rule:** For candidates from the **same parent**, choose **lowest weighted Gini** = **highest Gini gain**.

**The actual lecture comparison (slide 37):**

| Candidate split | Weighted Gini | Gini gain | Better? |
|---|---:|---:|---|
| **A: petal length ≤ 2.45** | **0.333** | **≈0.333** | **YES** |
| B: sepal length ≤ 5.45 | 0.439 | 0.228 | No |

For split A: 50 flowers at Gini 0; 100 flowers at Gini 0.5:

`(50/150)×0 + (100/150)×0.5 = 0.333`.

**Tie case (slide 38):** Petal length `≤2.45` and petal width `≤0.80` both achieve weighted Gini **0.333**, so **either** may be selected.

**MCQ trap:** A tie does **not** mean the tree cannot split.

### How to read the trained tree (slide 39)

Every node can display:

- A split rule; **Gini**; number of samples **n**.
- The terminal leaf's predicted **majority class**.
- Slide 39's depth-3 tree has **97.3% training accuracy** (4 of 150 misclassified). Training accuracy alone does **not** establish test performance.

### Overfitting and stopping rules (slide 40)

A tree that splits too deeply may memorise training examples and generalise worse to unseen examples.

| Hyperparameter | Check this | Example |
|---|---|---|
| `max_depth` | Maximum **depth** of tree (root is depth 0) | `max_depth=2` means no split beyond depth 2 |
| `min_samples_split` | Minimum samples in the **parent node** to be allowed to split | Threshold 8; parent has 7 → cannot split |
| `min_samples_leaf` | Minimum samples required in **each resulting leaf** | Threshold 3; split 2+8 → invalid |
| `criterion` | Criterion used to score splits | `"gini"` (default) or `"entropy"` (slide 40) |

**Two checks in order (very common MCQ):**

Suppose `min_samples_split=8`, `min_samples_leaf=3`, parent has 10 flowers.

1. Parent check: `10 ≥ 8` → **eligible to split**.
2. Proposed split **2 + 8** → **invalid**, because 2 < 3.
3. Proposed split **4 + 6** → **allowed** by these sample-count rules.

Another: `min_samples_split=6`, `min_samples_leaf=2`, parent 8; proposed `2+6` → **allowed**. Passing checks does not guarantee this is the split selected; impurity and other constraints still matter.

**Lecture's overfitting example (slide 40):** At depth **5**, training accuracy reaches **100%** but test accuracy drops to **0.933**. Depth **3** gives the best shown test score **0.967**. **Higher training accuracy does not always mean a better model.**

---

## 6. Unsupervised learning and K-means (slides 41–46) — HIGH-PRIORITY CALCULATIONS

### What unsupervised learning means (slide 42)

The algorithm receives **X without species labels y** and must find a pattern by itself.

Lecture tasks:

- **Clustering:** group similar samples. Examples: K-means, hierarchical, DBSCAN.
- **Dimensionality reduction:** compress features. Examples: PCA, t-SNE.
- **Anomaly detection:** identify unusual observations.

**MCQ trap:** K-means is **not a supervised classifier**, even if researchers later compare its clusters with real Iris species.

### K-means algorithm: four steps (slide 43)

1. **Choose K** = number of clusters.
2. **Initialise K centroids** (often at randomly chosen observations).
3. **Assign each point** to the nearest centroid using Euclidean distance.
4. **Update each centroid** to the mean of points assigned to it.
5. Repeat **assign → update** until centroid positions stabilise (**convergence**).

**K = 3 means three clusters**, not three data points in a cluster.

### Euclidean distance (slide 43)

```text
d(x, μ) = √Σ(x_j − μ_j)²
For 2D points: d = √[(x₂−x₁)² + (y₂−y₁)²]
```

**Worked assignment:** Point P = (4, 2), centroid A = (1, 2), centroid B = (6, 2):

```text
d(P,A) = √[(4−1)²+(2−2)²] = √9 = 3
d(P,B) = √[(4−6)²+(2−2)²] = √4 = 2
```

Assign P to **B**, because **2 < 3**.

**More typical 2D example:** P `(2,3)` vs centroid `(5,7)`:

`√[(2−5)² + (3−7)²] = √(9+16) = 5`.

### Updating a centroid (slide 43)

Take the **mean of each coordinate separately**.

**Worked example:** Cluster points `(2,4)` and `(6,8)`:

```text
new X = (2+6)/2 = 4
new Y = (4+8)/2 = 6
new centroid = (4,6)
```

**MCQ trap:** Centroid may move; the **original points don't physically move**, though their cluster assignments may change next round.

### Convergence (slides 43–44)

K-means alternates point assignment and centroid update. It converges when centroids stop changing (the lecture's displayed Iris example reaches a stable assignment after **8 iterations**).

### WCSS / inertia: the objective to minimise (slide 43)

```text
WCSS = Σ over clusters [Σ over assigned points (distance to centroid)²]
```

- **Low WCSS:** points near their centroids (tighter clusters).
- **High WCSS:** points more dispersed from centroids.
- **Never divide by number of points** when calculating WCSS; it is a **sum**, unlike MSE.

**Worked example:** Cluster points `[2,4,6]`, centroid = 4:

```text
WCSS = (2−4)² + (4−4)² + (6−4)²
     = 4 + 0 + 4
     = 8
```

**Two-cluster example:** Cluster A points `[1,3]`, centroid 2 → WCSS A = `1+1=2`; Cluster B `[7,9]`, centroid 8 → WCSS B = `1+1=2`; total **WCSS = 4**.

### Elbow method: choosing K (slide 45)

- Fit K-means using several K values.
- Calculate WCSS for each.
- Plot **K on x-axis**, **inertia/WCSS on y-axis**.
- Increasing K typically decreases WCSS; **choose the bend where further reductions become relatively small**.

**Worked practice:**

| K | WCSS | Drop |
|---|---:|---:|
| 1 | 600 | — |
| 2 | 300 | 300 |
| 3 | 120 | 180 |
| 4 | 115 | 5 |
| 5 | 112 | 3 |

After **K=3**, further reductions are small. **Choose K=3**.

**Lecture's Iris elbow example:** WCSS about **681.4, 152.3, 78.9, 57.2** for K **1–4**, elbow identified at **K=3**.

**MCQ trap:** Don't always select the **largest K** just to minimise WCSS. The elbow is about diminishing returns.

### Did clusters equal the species? (slide 46)

K-means used **no species labels for training**. The lecture compares discovered clusters afterward:

- **134/150** flowers in matching clusters.
- **Adjusted Rand Index (ARI) = 0.73** (`1` is perfect).
- All **50 Setosa** flowers appear together; Versicolor and Virginica overlap, creating the remaining mistakes.

**MCQ trap:** Using true labels to **evaluate** clusters afterward does not mean labels were used **to train** K-means.

---

## 7. Reinforcement learning (slides 47–48)

### The interaction loop

```text
Agent observes state s
    → chooses action a
    → environment changes
    → receives reward r and new state s'
    → improves its policy π (strategy for choosing actions)
```

| Term | Meaning | Tiny application |
|---|---|---|
| **Agent** | Learner/decision-maker | Robot controller |
| **Environment** | World the agent interacts with | Maze |
| **State `s`** | Current situation | Robot at junction A |
| **Action `a`** | Choice it makes | Turn left |
| **Reward `r`** | Numerical feedback | +10 when it reaches the goal |
| **Policy `π`** | Mapping from states to actions | "At A, go left" |

**Goal:** Maximise **total reward over time**, not merely immediate accuracy on a labelled dataset.

### Explore vs exploit (slide 48)

- **Exploration:** Try a new action that might be better.
- **Exploitation:** Pick an action currently believed to be best.
- **ε-greedy:** Explore randomly with probability **ε**; otherwise exploit.

**MCQ example:** If ε = 0.2, the exploration probability is **20%** (not 80%).

### Q-learning update: recognise the pieces (slide 48)

```text
Q(s,a) ← Q(s,a) + α [ r + γ max_a' Q(s',a') − Q(s,a) ]
```

- `Q(s,a)` = expected total reward for action `a` in state `s`.
- `α` = learning rate.
- `γ` = discount factor for future reward.
- `r` = immediate reward.
- `max Q(s',a')` = estimated best future value in next state.

**Easy application:** Suppose `Q(s,a)=2`, `α=0.5`, `r=4`, `γ=0.5`, max next-state Q = 6:

```text
Qnew = 2 + 0.5[4 + 0.5(6) − 2]
     = 2 + 0.5[4 + 3 − 2]
     = 2 + 2.5 = 4.5
```

**MCQ trap:** `α` controls **how big the update is**; `γ` weights **future rewards**; `ε` controls **exploration probability**. These are three different symbols.

---

## 8. Evaluating classifiers (slides 49–53) — HIGH-PRIORITY CALCULATIONS

### Confusion matrix: read its direction (slide 50)

**Rows = ACTUAL, columns = PREDICTED.** Diagonal entries are correct predictions.

Lecture: KNN (`k=5`, scaled), **30 test flowers**:

| Actual \\ Predicted | Setosa | Versicolor | Virginica |
|---|---:|---:|---:|
| **Setosa** | **10** | 0 | 0 |
| **Versicolor** | 0 | **10** | 0 |
| **Virginica** | 0 | **2** | **8** |

- Correct = diagonal `10+10+8 = 28`.
- Wrong = `2` Virginica called Versicolor.
- Accuracy = `28/30 = 0.933 ≈ 93.3%`.

### TP, FP, FN, TN: pick ONE positive class (slide 51)

Treat **Versicolor as positive**, the other two species as negative:

| | Predicted Versicolor | Predicted other |
|---|---:|---:|
| **Actually Versicolor** | **TP = 10** | **FN = 0** |
| **Actually other** | **FP = 2** | **TN = 18** |

- **TP:** Real positive and predicted positive.
- **FP:** Actually negative, **wrongly predicted positive**.
- **FN:** Actually positive, **wrongly predicted negative**.
- **TN:** Actually negative and predicted negative.

**MCQ memory:** **FP = false alarm**. **FN = missed real positive**.

### Accuracy, precision, recall, F1 (slide 52)

```text
Accuracy  = (TP + TN) / (TP + TN + FP + FN)
Precision = TP / (TP + FP)
Recall    = TP / (TP + FN)
F1        = 2 × Precision × Recall / (Precision + Recall)
         = 2TP / (2TP + FP + FN)  [equivalent form]
```

**Use the lecture numbers:** TP=10, FP=2, FN=0, TN=18.

```text
Accuracy  = (10+18)/30 = 28/30 ≈ 0.933
Precision = 10/(10+2)  = 10/12 ≈ 0.833
Recall    = 10/(10+0)  = 10/10 = 1.000
F1        = 2(0.833)(1.0)/(0.833+1.0) ≈ 0.909
```

**How to tell precision vs recall apart:**

- **Precision question:** "Out of the things the model **predicted positive**, how many really were positive?" → TP/(TP+FP).
- **Recall question:** "Out of **all real positives**, how many did the model find?" → TP/(TP+FN).

### When is each metric most important? (slide 53)

| Situation | Choose | Why |
|---|---|---|
| Balanced classes, similar error costs | **Accuracy** | Overall fraction correct is informative |
| False positives costly | **Precision** | Avoid false alarms (spam filter hiding legitimate email) |
| False negatives costly | **Recall** | Avoid misses (disease screening) |
| Imbalanced classes, both precision and recall matter | **F1-score** | Balances two competing concerns |

**Example:** A medical screening system catches only 1 of 10 real cases but classifies 990 healthy patients correctly. Overall accuracy may look high, while **recall is terrible**. That's why accuracy alone can mislead.

### Classification report (slide 53)

- **Support** = number of true instances of each class (Iris test data: **10 each**).
- Each class has its **own** precision, recall, F1.
- **Macro average** = simple mean across the classes, treating each class equally.
- Lecture example: macro precision **0.944**, recall **0.933**, F1 **0.933**, accuracy **0.933**.

**MCQ trap:** When a matrix has **three classes**, select a specific positive class to calculate **TP/FP/FN/TN**. These values change depending on which class you consider positive.

---

# PART C — EXAM TECHNIQUE: HOW TO SOLVE COMMON QUESTION TYPES

## Decision-tree calculation checklist

**Question: "Find the best split."**

1. For each child, compute class proportions `class count / child total`.
2. Compute each child's Gini: `1 − Σp²`.
3. Weight by number of samples: `(nL/n)GiniL + (nR/n)GiniR`.
4. If asked, calculate gain: `parent Gini − weighted Gini`.
5. **Pick LOWEST weighted Gini (or HIGHEST gain).**
6. Check whether any stopping-rule constraints make a split invalid.

**Question: "Can a tree node split?"**

1. Parent count ≥ `min_samples_split`?
2. Resulting child counts each ≥ `min_samples_leaf`?
3. `max_depth` limit not exceeded? Node not already pure?
4. If all applicable checks pass, the split may be **permitted**, not necessarily preferred.

## K-means calculation checklist

**Question: "Which cluster gets point P?"**

1. Calculate distance from P to **each** centroid.
2. Pick **smallest** Euclidean distance.

**Question: "New centroid?"**

1. Average the X coordinates.
2. Average the Y coordinates (and any additional feature coordinates).
3. Write the mean coordinate vector.

**Question: "Find WCSS?"**

1. Take each point's distance from its **assigned** centroid.
2. **Square** each distance.
3. **Sum** over all points in all clusters. **Do not divide.**

**Question: "Best K by elbow?"**

1. Look at how much WCSS drops between consecutive K values.
2. Choose K after which improvements become much smaller.

## Classification-metric checklist

1. Write **positive class** at top of scratch paper.
2. Identify TP, FP, FN, TN using *actual rows / predicted columns*.
3. Precision denominator: **TP + FP** (predicted positives).
4. Recall denominator: **TP + FN** (real positives).
5. Accuracy: **all correct / all samples**.
6. F1: harmonic mean of precision and recall.

## Regression calculation checklist

1. Prediction `ŷ = β₀ + β₁x`.
2. Residual `y − ŷ`.
3. MAE: absolute errors then **divide by n**.
4. MSE: squared errors then **divide by n**.
5. RMSE: **square root of MSE**.
6. Gradient update: **subtract** `learning rate × gradient`.

---

# PART D — 24 MCQs TO ATTEMPT BEFORE THE EXAM

**Instructions:** Hide the answer key at the end. Write your choice and a one-line reason. All questions are limited to lecture topics. Computation values are illustrative unless stated otherwise.

### Foundations, pipeline and scaling

**Q1.** A model receives flower measurements **without** species labels and groups them. This is:  
A. Supervised classification · B. Unsupervised learning · C. Regression · D. Reinforcement learning

**Q2.** In an Iris species-classification task, the species name is:  
A. An independent variable · B. A feature · C. The dependent target `y` · D. A centroid

**Q3.** In a stratified 60/20/20 split of 150 flowers, how many are in the validation set?  
A. 20 · B. 30 · C. 60 · D. 90

**Q4.** You fitted a StandardScaler on `X_train`. How should you scale test data?  
A. `fit_transform(X_test)` · B. Fit on all data · C. `transform(X_test)` · D. Never scale test data

**Q5.** If x=8, mean=6, standard deviation=2, the standardised z-score is:  
A. 0.25 · B. 1 · C. 2 · D. 4

**Q6.** A value x=5 is scaled with min=1 and max=9. Min–max result?  
A. 0.25 · B. 0.5 · C. 1 · D. 4

### Linear regression, gradient descent and evaluation

**Q7.** For `ŷ = 1 + 2x`, x=3 gives:  
A. 5 · B. 6 · C. 7 · D. 9

**Q8.** Actual y=4, predicted ŷ=6. Residual according to the lecture is:  
A. +2 · B. −2 · C. 10 · D. 0

**Q9.** Actual `[1,3]`, predicted `[2,5]`. MSE is:  
A. 1.5 · B. 2.5 · C. 3 · D. 5

**Q10.** With MAE=1 and MSE=4, what is RMSE?  
A. 1 · B. 2 · C. 4 · D. 16

**Q11.** Parameter β=5, learning rate α=0.1, gradient=+4. New β:  
A. 5.4 · B. 4.6 · C. 1 · D. 5

**Q12.** A learning rate far too large can cause:  
A. Guaranteed better fit · B. Overshooting/diverging loss · C. No parameter updates · D. More class labels

### Decision trees and Gini

**Q13.** A node contains 8 samples of one class and 0 of another. Gini is:  
A. 0 · B. 0.25 · C. 0.5 · D. 1

**Q14.** A node contains 3 positives and 1 negative. Gini is:  
A. 0.125 · B. 0.375 · C. 0.5 · D. 0.75

**Q15.** Parent 10 samples; child L has 4 samples with Gini 0; child R has 6 samples with Gini 0.5. Weighted Gini:  
A. 0.2 · B. 0.3 · C. 0.5 · D. 0.6

**Q16.** Parent Gini 0.6. Split A weighted Gini 0.4, B = 0.2. Best split and its gain?  
A. A, 0.2 · B. B, 0.4 · C. A, 0.4 · D. B, 0.2

**Q17.** `min_samples_split=8`, `min_samples_leaf=3`, parent has 9 samples. Which split satisfies the *sample-count* rules?  
A. 2 + 7 · B. 1 + 8 · C. 4 + 5 · D. None

**Q18.** Training accuracy rises but test accuracy drops as tree depth increases. Most likely issue?  
A. Underfitting · B. Overfitting · C. No labels · D. Perfect generalisation

### K-means and RL

**Q19.** For a K-means cluster with 1D points `[4,6,8]`, updated centroid is:  
A. 4 · B. 6 · C. 8 · D. 18

**Q20.** A point P=(1,1) and two centroids A=(1,4), B=(3,1). Nearest centroid?  
A. A, d=3 · B. B, d=2 · C. Both equal · D. No centroid

**Q21.** One cluster has points `[2,4]`, centroid=3. Its WCSS is:  
A. 1 · B. 2 · C. 3 · D. 6

**Q22.** WCSS values for K=1..4 are `[300,120,55,53]`. Elbow choice?  
A. K=1 · B. K=2 · C. K=3 · D. K=4

**Q23.** An ε-greedy RL agent explores with ε=0.10. Probability of a random exploratory action?  
A. 1% · B. 10% · C. 50% · D. 90%

### Classification metrics

**Q24.** For a class, TP=8, FP=2, FN=4, TN=6. Which statement is correct?  
A. Precision=8/12 · B. Recall=8/10 · C. Precision=0.8 · D. Accuracy=0.8

---

# PART E — ANSWER KEY + ONE-LINE REASONS

| Q | Answer | Why |
|---|---|---|
| 1 | **B** | No class labels; discover clusters. |
| 2 | **C** | Species is the target `y`. |
| 3 | **B** | 20% of 150 = 30. |
| 4 | **C** | Test is **transform only** to avoid leakage. |
| 5 | **B** | `(8−6)/2 = 1`. |
| 6 | **B** | `(5−1)/(9−1) = 4/8 = 0.5`. |
| 7 | **C** | `1+2(3)=7`. |
| 8 | **B** | Residual `4−6 = −2`, overprediction. |
| 9 | **B** | Squared errors `1` and `4`, MSE `(1+4)/2=2.5`. |
| 10 | **B** | RMSE = `√4 = 2`. |
| 11 | **B** | `5−0.1×4 = 4.6`. |
| 12 | **B** | Large α may overshoot and diverge. |
| 13 | **A** | Pure node → Gini 0. |
| 14 | **B** | `1−[(3/4)²+(1/4)²] = 1−(9/16+1/16)=6/16=0.375`. |
| 15 | **B** | `(4/10)×0+(6/10)×0.5=0.3`. |
| 16 | **B** | Smaller weighted Gini 0.2; gain `0.6−0.2=0.4`. |
| 17 | **C** | Parent 9≥8; both 4 and 5≥3. |
| 18 | **B** | Deeper tree may memorise training data. |
| 19 | **B** | `(4+6+8)/3=6`. |
| 20 | **B** | B distance 2; A distance 3. |
| 21 | **B** | `(2−3)²+(4−3)²=1+1=2`. |
| 22 | **C** | WCSS nearly flattens after K=3. |
| 23 | **B** | ε is exploration probability = 10%. |
| 24 | **C** | Precision `8/(8+2)=0.8`; recall `8/12≈0.667`; accuracy `14/20=0.7`. |

---

# PART F — FINAL 5-MINUTE RECALL (CLOSE THE NOTE FIRST)

Can you answer all 15 without looking?

1. What are **T, E, P** in Tom Mitchell's definition?
2. What do **X** and **y** mean? List the four Iris features.
3. What is the distinction between **classification, regression, clustering and RL**?
4. Why do we use **train, validation and test** separately?
5. Why do we **fit the scaler only on training** data?
6. Write **standardisation** and **min–max** formulas.
7. Write the **linear regression** equation; explain its intercept and slope.
8. How do you calculate a **residual, MAE, MSE and RMSE**?
9. What happens with a **too-small vs too-large** learning rate?
10. Compute **Gini** for a 6/4 binary-class node.
11. Compute **weighted Gini** for 5 at Gini 0 and 15 at Gini 0.4.
12. Distinguish **max_depth, min_samples_split, min_samples_leaf**.
13. State K-means **choose/initialise/assign/update** and explain convergence.
14. How do you calculate **Euclidean distance, a centroid, WCSS**, and select K with **elbow**?
15. Which of **precision, recall, accuracy, F1** fits each error-cost scenario?

**Answers for the two numbered calculations:** Q10 = `0.48`; Q11 = `0.30`. For the remaining questions, revisit their worked sections in Part B if you cannot explain the answer in your own words.

---

## One last way to avoid exam mistakes

> **Identify the task before touching the formula.** Is the question about a *node*, a *split*, a *cluster*, a *prediction error*, or a *classification count*? Choosing the right object usually tells you which formula to use.

**Best revision order if time is limited:**

1. **Decision-tree Gini + weighted Gini + stopping rules**.
2. **K-means distance + centroid + WCSS + elbow**.
3. **Confusion matrix + precision/recall/F1**.
4. **Regression prediction + residual + MSE + learning rate**.
5. **Pipeline/leakage/scaling, ML types, and RL concepts**.

*End of Week 07 revision note.*
