# Week 08 – Applied AI: Neural Networks
## Quick Revision Notes (MCQ Exam)

**Goal:** Refresh the examinable ideas quickly the day before the exam. Based on the uploaded *Week 8: Neural Networks* lecture and the previously reviewed examination-question screenshots. This is **Applied AI**, not an advanced Machine Learning mathematics course.

> **First remember:** Perceptron calculation → count connections → AND/OR/XOR → hidden layers → training → learning rate/loss → data splits → overfitting → confusion matrix and accuracy.

## 1. MOST IMPORTANT: Perceptron calculations (Priority A)

A **perceptron** is one artificial neuron. It turns input features into an output.

- **Input (`x`)**: Number given to the neuron (e.g., count of spam words).
- **Weight (`w`)**: How strongly an input affects the result. A negative weight decreases the sum.
- **Bias (`b`)**: An extra adjustable value added to the sum.
- **Weighted sum (`z`)**: Add every `(input × weight)` and the bias.
- **Activation function (`f`)**: Turns `z` into the output `y`.

**Formula:** `z = x1*w1 + x2*w2 + ... + b`, then `y = f(z)`.

**Exam method:** ① Multiply each input by its weight → ② Add products + bias → ③ Apply the **exact threshold rule given**.

**Past-paper-style worked example (5-mark type):** inputs `4, 3, 2`; weights `−2, 3, 1`; no bias shown; output is `1` when `z >= 3`.

`z = (4 × −2) + (3 × 3) + (2 × 1) = −8 + 9 + 2 = 3` → `y = 1`.

**TRAP:** `z > 0` is *not* the same as `z >= 0`. If `z = 0`, those two rules give different outputs. **Never assume a threshold**; read the question.

## 2. Count weights in a fully connected network (Priority A)

**Connection weights between two adjacent layers = neurons in first layer × neurons in second layer.** Add each pair of layers.

**Previous examination question type (4 marks):** 10 inputs → 30 hidden neurons → 5 outputs.

`(10 × 30) + (30 × 5) = 300 + 150 = 450 connection weights`.

If there are **10 inputs → 20 hidden → 5 outputs**, the answer is `(10×20) + (20×5) = 300` connection weights.

- **Fully connected / dense** means each neuron in a layer connects to every neuron in the next layer.
- **Biases are separate parameters.** If explicitly asked for *total trainable parameters*, include biases for non-input neurons. Example 10→20→5: `300 weights + (20+5) biases = 325 parameters` (assuming one bias per hidden/output neuron).

## 3. AND, OR, XOR and the single perceptron limit (Priority A)

For **two binary inputs (0 or 1), both weights = 1, no bias**, let `z = x1 + x2`:

| Inputs | AND (`z >= 2`) | OR (`z >= 1`) | XOR |
|---|---:|---:|---:|
| 0, 0 | 0 | 0 | 0 |
| 0, 1 | 0 | 1 | 1 |
| 1, 0 | 0 | 1 | 1 |
| 1, 1 | 1 | 1 | 0 |

- **AND:** Both must be `1`.
- **OR:** At least one must be `1`.
- **XOR (exclusive OR):** **Exactly one** must be `1`, not both.
- A **single ordinary perceptron** only makes a **linear** decision boundary and **cannot solve XOR** for all input combinations.
- **A hidden layer** with multiple neurons and suitable activations can represent XOR; the lecture combines an **OR neuron and an AND neuron**.

**TRAP:** OR at `(1,1)` is **1**, whereas XOR at `(1,1)` is **0**. Threshold `2` and threshold `1` produce different answers for `(1,0)`.

## 4. Layers, depth and activations (Priority A/B)

- **Input layer:** Receives data/features.
- **Hidden layer:** Learns intermediate relationships; lies between input and output.
- **Output layer:** Produces the prediction.
- **Deep neural network:** Has **multiple processing/hidden layers**. “Deep” does **not** mean lots of data or many input features. This was a past-exam MCQ theme (**3 marks**).
- **Non-linear activation functions** allow neural networks to learn relationships more complex than a single straight-line boundary.
- Lecture reference associations: **Sigmoid → binary classification**; **Softmax → multiclass classification** (softmax outputs sum to `1`); **MSE → commonly used for regression loss**. Cross-entropy is a common classification loss.
- **Input count** generally depends on number of input features. Example from lecture: a 15×12-pixel flattened image has `180` input values. Do not confuse output neurons with the number of classes; output representation depends on design.

## 5. How the network learns (Priority B)

**Training goal: reduce the loss (error) by changing weights and biases.**

1. **Forward propagation:** Inputs move through the network → prediction is calculated.
2. **Loss function:** Measures how wrong the prediction is (larger loss generally means worse predictions).
3. **Backpropagation:** Calculates **gradients** (how changing weights affects loss), working backward.
4. **Gradient descent / optimisation:** Uses gradients to update weights in steps aimed at lower loss.
5. Repeat over the training data.

**Learning rate (`η`, eta)** controls weight-update step size:

| Learning rate | Problem |
|---|---|
| Too small | Training may be very slow or make little progress |
| Too large | May overshoot, oscillate or diverge; unstable training |

**Perceptron learning rule** (worth being able to substitute small numbers):

- `w_new = w + η * (t − y) * x`
- `b_new = b + η * (t − y)`
- `t` = true/target label; `y` = prediction; `x` = input; `η` = learning rate.
- If `t − y = 0`: no perceptron update for that example.
- Example: `w=0.4`, `η=0.1`, `t=1`, `y=0`, `x=2` → `w_new = 0.4 + 0.1(1−0)2 = 0.6`.

**Epoch:** One complete pass through the whole training dataset. One epoch can involve many weight updates.

**Inference:** Using an already trained model to predict on new input (not normally training/updating it).

## 6. Training / Validation / Test (Priority B)

| Data split | Easy memory | Used for |
|---|---|---|
| **Training set** | **Learn** | Update the model's weights |
| **Validation set** | **Choose** | Compare learning rates, architectures, checkpoints and other *hyperparameters* |
| **Test set** | **Final check** | Evaluate the finished model on unseen data |

- **Hyperparameter** = a training/design choice set by the developer, such as learning rate or hidden-layer count.
- Lecture's example ratios: **80/10/10** (larger dataset) and **60/20/20** (smaller dataset). They are examples, **not fixed rules**.
- **TRAP:** Do **not** keep tuning the model using the test set. That makes final evaluation unreliable.

## 7. Overfitting and how to reduce it (Priority B)

**Overfitting = model fits the training examples too closely but performs poorly on unseen data.**

**Warning sign:** Training accuracy is very high while validation accuracy is much lower (e.g., `99%` vs `55%`). Training accuracy may keep rising while validation accuracy falls.

**Two methods in the lecture:**

| Method | What happens | MCQ memory trick |
|---|---|---|
| **Early stopping** | Monitor **validation performance** and stop when improvement ceases; restore/keep a good checkpoint | **Stop training** |
| **Dropout** | Randomly set some hidden-neuron activations to zero **during training** | **Temporarily switch off neurons** |

- Dropout is **not** permanent deletion; it is normally disabled during inference.
- **Epoch choice example:** Validation accuracies `73%, 83%, 88%, 81%` → **Epoch 3** is the best of those checkpoints because its validation accuracy is `88%`.
- **Underfitting:** Model has not learned enough; often does poorly even on training data.

## 8. Accuracy, class imbalance and confusion matrix (Priority B)

**Accuracy = `(number of correct predictions / total predictions) × 100%`.**

If 15 of 20 predictions are correct, accuracy = `(15/20)×100 = 75%`.

**Class imbalance:** One class has far more examples than another. Example: `99` normal emails and `1` spam email. A detector that **always says “Not spam”** has `99% accuracy` but catches **0 spam**. **High accuracy alone can mislead.**

**Confusion matrix:** Counts correct and incorrect predictions. Treat **Spam = Positive** and **Not Spam = Negative** for the table below.

| Actual \\ Prediction | Predict **Spam (positive)** | Predict **Not Spam (negative)** |
|---|---|---|
| Actually **Spam** | **TP** = True Positive (caught spam) | **FN** = False Negative (missed spam) |
| Actually **Not Spam** | **FP** = False Positive (false alarm) | **TN** = True Negative (normal email correctly passed) |

**Memory shortcuts:**

- **True** = prediction is **correct**; **False** = prediction is **wrong**.
- **Positive** = predicted spam; **Negative** = predicted not spam.
- **FP** = false alarm. **FN** = missed real positive case.
- **Accuracy = `(TP + TN) / (TP + TN + FP + FN) × 100%`.**

**Lecture example:** TP=`6`, FN=`4`, FP=`3`, TN=`87` → accuracy = `(6+87)/100 × 100 = 93%`.

## 9. Previous-paper question types to prioritise

These are **confirmed themes from the earlier examination screenshot review**, not a promise about the next paper:

1. **Perceptron output** given inputs, weights and threshold (**5 marks** in a reviewed in-class test): multiply, add, activate.
2. **Count fully connected weights** (**4 marks**): multiply adjacent layer sizes and add.
3. **Meaning of “deep”** (**3 marks**): multiple processing/hidden layers.
4. **Threshold-neuron logic**: determine AND or OR from binary inputs and threshold; know XOR limitation.
5. **Early history fact** (**1 mark**): neural-network research began in the **1940s** (low time investment).

**Important lecture-based potential MCQs (not confirmed as the above past-paper questions):** learning-rate effects, loss/backpropagation/gradient descent, epoch, data split purposes, overfitting, dropout, early stopping, accuracy and class imbalance, confusion-matrix cells, sigmoid vs softmax.

## 10. Thirty-second exam-day checklist

- [ ] Can I calculate `z` and apply **`>` vs `>=`** correctly?
- [ ] Can I count connection weights, and add biases **only if asked**?
- [ ] Do I know the AND/OR/XOR truth tables and why one neuron fails on XOR?
- [ ] Do I know shallow vs deep, hidden layers, and simple activation purposes?
- [ ] Can I identify **forward**, **loss**, **backprop**, **gradient descent**, **learning rate**, **epoch**?
- [ ] Can I calculate a tiny perceptron weight update?
- [ ] Do I know **Train = Learn; Validation = Choose; Test = Final**?
- [ ] Can I recognise overfitting, and distinguish **Early Stopping vs Dropout**?
- [ ] Can I identify TP, TN, FP, FN and calculate accuracy?
- [ ] Do I know why **99% accuracy may still be a bad spam detector**?

### If you only have five minutes

**Memorise this line:** `z = sum(x*w)+b` → activation; connections = adjacent layer products; XOR needs hidden-layer power; forward predicts → loss measures error → backprop gets gradients → gradient descent updates; **Train/Learn, Val/Choose, Test/Final**; overfitting → **early stopping OR dropout**; accuracy = correct/total; **FP=false alarm, FN=missed case**.

---
**Source:** *8 Applied AI – Neural Networks.pdf* (77 PDF pages; learning outcomes, worked perceptron and XOR examples, training workflow, data splits, overfitting and evaluation). Previous-paper details reflect the review of the uploaded *Applied AI Papers* ZIP earlier in this conversation. Terminology is kept at lecture/Applied-AI-MCQ level; advanced derivations are intentionally omitted.
