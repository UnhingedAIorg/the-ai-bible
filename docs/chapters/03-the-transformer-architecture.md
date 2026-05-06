# Chapter 3: The Transformer Architecture

## The Breakthrough That Changed Everything

In 2017, Google researchers published a paper titled **"Attention Is All You Need"**.

This paper introduced the **Transformer** — the architecture that powers almost all modern AI (ChatGPT, Claude, Grok, Gemini, Llama, etc.).

Before Transformers: Recurrent Neural Networks (RNNs) were slow and couldn't handle long text well.
After Transformers: AI could understand context across entire documents and generate coherent long-form content.

## Core Idea: Attention

The key innovation is **self-attention** — the model looks at every word in a sentence at the same time and decides which words are most important for understanding each other.

Example:
> "The animal didn't cross the street because it was too tired."

The model figures out that "it" refers to "the animal", not "the street" — using attention.

## High-Level Architecture

A Transformer has two main parts:

1. **Encoder** — Understands the input
2. **Decoder** — Generates the output

Modern LLMs are mostly **Decoder-only** Transformers (like GPT series).

## Simple Code Walkthrough (PyTorch)

Here's a minimal example of how attention works:

```python
import torch
import torch.nn as nn

class SimpleSelfAttention(nn.Module):
    def __init__(self, embed_dim):
        super().__init__()
        self.query = nn.Linear(embed_dim, embed_dim)
        self.key = nn.Linear(embed_dim, embed_dim)
        self.value = nn.Linear(embed_dim, embed_dim)
        
    def forward(self, x):
        Q = self.query(x)
        K = self.key(x)
        V = self.value(x)
        
        # Attention scores
        scores = torch.matmul(Q, K.transpose(-2, -1)) / (embed_dim ** 0.5)
        attn_weights = torch.softmax(scores, dim=-1)
        
        # Output
        output = torch.matmul(attn_weights, V)
        return output
```

This is the heart of the Transformer. Everything else (multi-head attention, feed-forward layers, positional encoding) builds on this idea.

## Why Transformers Are So Powerful

- Can process entire sequences in parallel (much faster)
- Excellent at long-range dependencies
- Scales extremely well with more data and compute

**This is why we have ChatGPT today.**