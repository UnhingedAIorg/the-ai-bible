# Chapter 8: Parameters, Training & Scale

## What Are Parameters?

**Parameters** are the "knobs and dials" inside an AI model — the actual numbers (weights and biases) that encode all its knowledge.

- Small model (like early GPT-2): ~1.5 billion parameters
- GPT-3: 175 billion parameters
- GPT-4 / Claude 3.5 / Grok-3 era: Estimated 1–2 trillion parameters
- 2026 frontier models: Often 10+ trillion parameters or more (exact numbers are usually secret)

Every parameter is a tiny piece of learned knowledge. Together they form the model's "brain."

## The Training Process

Training a large model is one of the most expensive and complex engineering projects on Earth:

1. **Data Collection**: Trillions of tokens from the internet, books, code repositories, scientific papers, etc.
2. **Preprocessing**: Cleaning, tokenization, deduplication.
3. **Pre-training**: The model predicts the next token across the entire dataset (self-supervised learning).
4. **Post-training**: Fine-tuning, RLHF/RLAIF, safety training, preference tuning.
5. **Evaluation & Iteration**

A single training run for a frontier model can cost **hundreds of millions of dollars** and take months on thousands of GPUs.

## Scaling Laws

One of the most important discoveries in AI:

**Bigger models + more data + more compute = dramatically better performance.**

This is not linear — it's often **super-linear** in certain capabilities. This is why companies are racing to build bigger and bigger models.

## Why Scale Matters

Many "emergent abilities" (sudden jumps in capability) only appear at certain scales. A 10-billion parameter model cannot do what a 100-billion parameter model can do, no matter how cleverly you prompt it.

This is why the future belongs to those who can build and train the largest, most efficient models.