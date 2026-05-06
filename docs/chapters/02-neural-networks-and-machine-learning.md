# Chapter 2: Neural Networks & Machine Learning

## What is Machine Learning?

**Machine Learning (ML)** is a subset of AI where computers learn patterns from data instead of being explicitly programmed for every task.

Traditional programming: You write rules.
Machine Learning: You show examples → the computer figures out the rules.

## Neural Networks: The Brain of Modern AI

A **neural network** is a computer system inspired by the human brain.

It consists of:

- **Neurons** (nodes) — simple mathematical units
- **Layers** — groups of neurons
- **Weights** — the strength of connections between neurons
- **Activation functions** — decide if a neuron "fires"

### Simple Analogy
Imagine a giant web of light bulbs. When you show it a picture of a cat, certain bulbs light up in a pattern that means "cat". The more cats it sees, the better it gets at recognizing them.

## How a Neural Network Learns

1. **Forward pass**: Data goes through the network → it makes a prediction.
2. **Calculate error**: How wrong was the prediction?
3. **Backward pass (Backpropagation)**: Adjust the weights to reduce error.
4. Repeat millions of times with lots of data.

This process is called **training**.

## Types of Machine Learning

- **Supervised Learning**: Learn from labeled examples ("this is a cat")
- **Unsupervised Learning**: Find hidden patterns in data
- **Reinforcement Learning**: Learn by trial and error with rewards

## Why This Matters

Almost every modern AI system (ChatGPT, Midjourney, Grok, etc.) is built on neural networks — specifically a very powerful type called the **Transformer** (next chapter).