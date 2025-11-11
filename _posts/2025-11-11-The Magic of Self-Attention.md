---
layout: post
title: The Magic of Self-Attention
---

# 🧠 Understanding “The Illustrated Transformer” — Simplified Breakdown

You picked one of the most legendary explainers of all time: **Jay Alammar’s _The Illustrated Transformer_**.  
It’s the foundation of how every modern LLM (like GPT-5) works.  
Let’s rebuild your understanding from zero → to “I can explain self-attention and Transformers to anyone.”

---

## 🧩 1. Starting Point — What’s the Problem?

Old models like **RNNs** or **LSTMs** read sentences **one word at a time**, remembering the past in a “hidden state.”  
That’s slow and limited — they forget long-range context.

Transformers fixed that.  
They read **all words at once**, then **decide which words should pay attention to which others.**

That magic is called **Self-Attention**.

---

## ⚙️ 2. The Core Idea — Self-Attention

When the model reads a sentence like:

> “The animal didn’t cross the street because it was too tired.”

It must figure out what “it” refers to.  
Self-attention helps the model “look around” the sentence and realize “it” means “the animal,” not “the street.”

So when processing each word, the model considers *all other words* and assigns each one a score of how relevant it is.

---

## 🧠 3. The Ingredients — Q, K, V (Query, Key, Value)

Every word (after turning into an **embedding**, a vector of numbers representing its meaning) is used to make **three smaller vectors**:

| Role | Formula | Meaning |
|------|----------|---------|
| Query (Q) | Q = X × W<sup>Q</sup> | What this word is looking for |
| Key (K) | K = X × W<sup>K</sup> | What this word offers |
| Value (V) | V = X × W<sup>V</sup> | The actual info it carries |

Think of:
- **Query** = a question a word asks (“Who am I connected to?”)
- **Key** = a tag saying “I represent a cat / a subject / a verb”
- **Value** = the meaning you pass if someone pays attention to you.

---

## 🔢 4. The Math of “Paying Attention”

Each word compares its Query (Q) with all the other Keys (K):

\[
\text{score} = Q \cdot K^T
\]

→ The higher the dot product, the more similar the word meanings — the more attention one pays to the other.

Then we:
1. **Scale down** the scores by dividing by √(key dimension), e.g., √64 = 8.  
   (stabilizes the math)
2. **Softmax** them → turn scores into probabilities that sum to 1.  
   (how much of your attention to give each word)
3. **Multiply** each Value (V) by these attention weights.  
   (focus more on relevant words)
4. **Sum** all of them to get the **output** vector — a new meaning for the word that now includes info from others.

---

## 🧮 5. Doing It for a Whole Sentence (Matrix Version)

If you have 5 words:
- Pack all word embeddings into a matrix **X**.
- Multiply by **W<sup>Q</sup>**, **W<sup>K</sup>**, **W<sup>V</sup>** to get matrices **Q**, **K**, and **V**.

Then compute:

\[
\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V
\]

This one line is **the core equation of self-attention** — used in every Transformer, from GPT-2 to GPT-5.

---

## 👑 6. Multi-Headed Attention

One attention mechanism might focus only on one kind of relation (like subject-verb).  
But we need multiple “angles of understanding.”

So the model runs **8 or more attention heads** — each has its own W<sup>Q</sup>, W<sup>K</sup>, W<sup>V</sup> — to learn different types of relationships:
- one head might track who does what,
- another focuses on adjectives,
- another on long-distance dependencies.

Then they’re all combined:

\[
\text{MultiHead}(Q,K,V) = \text{Concat}(\text{head}_1,...,\text{head}_h)W^O
\]

That’s how the model can “think” from multiple perspectives at once.

---

## 🧩 7. Positional Encoding — Order Matters

Transformers read all tokens simultaneously — no natural order like RNNs.  
So we add **Positional Encodings** to tell the model *where* each word sits.

These are sinusoidal (sine/cosine) wave patterns that encode position as continuous signals:
\[
PE_{(pos, 2i)} = \sin(pos / 10000^{2i/d_{model}})
\]
\[
PE_{(pos, 2i+1)} = \cos(pos / 10000^{2i/d_{model}})
\]
This gives each position a unique pattern and preserves relative distance.

---

## 🧱 8. Encoder and Decoder Blocks

- **Encoder**:
  - Self-Attention → Feed-Forward → Output
  - Each layer learns deeper relationships.

- **Decoder**:
  - Has Self-Attention too, **but masked** (so it can’t peek at future words).
  - Also attends to encoder output (so it can focus on the relevant parts of the input sentence).

---

## 🧮 9. Final Linear + Softmax

After decoding, the model outputs a vector of size equal to its **vocabulary** (say, 50,000 words).  
Softmax turns those raw numbers into probabilities.  
The word with the highest probability is chosen.

That’s how text is generated, one token at a time.

---

## 🧠 10. Training and Loss Function

During training:
- The model predicts the next word.
- We compare its prediction to the correct answer using **cross-entropy loss**.
- Then adjust weights via **backpropagation** to reduce the error.

Over millions of examples, it learns language structure and meaning.

---

## 🏗️ 11. Key Intuitions

| Concept | What It Does | Analogy |
|----------|---------------|----------|
| **Embedding** | Turns words into numbers with meaning | Colors in a palette |
| **Self-Attention** | Decides who to listen to | Conversation in a group |
| **Multi-Head** | Sees from multiple perspectives | Different experts in a meeting |
| **Positional Encoding** | Keeps track of order | Page numbers in a book |
| **Feed-Forward** | Refines info per token | Each person thinks individually |
| **Residuals** | Keep the original idea while refining | Helps deep networks learn |
| **Softmax** | Turns scores into probabilities | Voting |
| **Loss** | Penalizes wrong predictions | Teacher grading a test |
| **Backprop** | Adjusts weights to improve | Correcting your aim after each throw |

---

## 🧩 12. How This Scales to LLMs

| Mini Transformer | Modern LLM |
|------------------|------------|
| 2–6 layers | 70–100+ layers |
| 512-dim embeddings | 4096–16384 dims |
| 8 heads | 32–128 heads |
| Single GPU | Thousands of GPUs |
| Megabytes of text | Trillions of tokens |

But the math and ideas are **identical.**

---

## 🧩 13. Visual Summary

To visualize:
- Each word is like a node that shines light toward other nodes.
- Attention weights decide how bright each connection is.
- Multi-headed attention is like shining light in different colors — each highlights different relationships.

---
