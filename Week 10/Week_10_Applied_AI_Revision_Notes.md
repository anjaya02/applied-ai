# Week 10 — Applied AI Exam Revision Notes

**Module:** 6COSC020W — Applied AI  
**Lecture:** Generative AI and Large Language Models (LLMs)  
**Purpose:** Exam-night revision | MCQ-focused | Approx. 5–10 minutes

> Focus on definitions, differences and exam traps. LLMs are expected to contribute only 1–2 MCQs, but this is not guaranteed. Detailed mathematical architectures and historical examples are deliberately deprioritised.

---

## 1. Generative AI and LLMs ★★★

- **Generative AI:** Learns patterns from existing data to generate new content (text, images, audio, video, code).
- **Discriminative models:** Learn to distinguish classes or predict labels; e.g. classify an image as cat or dog.
- **LLM:** Large Language Model.
- **GPT:** Generative Pre-trained Transformer.
- GPT models are **Transformer-based**. ChatGPT is an example of **Generative AI**.
- Generative AI is not a single architecture; GANs, VAEs, Transformers and diffusion models are different model families.
- Generative LLMs predict the **next token** using the preceding context. They do not simply retrieve stored answers.

### How an LLM generates text (slides 15–42)

```text
Input text
    ↓
Tokenisation
    ↓
Embedding + positional information
    ↓
Transformer blocks (self-attention)
    ↓
Softmax → next-token probabilities
    ↓
Select next token → append to context → repeat
```

Example: `I want to drink ...` → water (60%), coffee (25%), tea (10%), milk (5%). The model selects a token from its distribution and repeats. Sampling can produce different answers to the same prompt; the highest-probability token is not always selected.

| Term | Exam definition |
|---|---|
| Tokenisation | Splitting text into tokens: words, word parts, punctuation, etc. |
| Token ID | Numerical identifier of a token in the vocabulary. |
| Embedding | Learned numerical vector representing a token. |
| Positional encoding | Provides information about token order. |
| Self-attention | Allows tokens to use relevant information from other tokens in the sequence. |
| Softmax | Converts model scores into probabilities that sum to 1. |
| Context window | Amount of token information the model can process in its context. |

**Your common mistake:** Positional encoding = **order**; self-attention = **relationships**. `Dog bites man` and `Man bites dog` illustrate why positional information matters.

### Small calculation backup (slides 31, 34)

- Dot product: multiply corresponding vector components, then add. `(0.9, 0.8) · (0.8, 0.9) = 0.72 + 0.72 = 1.44`. Used in attention to compute scores; magnitude also affects the dot product.
- Illustrative positional encoding: `(0.8, 0.9, 0.1) + (0, 0.1, 0) = (0.8, 1.0, 0.1)`.
- No need to memorise the example vectors.

---

## 2. Training and fine-tuning ★★★ (slides 41–49)

| Term | What it does |
|---|---|
| Pretraining | Initial training on huge text collections, typically by predicting the next token. |
| Fine-tuning | Further training a pretrained model on specific data to adapt its behaviour or performance. |
| Transfer learning | Reusing knowledge from an existing trained model instead of training from scratch. |
| Instruction tuning | Trains the model on instruction–response examples to follow instructions. |
| Preference tuning / RLHF | Encourages responses aligned with human preferences. RLHF = Reinforcement Learning from Human Feedback. |
| Inference | Using a trained model to produce an output, rather than training it. |
| Prompt engineering | Improving the input instructions without directly updating model weights. |

Training loop: **Predict → Compare with actual next token (loss) → Adjust weights using backpropagation/gradient descent → Repeat.**

**Your past-paper mistake:** Fine-tuning is NOT merely “finessing the model's parameters to optimise its output.” The best MCQ definition is **continue training a pretrained model on a specific dataset for a specialised task**.

Normal chat interactions change the context, not the model's weights in real time. LLMs do not require explicit programming for every new task or language.

---

## 3. LLM limitations ★★★ (slides 51–62)

| Limitation | Meaning |
|---|---|
| Hallucination | Plausible-sounding but false or fabricated information. |
| Bias | Training data and design choices can lead to biased outputs. |
| Knowledge cut-off | Later events may be unknown unless external information is provided. |
| Context limits | Finite token window; long inputs may be handled imperfectly. |
| Prompt sensitivity | Different wording can produce different answers. |
| Sycophancy | Tendency to agree with the user even when the user is wrong. |
| Reasoning limits | Can make logical, mathematical or letter-counting mistakes. |

**Why hallucinations happen:** LLMs learn plausible next-token predictions, not a guarantee of truth.

**Grounding:** Provide external information (e.g. retrieved documents or web results) in the context to improve factual accuracy. It does not guarantee correctness.

Verify important claims using primary and independent sources; check dates and whether the model could know the information.

---

## 4. Image generation model families ★★★ (slides 64–68)

| Model | Key mechanism |
|---|---|
| GAN — Generative Adversarial Network | Generator and discriminator compete. |
| VAE — Variational Autoencoder | Encoder, probabilistic latent space, decoder. |
| Diffusion model | Progressive noising and learned denoising. |
| Transformer | Attention over sequences. |

### GAN (previously examined)

- **Generator:** Creates synthetic data from random noise.
- **Discriminator:** Tries to distinguish real data from generated data.
- The generator improves using **feedback from the discriminator**.

### VAE

- Encoder maps input into a probabilistic latent representation.
- Decoder reconstructs data during training.
- Generate new data by sampling a latent vector and decoding it.

### Diffusion

- **Forward process:** Gradually adds Gaussian noise to a real image.
- **Reverse process:** Learns to remove noise progressively; generation starts from noise.

Memory trick: **GAN = competition; VAE = encode/decode; Diffusion = denoise; Transformer = attention.**

---

## 5. Prompt engineering and responsible AI ★★ (slides 47, 78–86)

- Provide clear instructions and relevant context; separate unrelated topics and break complex instructions into manageable steps.
- **Human-AI Teaming (HAIT):** Humans and AI combine their capabilities to achieve shared goals.
- Main ethical risks: **bias, privacy, copyright, academic integrity, energy consumption and over-reliance**.
- Biased training data can produce biased outputs. Removing bias completely is difficult.
- Do not blindly trust fluent answers or assume that references are genuine. Verify important outputs and follow university AI-use rules.

---

## 6. Actual past-paper questions — quick answer key

Seven distinct relevant questions identified in the earlier exam-archive review (15 marks across assessments). Duplicate screenshots are not counted twice. Question stems below are retained or condensed for quick revision; answers are shown instead of full distractors.

| Source | Question | Correct answer | Marks |
|---|---|---|---:|
| 2022 PAPER, Q20 | ChatGPT is based on which type of AI? | Generative AI | 3 |
| 2022 PAPER, Q10 | GPT models used in NLP are Transformer-based. | True | 1 |
| D, Q2 | LLMs require explicit programming for each new language or task. | False | 1 |
| D, Q29 | What is fine-tuning in the context of LLMs? | Continue training on a specific dataset for a specialised task. | 3 |
| D, Q16 | What is the main limitation of LLMs in NLP tasks? | May generate plausible-sounding but factually incorrect information. | 3 |
| In-Class Test 2, Q12 | In GANs, the generator improves based on feedback from what? | Discriminator | 3 |
| In-Class Test 2, Q28 | LLMs like GPT-3 generate responses from previously learned patterns, not real-time learning during user interactions. | True | 1 |

---

## 7. Exam traps — true or false

| Statement | Answer |
|---|---|
| The same prompt can generate different answers. | TRUE |
| An LLM always selects the most probable token. | FALSE |
| Fluent, confident answers guarantee factual accuracy. | FALSE |
| Asking for sources guarantees they are genuine. | FALSE |
| Text typed into a chat directly changes model weights. | FALSE |
| Earlier text in the chat can influence later responses. | TRUE |
| Pretraining teaches generative LLMs to predict the next token. | TRUE |
| Positional encoding identifies relationships between tokens. | FALSE — self-attention |
| Fine-tuning requires additional training. | TRUE |
| Prompt engineering directly updates the model's weights. | FALSE |
| The GAN discriminator generates fake images. | FALSE — generator |
| The diffusion forward process removes noise. | FALSE — adds noise |
| Biased training data can lead to biased outputs. | TRUE |

---

## 8. Last-minute checklist

- [ ] GPT = Generative Pre-trained Transformer; ChatGPT = Generative AI.
- [ ] LLM = next-token prediction, not a database of answers.
- [ ] Tokenisation = split; embedding = vector; position = order; attention = relationships; softmax = probabilities.
- [ ] Pretraining = initial training; fine-tuning = further training; transfer learning = reuse; prompting = no retraining.
- [ ] Hallucination = plausible but false; verify even confident answers.
- [ ] GAN = generator + discriminator; VAE = encoder + latent space + decoder; diffusion = add/remove noise.
- [ ] Understand bias, privacy and responsible use.

**Source:** Week 10 lecture — *10 Applied AI - Large Language Models and Generative AI.pdf* (88 slides), and previously reviewed exam screenshots from `D(1).zip`. Past-paper coverage refers to visible questions; future exam content is not guaranteed.
