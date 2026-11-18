# Week 09 — Applied AI Revision Notes

**Module:** Applied AI (6COSC020W)  
**Lecture:** Deep Learning and Computer Vision (`Applied_AI_Nov_26_final.pdf`, 90 pages)  
**Purpose:** Understand the lecture, revise quickly, and practise exam-style questions.  
**Language:** Simple English with the technical words you need in the exam.

> **How to use this file:** Read Sections 1–9 to understand the story. Work through the examples in Section 10 **without looking at the answers**. Then try the MCQs in Section 12. On the day before the exam, focus on Section 13.
>
> **Priority key:** **A = essential foundation / highly examinable calculation or idea**, **B = important lecture comparisons**, **C = recognition-level detail**. These are study priorities based on lecture content and common question formats, **not verified counts of past-paper questions**. Practice questions below were written for revision; they are **not copied from an examination paper**.

---

## 1. The Big Picture: What Is Computer Vision? [A]

**Computer vision** is about helping computers understand information in images.

A computer does **not** see a cat the way a human does. It initially sees an **array of numbers** describing pixels. A learning system must discover useful patterns from those numbers.

### Four computer vision tasks (lecture p. 8)

| Task | What it answers | Example |
|---|---|---|
| **Classification** | **What** is in the image? | "This is a dog." |
| **Localisation** | **Where** is the main object? | Draw a box around the dog. |
| **Object detection** | What objects are present **and where**? | Find two dogs and one person, each with a box. |
| **Segmentation** | Which **pixels** belong to which region/object? | Mark all pixels belonging to the dog. |

**Common MCQ trap:** Classification gives a class label; detection also finds object locations; segmentation is pixel-level.

### Images as numbers (lecture pp. 9–11)

- A **pixel** is one small picture element.
- A grayscale image is a 2D grid of brightness values (often 0 = black and 255 = white for 8-bit images).
- A typical colour RGB image has **three channels:** Red, Green, Blue.
- Example: `32 × 32 × 3` means **height 32, width 32, three colour channels**.
- A classifier may output class probabilities, such as Cat 0.7 and Dog 0.3, and choose the higher one.

---

## 2. Why Deep Learning Rather Than Hand-Crafted Features? [A]

### Traditional computer vision (lecture pp. 12–26)

`Image → Human-designed feature extractor → Classifier → Prediction`

Engineers first decide which features matter, such as edges, corners, textures and shapes. They might use **HOG, SIFT or SURF**, then train a classifier such as **SVM, decision tree, random forest or k-NN**.

**Problem:** The same object looks different when the camera angle, size, lighting, pose, background or visible portion changes. Designing features for every situation is difficult.

### Deep learning (lecture pp. 30–33)

`Image → Network learns useful features → Network makes prediction`

The CNN learns **both the feature representation and the classifier** from labelled data. This is called **end-to-end learning**.

| Traditional approach | CNN/deep learning |
|---|---|
| Humans hand-design many features | Filters/features are **learned from data** |
| Feature extractor and classifier are often separate | Can train feature extraction and prediction together |
| Hard to handle every visual variation manually | Learns representations from many examples |

### Feature hierarchy

A deep CNN usually learns increasingly complex representations:

`Pixels → Edges/colours → Textures & object parts → Complex object patterns → Class prediction`

**Why do we need many layers?** The first layer may detect an edge, but one edge alone does not identify a car. Later layers combine earlier patterns into parts (wheels, windows) and then more complex objects.

---

## 3. Why a CNN Instead of a Fully Connected Network? [A]

In a **fully connected (dense) network**, every neuron in the next layer connects to every value in the previous layer. If we flatten a large image immediately, the network:

1. Uses **many parameters** (weights).
2. Does not explicitly preserve the image's original **2D neighbourhood structure**.

A **Convolutional Neural Network (CNN)** solves this differently:

- **Local receptive fields:** A neuron looks at a small image patch, not necessarily the entire image.
- **Weight sharing:** The **same filter weights** are reused at different image locations.
- **Multiple filters:** Different filters learn different visual patterns.
- **Feature maps:** Each filter creates an output map showing where its learned pattern responds strongly.

**Analogy:** Instead of memorising the exact location of a cat's ear, use a small pattern detector that can scan the whole image.

**Remember:** Sparse/local connections + shared weights reduce the number of unique weights compared with naive dense connections.

---

## 4. Convolution, Filters, Feature Maps, Stride and Channels [A]

### 4.1 What is a filter/kernel?

A **filter** (also called a **kernel**) is a small collection of learned weights, often arranged as `3 × 3` for a 2D spatial filter.

A filter scans local patches of the image to produce a **feature map**.

- **One filter → one output feature map.**
- **K filters → K output feature maps**, so output depth/channels = **K**.
- Each filter is reused at every spatial location (**weight sharing**).
- Different filters have **different** learned weights.

### 4.2 One convolution calculation (lecture pp. 18–22)

For one position:

1. Put the filter over an equally sized image patch.
2. **Multiply matching entries.**
3. **Add all products.**
4. Record the result in the feature map.
5. Slide to the next position and repeat.

Example:

```text
Image patch       Filter             Elementwise products
1  0  0           1  0  1            1  0  0
0  1  0     ×     0  1  0      →     0  1  0
0  0  1           1  0  1            0  0  1

Convolution response = 1 + 1 + 1 = 3
```

**The multiplication sign means element-by-element multiplication, not matrix multiplication.** (Technically the operation used by common CNN software is cross-correlation, but the lecture calls it convolution.)

**From the lecture's 5 × 5 X example:** A 3 × 3 X-shaped filter produces a **3 × 3 feature map**:

```text
3  0  3
0  5  0
3  0  3
```

The centre value **5** is the strongest match to that filter.

### 4.3 What is stride?

**Stride** = how many pixels the filter moves each time.

- **Stride 1:** move one pixel at a time → more output positions.
- **Stride 2:** move two pixels at a time → generally smaller output.

For a square input of size `N × N`, square filter `F × F`, stride `S`, **no padding**:

```text
Output height/width = floor((N - F) / S) + 1
```

Example: `7 × 7` input, `3 × 3` filter, stride `2`:

`floor((7 - 3)/2) + 1 = 3`, so output is `3 × 3` **per filter**.

### 4.4 Padding: just what the lecture needs (pp. 38, 40, 60)

**Padding** adds extra values (commonly zeros) around the image border.

- **No padding (`P = 0`):** a `32 × 32` input with a `5 × 5` filter, stride 1 → `28 × 28`.
- **Padding 1 (`P = 1`):** a `3 × 3` filter, stride 1 can keep the same height and width.

General formula for one spatial dimension:

```text
Output = floor((N + 2P - F) / S) + 1
```

**Exam strategy:** Understand what padding changes; don't spend time on advanced padding variations. The lecture uses **both** no padding (p. 38) and padding 1 (pp. 40 and 60).

### 4.5 Channels and output volume

For an RGB image `32 × 32 × 3`, a spatial `5 × 5` filter must span all **3 input channels**. Its actual shape is `5 × 5 × 3`.

From lecture p. 38:

```text
Input:          32 × 32 × 3
Filters:        2 filters, each 5 × 5 × 3
Stride:         1
Padding:        0
Output:         28 × 28 × 2
```

**Why is the last dimension 2 rather than 3?** Output depth is the **number of filters**, not the input's number of channels.

### 4.6 Convolutional trainable parameters

If the convolution uses **one bias per output filter**:

```text
Parameters = (Filter height × Filter width × Input channels + 1) × Number of filters
```

For the lecture p. 38 example:

`(5 × 5 × 3 + 1) × 2 = (75 + 1) × 2 = 152 parameters`.

**Key:** Do **not** multiply the number of weights by all image locations. The filter reuses the **same weights** at every location.

---

## 5. ReLU, Pooling, Flattening, Dense and Softmax [A]

A simple classification CNN can be understood as:

`Input image → Convolution → ReLU → Pooling → ... → Flatten → Dense → Softmax → Predicted class`

Real CNNs may repeat Convolution/ReLU/Pooling blocks several times.

### 5.1 ReLU — Rectified Linear Unit (lecture pp. 82–83)

```text
ReLU(x) = max(0, x)
```

Examples: `ReLU(-4) = 0`, `ReLU(0) = 0`, `ReLU(7) = 7`.

**Purpose:** Introduce **non-linearity**, so the network can learn complex relationships. It is computationally simple and commonly used after convolution.

**Trap:** ReLU does **not** mean "turn everything into a probability." That is not its job.

### 5.2 Pooling (lecture p. 42)

**Pooling reduces height and width** by summarising small regions of each feature map.

| Operation | What it keeps | Example `2, 4, 6, 8` |
|---|---|---|
| **Max pooling** | Largest value in the window | **8** |
| **Average/mean pooling** | Arithmetic mean | **5** |

Typically `2 × 2` pooling with stride `2`:

- `12 × 12 × 6` → `6 × 6 × 6`.
- Height and width become smaller.
- **Channel count stays the same.**
- Ordinary max/average pooling has **no learned weights**.

**Exact lecture example, p. 42:**

```text
Input 4×4 feature map:          2×2 max pool:     2×2 average pool:
31   15   28  184             100   184         36.5   80
 0  100   70   38              12    45         12     15
12   12    7    2
12   12   45    6
```

### 5.3 Flattening

**Flatten** changes a group of 2D feature maps into a **1D list**. It changes the arrangement **without removing any values**.

```text
2 × 2 × 3 feature maps → 12 values
3 × 3 × 2 feature maps → 18 values

Flattened length = Height × Width × Channels
```

### 5.4 Dense (fully connected) layer

Every input connects to every output neuron in a Dense layer.

For one neuron, its raw output is based on weighted inputs and a bias:

`z = x1*w1 + x2*w2 + ... + bias`

**Trainable parameter count** (assuming a bias for each output neuron):

```text
Dense parameters = (Input values × Output neurons) + Output neurons
```

Example: 4 inputs, 3 output neurons:

- `4 × 3 = 12` weights.
- `3` biases.
- **15 total trainable parameters**.

### 5.5 Softmax

**Softmax** converts a set of raw class scores into **class probabilities**:

- Each probability is **strictly between 0 and 1** for finite input scores.
- All probabilities **sum to 1 (100%)**.
- The **highest raw score** remains the **highest probability**.

Example: If Cat has the largest raw score, Cat also has the largest Softmax probability.

**Trap:** A negative raw score does **not** produce a negative Softmax probability.

### ReLU vs Softmax vs Pooling

| Operation | Main purpose | Learnable parameters? |
|---|---|---|
| Convolution | Detect learned patterns | **Yes** |
| ReLU | Add non-linearity | **No** |
| Max/average pooling | Reduce spatial dimensions | **No** |
| Flatten | Rearrange values into a list | **No** |
| Dense | Combine inputs for predictions | **Yes** |
| Softmax | Convert class scores into probabilities | **No** |

---

## 6. Worked CNN Pipeline from the Lecture [A]

**Lecture p. 40:** A small illustrative CNN processes an `8 × 8 × 1` image. Each convolution uses `3 × 3` filters, **stride 1, padding 1**. Each max-pool uses a `2 × 2` window, **stride 2**.

| Stage | Convolution | After ReLU + Max Pool |
|---|---|---|
| Input | — | `8 × 8 × 1` |
| 1 | **2 filters**, each `3 × 3 × 1` | **`4 × 4 × 2`** |
| 2 | **4 filters**, each `3 × 3 × 2` | **`2 × 2 × 4`** |
| 3 | **8 filters**, each `3 × 3 × 4` | **`1 × 1 × 8`** |
| Classifier | Flatten: `1 × 1 × 8 = 8` values | Dense → **3 class scores** |

**Explain the flow:**

1. Stage 1's two filters create **two maps**; padding preserves `8 × 8` during convolution, then pooling makes `4 × 4 × 2`.
2. Stage 2's four filters scan **all two input channels**, create **four maps**, then pooling makes `2 × 2 × 4`.
3. Stage 3's eight filters scan **all four input channels**, create **eight maps**, then pooling makes `1 × 1 × 8`.
4. Flatten produces **8 values**. The classifier converts those values into **3 class scores**.

### Optional parameter-count exercise using the same architecture

**Assumption:** One bias per convolution filter and per Dense neuron; no extra trainable layers.

| Layer | Calculation | Parameters |
|---|---|---:|
| Conv 1 | `(3 × 3 × 1 + 1) × 2` | 20 |
| Conv 2 | `(3 × 3 × 2 + 1) × 4` | 76 |
| Conv 3 | `(3 × 3 × 4 + 1) × 8` | 296 |
| Dense, 8 inputs → 3 outputs | `(8 × 3) + 3` | 27 |
| **Total** | `20 + 76 + 296 + 27` | **419** |

This count is **our worked calculation from the slide's dimensions**, not a total printed on the slide. ReLU, pooling and flattening add **0 trainable parameters**.

---

## 7. How a CNN Learns: Training, Loss and Gradients [A]

### 7.1 One training cycle

**Example:** A labelled training photo contains a **cat**.

1. **Forward pass / feedforward:** The image passes through the CNN; the CNN predicts Cat 20%, Dog 80%.
2. **Loss function:** Measures how wrong the prediction is compared with the true label (Cat).
3. **Backpropagation:** Calculates the **gradients** (how changes in trainable weights affect the loss).
4. **Optimisation:** **Gradient descent** or **stochastic gradient descent (SGD)** changes weights to aim for a lower loss.
5. **Repeat:** The network processes further labelled examples, often over multiple epochs.

**Important distinction:** The loss tells the model **how bad** the prediction is; backpropagation calculates **how weights affect that loss**; the optimiser **updates the weights**.

### 7.2 Gradient descent formula (lecture p. 41)

```text
new weight = old weight - learning rate × loss gradient

θ_new = θ - η (∂L/∂θ)
```

- `θ` (theta) = a trainable parameter (weight or bias).
- `η` (eta) = learning rate, the step size of the update.
- `L` = loss.
- `∂L/∂θ` = gradient of loss with respect to that parameter.

**Tiny numeric example:** Old weight = `2.0`, learning rate = `0.1`, gradient = `3.0`:

`New weight = 2.0 - (0.1 × 3.0) = 1.7`.

If the gradient is negative, subtraction can **increase** the weight instead.

### 7.3 What is special about shared CNN filters?

The **same filter weight** is used at many image locations. During backpropagation, gradient contributions from those locations are **combined/summed** to update that shared weight (lecture p. 41).

A filter is **not given a separate new set of weights for every position**.

### 7.4 Training vocabulary (lecture p. 86)

| Term | Meaning in simple English |
|---|---|
| **Training set** | Examples the model learns from |
| **Label/target** | Correct answer provided with a training example |
| **Batch / mini-batch** | A group of training examples processed for an update |
| **Epoch** | One full pass over the training dataset |
| **SGD** | An optimisation approach using gradients from training examples/mini-batches |
| **Learning rate** | Size of the weight-update step |
| **Overfitting** | Performs well on training examples but poorly on new examples |
| **Dropout** | Randomly deactivates some units during training to help reduce overfitting |
| **Data augmentation** | Creates varied training examples (e.g., flips/crops/lighting changes) to improve generalisation |

**MCQ traps:** Dropout is a regularisation technique, **not** a new image class. Data augmentation changes training examples, **not** their intended meaning. More epochs do **not guarantee** better generalisation.

---

## 8. CNN Architectures and What Makes Each Different [B]

The lecture gives a history and a set of important model families (pp. 55–74). **Learn the distinctive idea**, not every detail of the original research papers.

| Model / milestone | Date in lecture | Recognition clue |
|---|---|---|
| **Neocognitron** (Fukushima) | 1980 | Early biologically inspired architecture |
| **Backpropagation** (Rumelhart, Hinton, Williams) | 1986 | General training method; **not itself a CNN architecture** |
| **LeNet** (LeCun and colleagues) | 1989–1998 | **Handwritten digit recognition** |
| **AlexNet** | **2012** | ImageNet breakthrough, **GPU training**, deeper CNN |
| **VGG / VGGNet** | **2014** | Repeated **small 3 × 3 convolutions** and deeper networks |
| **GoogLeNet / Inception** | **2014** | **Parallel multi-scale branches**, often uses `1 × 1` convolutions |
| **ResNet** | **2015** | **Residual / skip connections** make very deep networks easier to train |
| **MobileNet** | **2017** | **Depthwise separable convolutions** for efficient mobile models |
| **EfficientNet** | **2019** | **Compound scaling:** depth, width and image resolution together |

### 8.1 AlexNet (lecture p. 59)

- Won ImageNet 2012 by a large margin.
- **5 convolutional layers** and **3 fully connected layers**.
- Used **ReLU**, max pooling, **dropout**, and **GPU training**.
- Showed deep CNNs could outperform traditional computer vision techniques.

**Remember:** AlexNet → **2012, ImageNet, GPUs, 5 conv + 3 FC**.

### 8.2 VGG (lecture pp. 60–61)

- VGG-16: **16 learned layers**; VGG-19: **19 learned layers**.
- Uses repeated **`3 × 3` convolutions** (stride 1, padding 1).
- Uses **`2 × 2` max pooling** (stride 2).
- Why several small convolutions? Three stacked `3 × 3` filters have an **effective receptive field** of `7 × 7`, with fewer spatial filter weights than one `7 × 7` filter for comparable channel widths.

Lecture's simplified spatial-weight comparison: one `7 × 7` uses **49** spatial weights vs three `3 × 3` layers using **`3 × 9 = 27`** per corresponding one-channel path. Full network parameter counts also depend on channel numbers.

### 8.3 GoogLeNet / Inception (lecture pp. 62–65)

**Problem:** Different visual features need different spatial scales. Large filters can be computationally expensive.

**Solution:** An **Inception module** processes the same input through **parallel convolution/pooling branches**, then joins the outputs **along the channel dimension**.

A **`1 × 1` convolution** can reduce the number of channels before more expensive filters.

- `1 × 1` does **not** mean "only one output channel".
- With stride 1, a `1 × 1` convolution can keep **height and width unchanged** while changing **channel count**.
- Lecture p. 63 example: **`56 × 56 × 64` input + 32 filters of size `1 × 1 × 64` → `56 × 56 × 32` output**.
- GoogLeNet uses **global average pooling**, reducing reliance on large Dense layers.

**Remember:** Inception → **parallel branches, multi-scale patterns, channel reduction with `1 × 1`**.

### 8.4 ResNet (lecture pp. 71–72)

**Problem:** Adding more layers can make optimisation harder, even when the extra capacity should be useful.

**Residual block:** Learns a correction `F(x)` while an identity shortcut carries the input `x` forward:

```text
Output = F(x) + x
```

These **skip/residual connections** help train very deep networks. The lecture mentions **152 layers** in a ResNet ImageNet example.

**Trap:** A skip connection does **not** simply delete all the layers in between; it adds a shortcut path around them.

### 8.5 MobileNet (lecture p. 73)

**Goal:** Make CNNs cheaper for **mobile and embedded devices**.

A standard spatial convolution is replaced with **depthwise + pointwise** operations:

1. **Depthwise convolution:** One spatial filter per input channel; processes channels separately.
2. **Pointwise convolution:** `1 × 1` filters **mix information across channels** and create output channels.

Lecture's example: 32 input channels and 64 output channels, `3 × 3` filters, **excluding biases and batch normalisation**:

| Calculation | Weight count |
|---|---:|
| Standard convolution: `3 × 3 × 32 × 64` | **18,432** |
| Depthwise: `3 × 3 × 32` | **288** |
| Pointwise: `1 × 1 × 32 × 64` | **2,048** |
| Depthwise + pointwise | **2,336** |

Thus `18,432 / 2,336 ≈ 7.9` times fewer weights in this example. **Lower weight/computation counts do not guarantee the same speedup on every device.**

### 8.6 EfficientNet (lecture p. 74)

Instead of scaling only one aspect of a CNN, EfficientNet scales **three dimensions together**:

- **Depth:** Number of repeated layers.
- **Width:** Number of channels per layer.
- **Resolution:** Input image size.

The lecture calls this **compound scaling** and describes the EfficientNet **B0–B7** family.

**Remember:** EfficientNet → **balanced scaling of depth + width + input resolution**.

---

## 9. Activation Functions and Other Lecture Details [B/C]

### Activation comparison (lecture pp. 82–83)

| Function | Typical output range | Important observation |
|---|---|---|
| **Sigmoid** | `(0, 1)` | Can saturate and cause very small gradients |
| **tanh** | `(-1, 1)` | Zero-centred, but can also saturate |
| **ReLU** | `[0, ∞)` | `max(0, x)`; simple and avoids positive-side saturation |

**Do not confuse:** Sigmoid/tanh are individual activation functions; **Softmax** operates across class scores to create probabilities summing to 1.

### Why deep models are not automatically better

- Extra layers can increase learning capacity, but also make **training/optimisation harder**.
- **ResNet** tackles depth-related training difficulty using skip connections.
- **Dropout** and **data augmentation** can help generalisation.
- Real-world vision must handle **lighting, pose, scale, occlusion and background clutter**.

### Applications mentioned in the lecture

Face recognition, retinal haemorrhage detection, robotics, self-driving vehicles and surveillance.

---

## 10. Calculations You Should Be Able to Do [A]

### Worked Example 1 — Filter response

```text
Patch              Filter
1  2               1  0
3  4               0  1

Output = (1×1) + (2×0) + (3×0) + (4×1) = 5
```

**Method:** Elementwise multiply, then add.

### Worked Example 2 — Convolution output size

`Input 9 × 9`, `Filter 3 × 3`, `Stride 1`, `Padding 0`, **4 filters**:

- Height: `(9 − 3)/1 + 1 = 7`.
- Width: `7`.
- Output channels: `4`.
- **Output: `7 × 7 × 4`.**

### Worked Example 3 — Pooling output size

`Input 12 × 12 × 6`, max pool `2 × 2`, stride `2`:

- Height: `12 / 2 = 6`.
- Width: `6`.
- Channels: still `6`.
- **Output: `6 × 6 × 6`.**

### Worked Example 4 — Flatten

`3 × 3 × 2` → `3 × 3 × 2 =` **18 values**.

### Worked Example 5 — Dense parameters

`4 inputs → 3 output neurons`:

- Weights: `4 × 3 = 12`.
- Biases: `3`.
- **Total: 15 parameters**.

### Worked Example 6 — Convolution parameters

`3 × 3` filters, `3 input channels`, `8 output filters`, one bias per filter:

`(3 × 3 × 3 + 1) × 8 = 28 × 8 =` **224 parameters**.

### Worked Example 7 — ReLU then max pooling

```text
Before ReLU:          After ReLU:
-4   3                 0   3
 7  -2                 7   0

2×2 max-pooling result = 7
```

### Worked Example 8 — Gradient descent

`Old weight = 2.0`, `gradient = 3.0`, `learning rate = 0.1`:

`2.0 − (0.1 × 3.0) =` **1.7**.

---

## 11. Most Important Comparisons and MCQ Traps [A/B]

| Easy-to-confuse terms | Correct difference |
|---|---|
| **Feature map vs filter** | Filter = learned weights; feature map = output after applying that filter |
| **Input channels vs output channels** | Input channels determine filter depth; number of filters determines output channels |
| **Stride vs padding** | Stride = step size; padding = added border values |
| **Convolution vs pooling** | Convolution uses learned filters; ordinary pooling summarises without learned weights |
| **Max vs mean pooling** | Highest value vs arithmetic average |
| **ReLU vs Softmax** | ReLU introduces non-linearity; Softmax converts class scores to probabilities |
| **Flatten vs pooling** | Flatten rearranges all values; pooling reduces the number of spatial positions |
| **Dense vs convolution** | Dense connects all inputs to each output; convolution uses local shared filters |
| **Loss vs backpropagation vs optimiser** | Measure prediction error vs calculate gradients vs update weights |
| **Epoch vs mini-batch** | Full pass over dataset vs subset/group used in training |
| **Dropout vs data augmentation** | Randomly deactivate units vs create variations of training inputs |
| **Classification vs detection** | Class label vs class plus position(s) |
| **VGG vs ResNet** | Repeated `3 × 3` filters vs residual shortcuts |
| **GoogLeNet vs MobileNet** | Parallel multi-scale branches vs depthwise+pointwise efficiency |
| **MobileNet vs EfficientNet** | Efficient convolution design vs compound model scaling |

**High-yield rules:**

1. **Same convolution filter reused at all locations** = weight sharing.
2. **One filter produces one feature map.**
3. **Output channel count = number of convolution filters.**
4. **Ordinary pooling changes height/width, not channels.**
5. **Flatten preserves the total number of values.**
6. **Dense parameter calculations usually include biases.**
7. **Softmax probabilities sum to 1.**
8. **Backpropagation calculates gradients; gradient descent updates weights.**
9. **ReLU replaces negative values with 0.**
10. **Bigger raw score → bigger Softmax probability.**

---

## 12. Exam-Style Practice MCQs

> These questions were created from the Week 9 lecture topics to practise concepts and calculations. Try them before opening the answers.

**Q1.** An image classifier says "Cat" but provides no location. Which task is this?  
A. Segmentation · B. Classification · C. Detection · D. Localisation

**Q2.** What is the key advantage of weight sharing in CNNs?  
A. A separate filter is needed for every location · B. The network needs fewer unique weights · C. It removes the training process · D. It changes all images to grayscale

**Q3.** What does a convolution filter do at one location?  
A. Keeps the largest pixel · B. Multiplies corresponding patch/filter values and sums them · C. Divides all pixels by 255 · D. Flattens the image

**Q4.** A `9 × 9 × 3` input is processed by four `3 × 3` filters, stride 1, no padding. Output shape?  
A. `7 × 7 × 3` · B. `9 × 9 × 4` · C. `7 × 7 × 4` · D. `3 × 3 × 4`

**Q5.** A layer applies `2 × 2` max pooling, stride 2, to `12 × 12 × 6`. Output?  
A. `6 × 6 × 6` · B. `6 × 6 × 3` · C. `12 × 12 × 6` · D. `24 × 24 × 6`

**Q6.** ReLU receives `[-2, 0, 5]`. What does it output?  
A. `[-2, 0, 5]` · B. `[2, 0, 5]` · C. `[0, 0, 5]` · D. `[0, 1, 1]`

**Q7.** What is the average-pooling result for `[2, 4; 6, 8]` using one `2 × 2` window?  
A. 8 · B. 5 · C. 20 · D. 2

**Q8.** Flattening `3 × 3 × 2` produces how many values?  
A. 9 · B. 6 · C. 18 · D. 27

**Q9.** A Dense layer has 4 inputs and 3 output neurons, with a bias per output. Parameters?  
A. 12 · B. 15 · C. 7 · D. 16

**Q10.** Eight `3 × 3` filters process an input with 3 channels, using one bias per filter. Parameters?  
A. 72 · B. 216 · C. 224 · D. 288

**Q11.** Softmax receives class scores Cat = 3, Dog = −1, Bird = 2. What must be true?  
A. Dog's probability is negative · B. Cat has highest probability, total = 100% · C. Cat is exactly 30% · D. Cat becomes 100%

**Q12.** What does a loss function measure during supervised training?  
A. How wrong the model is relative to the correct label · B. Pixel count · C. Number of filters · D. Number of GPUs

**Q13.** Which statement is accurate?  
A. Backpropagation replaces every negative output with zero · B. Gradient descent computes the class labels · C. Backpropagation computes gradients used for weight updates · D. Pooling updates filter weights

**Q14.** One **epoch** means:  
A. One convolution window · B. One complete pass through the training dataset · C. One class prediction · D. One dropout unit

**Q15.** Which architecture is identified with **skip/residual connections**?  
A. VGG · B. MobileNet · C. ResNet · D. LeNet

**Q16.** Which architecture uses **parallel multi-scale branches**?  
A. Inception/GoogLeNet · B. AlexNet · C. EfficientNet · D. LeNet

**Q17.** A `1 × 1` convolution with 32 filters receives `56 × 56 × 64`, stride 1, no padding. Output?  
A. `1 × 1 × 32` · B. `56 × 56 × 64` · C. `56 × 56 × 32` · D. `32 × 32 × 56`

**Q18.** MobileNet's main efficiency trick is:  
A. Use no convolution · B. Depthwise plus pointwise convolution · C. Always use fully connected layers · D. Train without labels

**Q19.** EfficientNet primarily balances scaling of which three dimensions?  
A. Loss, accuracy and batch · B. Depth, width and input resolution · C. RGB, HSV and CMYK · D. Sigmoid, tanh and ReLU

**Q20.** Which statement about feature learning is correct?  
A. CNN filters must all be manually designed · B. Earlier layers tend to learn simpler patterns than later layers · C. Pooling increases channel count · D. Flatten learns edge-detector weights

<details>
<summary><strong>Open answer key with explanations</strong></summary>

| Q | Answer | Why |
|---|---|---|
| 1 | **B** | Classification answers what the image contains. |
| 2 | **B** | Same weights are reused across image positions. |
| 3 | **B** | Convolution computes a sum of elementwise products. |
| 4 | **C** | `(9−3)+1 = 7`; 4 filters → 4 output channels. |
| 5 | **A** | Pooling halves height/width; 6 channels remain. |
| 6 | **C** | ReLU replaces negative values with zero. |
| 7 | **B** | `(2+4+6+8)/4 = 5`. |
| 8 | **C** | `3×3×2 = 18` values. |
| 9 | **B** | `4×3+3 = 15`. |
| 10 | **C** | `(3×3×3+1)×8 = 224`. |
| 11 | **B** | Softmax keeps score ranking and sums to 100%. |
| 12 | **A** | Loss compares prediction with correct target. |
| 13 | **C** | Backpropagation produces gradients. |
| 14 | **B** | One full pass over the training set. |
| 15 | **C** | ResNet uses residual/identity shortcuts. |
| 16 | **A** | Inception processes several scales in parallel. |
| 17 | **C** | `1×1` mixes channels; 32 filters → 32 output channels. |
| 18 | **B** | Depthwise + pointwise reduces computation. |
| 19 | **B** | Compound scaling balances depth, width, resolution. |
| 20 | **B** | Deeper layers combine simpler features into more complex ones. |

</details>

### Extra short-answer prompts

1. Explain **two differences** between a standard fully connected network and a CNN.
2. Show **every multiplication** needed for a small convolution response.
3. Describe what changes to height, width and channels after a pooling layer.
4. Explain **why** weight sharing is useful and how it affects the number of trainable parameters.
5. Explain the roles of **loss, backpropagation and gradient descent** in one connected training cycle.
6. Compare the main innovations of **VGG, GoogLeNet, ResNet, MobileNet and EfficientNet**.

---

## 13. Last-Minute Exam Revision Sheet (5–10 Minutes)

### The CNN story in one sentence

**CNNs learn filters that detect local visual patterns, build increasingly complex feature maps, and use the learned information to predict image classes.**

### Core pipeline

`Pixels → Convolution → ReLU → Pooling → [repeat] → Flatten → Dense → Softmax`

### The five formulas

```text
1. One convolution response = sum(patch value × matching filter weight)
2. Conv output dimension = floor((N + 2P - F)/S) + 1
3. Conv parameters = (Fh × Fw × input channels + 1) × number of filters
4. Flattened length = height × width × channels
5. Dense parameters = (inputs × outputs) + outputs
```

> Parameter formulas above assume **one bias per output channel/neuron**. For special questions explicitly excluding bias, omit the bias term.

### Fast facts to memorise

- **Stride:** how far the filter moves.
- **Padding:** border values added around an input.
- **Receptive field:** region of the input a unit responds to.
- **Feature map:** output generated by a filter.
- **Weight sharing:** identical filter weights across image positions.
- **Number of output channels:** number of convolutional filters.
- **ReLU:** `max(0, x)`; adds non-linearity.
- **Max pooling:** largest value; usually reduces height/width, **not** channels.
- **Average pooling:** mean of the values.
- **Flatten:** reshape to 1D; number of values stays the same.
- **Softmax:** probabilities add to 100%.
- **Backpropagation:** gradients; **optimiser:** weight updates.
- **Dropout and augmentation:** reduce overfitting risk.
- **LeNet:** handwritten digits.
- **AlexNet:** 2012, GPUs, 5 conv + 3 FC.
- **VGG:** repeated `3 × 3` convolutions.
- **GoogLeNet:** Inception, parallel branches, `1 × 1` channel reduction.
- **ResNet:** skip/residual connections.
- **MobileNet:** depthwise + pointwise convolutions.
- **EfficientNet:** scale depth + width + resolution together.

### Before submitting an MCQ

- [ ] Did I use **height × width × channels** correctly?
- [ ] Did I confuse **number of input channels** with **number of filters**?
- [ ] Did I apply **stride and padding** correctly?
- [ ] Did I leave the **channel count unchanged** after ordinary pooling?
- [ ] Did I include **biases** when calculating trainable parameters?
- [ ] Did I distinguish **ReLU** from **Softmax**?
- [ ] Did I distinguish **loss**, **backpropagation**, and **gradient descent**?
- [ ] Did I match the CNN architecture with its **main innovation**?

---

## Source and Scope Notes

- **Primary source:** `Applied_AI_Nov_26_final.pdf`, *Deep Learning and Computer Vision*, Applied AI (6COSC020W), Aleka Psarrou.
- Key areas in the PDF: **pp. 8–14** (tasks, image data, traditional features); **pp. 17–23** (convolution example); **pp. 31–44** (learned features, CNN operations, training, pooling); **pp. 55–74** (CNN history and architectures); **pp. 82–86** (activations and training concepts); **p. 89** (lecture summary).
- Worked arithmetic and MCQs were **prepared for revision** from the lecture topics. When the note extends a slide with an extra numeric calculation, it labels it as a worked example rather than pretending the slide contained that result.
- This is a **revision aid**, not a guarantee of which questions will appear in the examination. If short on time, prioritise **Sections 3–7, 10–11 and 13**, then review the architecture differences in Section 8.
