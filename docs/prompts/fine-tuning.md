# Fine-Tuning, LoRAs & Model Sovereignty Prompts

**Make the model yours.**

This library contains **100+ prompts** for dataset creation, fine-tuning strategy, LoRA training, evaluation, and deployment of sovereign models.

---

## Dataset Engineering

### 1. High-Quality Instruction Dataset Generator
Generate 500 diverse, high-quality instruction-response pairs in the style of [expert/domain]. Include edge cases, multi-turn conversations, and adversarial examples. Output as JSONL format with fields: instruction, input, output, system_prompt (optional).

### 2. Style Transfer Dataset
Create a dataset that teaches a model to write exactly like [author/style]. Provide 50 examples of source text and target transformation. Include variations in tone, vocabulary patterns, sentence structure, and characteristic expressions unique to that style.

### 3. Domain Adaptation Dataset Generator
Generate training data to adapt a general model to [specific domain: legal/medical/finance/technical]. Create 1000 examples covering: domain terminology, common tasks, typical queries, edge cases, and regulatory considerations. Balance breadth and depth.

### 4. Synthetic Data Generator with Constraints
Generate synthetic training data for [task] with these constraints: [accuracy requirements, diversity needs, bias considerations]. Use self-instruction techniques: seed with human-written examples, generate variations, filter by quality, validate against constraints.

### 5. Preference Data Generator (RLHF/DPO)
Create preference pairs for [task type]. For each prompt, generate 2-4 responses with varying quality. Then rank or score them on: helpfulness, accuracy, safety, style. Output format: prompt, chosen (best), rejected (worse), ranking/scores.

### 6. Multi-Turn Conversation Dataset
Generate 200 multi-turn conversation examples for [use case]. Include: context maintenance across turns, natural turn-taking, handling clarifications, recovering from misunderstandings, and graceful endings. Vary conversation lengths from 2-10 turns.

### 7. Code Dataset Generator
Generate programming dataset for [language/framework]. Include: function implementations, bug fixes, code explanations, refactoring examples, test generation, and documentation. Cover easy to hard difficulty levels with clear problem specifications.

### 8. Safety Alignment Dataset
Create safety training examples covering: harmful request refusal, honest uncertainty expression, avoiding stereotypes, maintaining neutrality on polarizing topics, and graceful handling of adversarial prompts. Balance safety with helpfulness.

### 9. Tool-Use Dataset Generator
Generate training data for tool-using agents. Include: when to use tools, correct tool selection, proper parameter formatting, handling tool errors, chaining multiple tools, and knowing when NOT to use tools. Include function calling formats.

### 10. Function Calling Dataset
Create dataset for function calling training. Format: system message (available functions), user message (request), assistant response (function call with parameters or direct answer). Cover: single calls, multiple calls, parallel calls, chained calls.

---

## Fine-Tuning Strategy

### 11. Full Fine-Tuning vs LoRA Decision Framework
Analyze [use case] to recommend approach:
- **Full fine-tuning**: Large dataset (>100k), major behavior change, compute available
- **LoRA**: Smaller dataset, specific task adaptation, limited compute, faster iteration
- **QLoRA**: Ultra-limited VRAM, acceptable slight quality trade-off
- Provide reasoning and expected outcomes.

### 12. Learning Rate Scheduler Design
Design learning rate schedule for [model size, dataset size]:
1. Warmup: Linear increase to peak over first [X]% of steps
2. Main phase: Cosine decay or constant
3. Peak LR: [value based on model/dataset]
4. Min LR: [fraction of peak]
5. Justify choices based on convergence patterns.

### 13. Epoch vs Steps Optimization
Determine optimal training duration for [dataset size, model]:
- Calculate total tokens
- Estimate convergence point
- Consider: overfitting risks, checkpoint evaluation frequency, early stopping criteria
- Recommendation: number of epochs or max steps with validation checkpoints.

### 14. Batch Size Selection
Recommend batch size and gradient accumulation for [hardware specs, model size]:
- Calculate memory constraints
- Determine optimal batch size for GPU utilization
- Set gradient accumulation steps to achieve effective batch size
- Consider: throughput, gradient noise, convergence stability

### 15. Instruction Template Design
Design chat template for fine-tuning:
```
<|system|> {system_message}
<|user|> {user_message}
<|assistant|> {assistant_response}
```
Or choose: Alpaca, Vicuna, ChatML, Llama-2/3 format. Explain choice based on target inference platform.

### 16. Data Mixing Strategy
Design data mixture for balanced fine-tuning:
- Proportions: [task A: X%, task B: Y%, general: Z%]
- Upsampling rare but important examples
- Downsampling overrepresented categories
- Curriculum learning: easy → hard sequencing
- Continuous monitoring of data distribution

### 17. Continual Pre-training Prompts
For domain-specific continual pre-training on [domain corpus]:
1. Data preprocessing: deduplication, quality filtering
2. Tokenization verification
3. Next-token prediction setup
4. Learning rate: typically 10-100x smaller than pre-training
5. Checkpointing and evaluation cadence
6. Catastrophic forgetting mitigation strategies

### 18. Multi-Task Fine-Tuning Setup
Configure multi-task fine-tuning for [list of tasks]:
1. Task-specific prompt prefixes
2. Balanced sampling across tasks
3. Task-specific loss weighting (if needed)
4. Evaluation on each task
5. Identify task conflicts and resolutions
6. Final merged model testing

---

## LoRA (Low-Rank Adaptation)

### 19. LoRA Rank Selection
Recommend LoRA rank for [use case]:
- **Rank 4-8**: Simple tasks, limited compute, quick iteration
- **Rank 16-32**: Standard tasks, good quality/speed balance
- **Rank 64-128**: Complex tasks, maximum quality
- **Rank 256+**: Diminishing returns, only for specific needs
- Consider: target modules, alpha scaling, trainable params

### 20. LoRA Target Module Selection
Select which modules to apply LoRA for [model architecture]:
- **Q, V attention**: Standard, most parameter efficient
- **Q, K, V, O**: More expressive, more parameters
- **All linear**: Maximum adaptation, high parameter count
- **Specific layers**: Layer-wise analysis for optimal targets
- Trade-off analysis included.

### 21. QLoRA 4-bit Configuration
Configure QLoRA training for [model size, VRAM]:
```python
# BitsAndBytes Config
bnb_config = {
    "load_in_4bit": True,
    "bnb_4bit_use_double_quant": True,
    "bnb_4bit_quant_type": "nf4",
    "bnb_4bit_compute_dtype": "bfloat16"
}
```
Plus LoRA config, training arguments, and memory optimization tips.

### 22. LoRA Merging Strategy
Merge LoRA adapters into base model:
1. Weighted merging: merge multiple task-specific LoRAs
2. Task arithmetic: add/subtract LoRA directions
3. SVD reconstruction for rank reduction
4. Validation of merged model
5. Export to standard format (GGUF, ONNX, etc.)

### 23. DoRA (Weight-Decomposed LoRA)
Configure DoRA for higher quality:
- Decomposes weights into magnitude and direction
- Applies LoRA only to direction component
- Better fine-grained control
- Slightly more parameters but often better quality
- Configuration and training setup.

### 24. Multi-LoRA Composition
Compose multiple LoRAs:
1. **Sequential**: Apply LoRA A, then LoRA B
2. **Weighted sum**: alpha*LoRA_A + (1-alpha)*LoRA_B
3. **Switching**: Route to different LoRAs by task
4. **Attention-based**: Blend dynamically based on input
5. Evaluation of composition strategies

---

## Training Execution

### 25. Distributed Training Setup
Configure multi-GPU training:
1. **Data Parallel**: Replicate model, split batch (simpler)
2. **Tensor Parallel**: Split model layers (large models)
3. **Pipeline Parallel**: Split model vertically
4. **ZeRO**: Optimize memory with optimizer state sharding
5. **FSDP**: Fully Sharded Data Parallel (PyTorch native)
6. Configuration code and launch commands.

### 26. Checkpointing Strategy
Implement robust checkpointing:
1. Save frequency: every N steps or minutes
2. Save components: model weights, optimizer state, RNG state, training config
3. Resume from checkpoint: restore full training state
4. Best model tracking: validation metric-based
5. Cleanup: manage disk space, keep N recent + best

### 27. Training Monitoring Setup
Configure training monitoring:
1. **WandB/TensorBoard**: Loss curves, learning rate, gradients
2. **Custom metrics**: Perplexity, task-specific scores
3. **Sample generation**: Periodic qualitative checks
4. **Alerts**: Loss spikes, NaN detection, disk space
5. **Resource monitoring**: GPU utilization, memory, throughput

### 28. Gradient Clipping Configuration
Set up gradient clipping:
- **Value clipping**: max_grad_norm = 1.0 (standard)
- **When to use**: Prevent exploding gradients, stabilize large batch training
- **Monitoring**: Track gradient norms over time
- **Adaptive**: Adjust based on training stability
- Configuration for different scenarios.

### 29. Mixed Precision Training
Configure AMP (Automatic Mixed Precision):
1. **FP16**: Standard, 2x speedup on V100+
2. **BF16**: Better stability, recommended for A100+
3. **FP8**: H100 only, maximum speed
4. **GradScaler**: Handle loss scaling
5. **Loss scaling**: Dynamic or fixed
6. Configuration code and fallback strategies.

### 30. Flash Attention Integration
Integrate Flash Attention 2 for speed:
1. Installation: `pip install flash-attn --no-build-isolation`
2. Model configuration: `attn_implementation="flash_attention_2"`
3. Memory savings: ~20-40% reduction
4. Speed gains: 2-4x on long sequences
5. Compatibility checks and fallbacks.

---

## Evaluation & Validation

### 31. Perplexity Evaluation
Evaluate fine-tuned model with perplexity:
```python
# Calculate on held-out validation set
total_loss = 0
total_tokens = 0
for batch in eval_loader:
    outputs = model(**batch)
    total_loss += outputs.loss.item() * batch_tokens
    total_tokens += batch_tokens

perplexity = exp(total_loss / total_tokens)
```
Plus interpretation of results and comparison to baseline.

### 32. Task-Specific Benchmark Evaluation
Evaluate on relevant benchmarks:
- **General**: MMLU, HellaSwag, ARC, TruthfulQA
- **Code**: HumanEval, MBPP
- **Chat**: MT-Bench, AlpacaEval
- **Safety**: StrongREJECT, HarmBench
- Custom benchmarks for domain-specific tasks

### 33. Human Evaluation Protocol
Conduct human evaluation:
1. Define evaluation criteria (helpfulness, accuracy, tone)
2. Create evaluation rubric with examples
3. Recruit evaluators with domain knowledge
4. Blind comparison vs. baseline
5. Inter-annotator agreement calculation
6. Statistical significance testing

### 34. A/B Testing Framework
Production A/B testing:
1. Shadow deployment: route traffic to both, only log new model
2. Canary deployment: 1% → 5% → 25% → 100%
3. Metrics: latency, error rates, user satisfaction
4. Rollback criteria: automatic on error threshold
5. Long-term monitoring for drift

### 35. Bias & Fairness Evaluation
Evaluate for biases:
1. **Demographic parity**: Equal performance across groups
2. **Stereotype detection**: BBQ, StereoSet benchmarks
3. **Toxicity**: Perspective API, custom classifiers
4. **Representation**: Analyze training data distribution
5. **Mitigation**: Fine-tuning on balanced data

---

## Deployment & Production

### 36. GGUF Quantization for Local Deployment
Quantize to GGUF format:
```bash
# Convert to GGUF
python convert_hf_to_gguf.py --model path/to/model --outfile model.gguf

# Quantization levels:
# Q4_K_M: Balanced quality/size
# Q5_K_M: Better quality
# Q8_0: Near-lossless
# F16: No quantization
```
Plus llama.cpp inference commands and speed benchmarks.

### 37. vLLM Deployment Setup
Deploy with vLLM for throughput:
```python
from vllm import LLM, SamplingParams

llm = LLM(
    model="path/to/model",
    tensor_parallel_size=2,
    gpu_memory_utilization=0.9
)

# Continuous batching for high throughput
# PagedAttention for memory efficiency
```
Plus API server setup and benchmarking.

### 38. Ollama Integration
Package for Ollama:
1. Create Modelfile:
```
FROM ./model.gguf
PARAMETER temperature 0.7
PARAMETER top_p 0.9
SYSTEM """You are a helpful assistant..."""
```
2. Build: `ollama create mymodel -f Modelfile`
3. Run: `ollama run mymodel`
4. Push to registry for sharing

### 39. API Server Deployment
Deploy as REST API:
```python
from fastapi import FastAPI
from transformers import pipeline

app = FastAPI()
generator = pipeline("text-generation", model="path/to/model")

@app.post("/generate")
def generate(prompt: str, max_tokens: int = 512):
    return generator(prompt, max_new_tokens=max_tokens)
```
Plus Docker containerization and scaling considerations.

### 40. Edge Deployment Optimization
Optimize for edge devices:
1. **Quantization**: INT8 or INT4
2. **Pruning**: Remove less important weights
3. **Knowledge distillation**: Smaller student model
4. **ONNX Runtime**: Cross-platform optimization
5. **Mobile frameworks**: CoreML, TensorFlow Lite

---

## Model Merging & Ensembling

### 41. Task Arithmetic for Model Editing
Merge models with task arithmetic:
```python
# Model merging: θ_merged = θ_base + λ * (θ_task - θ_base)
# Add capabilities: θ_base + 0.3 * (θ_coding - θ_base)
# Remove capabilities: θ_base - 0.5 * (θ_toxic - θ_base)
```
Plus SLERP, TIES, and DARE merging techniques.

### 42. SLERP (Spherical Linear Interpolation)
Merge with SLERP for smooth interpolation:
```python
# Better than linear interpolation
# Interpolates on hypersphere
# Preserves vector relationships
# Configurable interpolation coefficient
```
Implementation and when to use vs. linear merging.

### 43. Model Soups (Weight Averaging)
Create model soup:
1. Fine-tune multiple models with different hyperparameters
2. Average their weights: θ_soup = mean(θ_1, θ_2, ... θ_n)
3. Often outperforms individual models
4. Especially effective with varying data orders
5. Simple to implement, impressive results

### 44. Frankenmerging (Layer-wise)
Mix layers from different models:
1. Take base from model A (bottom layers)
2. Take middle from model B
3. Take top from model C
4. Experiment with splice points
5. Evaluate emergent capabilities

---

## Advanced Techniques

### 45. Parameter-Efficient Fine-Tuning (PEFT) Comparison
Compare PEFT methods for [use case]:
- **LoRA**: Most popular, good quality
- **Prefix Tuning**: Task-specific prefixes
- **P-Tuning**: Soft prompts
- **IA³**: Learned scaling vectors
- **Adapter**: Small bottleneck layers
- Selection criteria and trade-offs.

### 46. Reinforcement Learning from Human Feedback (RLHF)
Implement RLHF pipeline:
1. **SFT**: Supervised fine-tuning on demonstrations
2. **Reward Model**: Train to score responses
3. **PPO**: Optimize policy against reward model
4. **KL Penalty**: Prevent policy drift
5. **Rejection Sampling**: Alternative to PPO
6. Full pipeline with code examples.

### 47. Direct Preference Optimization (DPO)
Simpler alternative to RLHF:
```python
# No reward model needed!
# Directly optimizes from preference pairs
# Often matches or exceeds RLHF quality
# Simpler, more stable training
```
Implementation details and comparison to RLHF.

### 48. Constitutional AI (CAI)
Implement self-improvement via principles:
1. Define constitutional principles (helpfulness, harmlessness, honesty)
2. Generate responses
3. Critique responses against principles
4. Revise based on critique
5. Train on revised responses
6. Iterate for refinement

### 49. Instruction Backtranslation
Generate training data via backtranslation:
1. Seed with high-quality outputs
2. Generate instructions that would produce those outputs
3. Filter for quality and diversity
4. Use synthetic pairs for training
5. Iterative refinement
6. Cost-effective data scaling

### 50. Self-Play Fine-Tuning
Generate and learn from self-play:
1. Model generates problem-solution pairs
2. Another instance critiques and improves
3. Filter for high-quality examples
4. Fine-tune on self-generated data
5. Iterative capability improvement
6. Especially effective for reasoning

---

## Sovereign AI & Open Source

### 51. Local Model Setup (Fully Offline)
Deploy completely offline:
1. Download weights via torrent/IPFS (if needed)
2. Air-gapped inference machine
3. Local document processing
4. No telemetry or data leakage
5. Verified supply chain
6. Emergency fallback procedures

### 52. Self-Hosted Model Registry
Manage private model repository:
1. **Git LFS**: Version control for models
2. **DVC**: Data version control integration
3. **Private HuggingFace Hub**: Self-hosted
4. **S3-compatible**: MinIO, etc.
5. Access control and audit logging

### 53. Federated Fine-Tuning
Collaborative training without centralizing data:
1. Train locally on private data
2. Share only gradient updates (secure aggregation)
3. Central server aggregates updates
4. Differential privacy protections
5. No raw data leaves organization

### 54. Model Verification
Verify model integrity:
1. **Hash verification**: SHA-256 of weights
2. **Signature checking**: GPG signed models
3. **Supply chain**: SBOM for dependencies
4. **Provenance**: Training data lineage
5. **Behavioral testing**: Canary tests for backdoors

### 55. Catastrophic Forgetting Mitigation
Prevent losing general capabilities:
1. **Replay buffer**: Mix general data during fine-tuning
2. **Elastic Weight Consolidation**: Protect important weights
3. **Progressive learning**: Gradual introduction of new data
4. **Multi-task**: Train on old + new tasks simultaneously
5. **Adapter-based**: Keep base frozen, adapt through LoRA

---

## Specialized Fine-Tuning

### 56. Code Model Fine-Tuning
Fine-tune for code generation:
1. **Data**: The Stack, GitHub repos, CodeSearchNet
2. **Languages**: Target specific language(s)
3. **FIM (Fill-in-Middle)**: Span corruption training
4. **Long context**: Repository-level context
5. **Tool use**: Function calling for IDEs
6. **Evaluation**: HumanEval, MBPP, custom tests

### 57. Math & Reasoning Fine-Tuning
Improve mathematical capabilities:
1. **Data**: GSM8K, MATH dataset, synthetic problems
2. **Chain-of-thought**: Include reasoning steps
3. **Tool use**: Calculator, symbolic solver
4. **Verification**: Self-check answers
5. **Curriculum**: Easy → medium → hard problems
6. **Format**: Clear step-by-step solutions

### 58. Medical Domain Fine-Tuning
Adapt for healthcare (non-diagnostic):
1. **Data**: Medical textbooks, guidelines, research
2. **Safety**: Clear non-medical-advice disclaimers
3. **Terminology**: Standardized medical vocabulary
4. **Evidence-based**: Cite sources, acknowledge uncertainty
5. **Red teaming**: Test for dangerous advice
6. **Compliance**: HIPAA considerations for training data

### 59. Legal Domain Fine-Tuning
Adapt for legal assistance:
1. **Data**: Case law, statutes, contracts, legal writing
2. **Jurisdiction**: Specific to legal system
3. **Citations**: Proper legal citation format
4. **Disclaimer**: Not legal advice, consult attorney
5. **Confidentiality**: No training on privileged info
6. **Updates**: Track law changes over time

### 60. Creative Writing Fine-Tuning
Train for creative tasks:
1. **Data**: Fiction, poetry, screenplays, diverse genres
2. **Style preservation**: Maintain author voice
3. **Constraints**: Format adherence (haiku, sonnet)
4. **Continuation**: Story completion quality
5. **Character consistency**: Personality maintenance
6. **Evaluation**: Human preference, not automated metrics

---

## Debugging & Troubleshooting

### 61. Loss Spike Diagnosis
Diagnose training loss spikes:
1. Check for bad data batches
2. Verify learning rate not too high
3. Examine gradient norms before spike
4. Check for numerical instability (NaN/Inf)
5. Review recent data distribution changes
6. Implement gradient clipping if needed

### 62. Overfitting Detection & Remedies
Address overfitting:
1. **Symptoms**: Train loss ↓, val loss ↑, generation quality ↓
2. **Regularization**: Dropout, weight decay
3. **Early stopping**: Patience, best checkpoint
4. **Data augmentation**: More diverse training data
5. **Simplification**: Reduce model capacity (LoRA rank)
6. **Ensembling**: Average multiple checkpoints

### 63. Underfitting Diagnosis
Fix underfitting:
1. **Symptoms**: Both train and val loss high
2. **More capacity**: Increase LoRA rank or full fine-tune
3. **More data**: Collect additional training examples
4. **Longer training**: Increase epochs
5. **Higher learning rate**: Less conservative schedule
6. **Better initialization**: Pre-trained checkpoints

### 64. Memory Optimization
Fit larger models in limited VRAM:
1. **Gradient checkpointing**: Trade compute for memory
2. **LoRA**: Reduce trainable parameters
3. **QLoRA**: 4-bit base model
4. **DeepSpeed ZeRO**: Shard optimizer states
5. **Offloading**: CPU offloading for optimizer
6. **Batch size**: Reduce with gradient accumulation

### 65. Slow Training Diagnosis
Speed up training:
1. **Data loading**: Num workers, prefetch, caching
2. **GPU utilization**: Check if 100%, adjust batch size
3. **Mixed precision**: FP16/BF16
4. **Flash Attention**: Memory-efficient attention
5. **Compilation**: torch.compile() for PyTorch 2.0+
6. **Distributed**: Multi-GPU parallelization

---

## Best Practices

### 66. Data Quality Checklist
Verify training data quality:
- [ ] Remove duplicates (exact and near-duplicate)
- [ ] Filter low-quality examples (length, coherence)
- [ ] Check for toxic/harmful content
- [ ] Balance categories and topics
- [ ] Validate format consistency
- [ ] Include diverse authors/styles
- [ ] Add variety in prompt phrasing
- [ ] Test on held-out examples

### 67. Reproducibility Checklist
Ensure reproducible training:
- [ ] Fix random seeds (Python, NumPy, PyTorch, CUDA)
- [ ] Pin all dependency versions
- [ ] Document exact hardware/software
- [ ] Version control training code
- [ ] Log all hyperparameters
- [ ] Save full training config with model
- [ ] Record dataset version/provenance
- [ ] Document any manual steps

### 68. Evaluation Checklist
Comprehensive evaluation:
- [ ] Perplexity on held-out text
- [ ] Task-specific benchmarks
- [ ] Human evaluation (blind comparison)
- [ ] Safety/red-teaming tests
- [ ] Long-tail query testing
- [ ] Edge case handling
- [ ] Performance/latency benchmarks
- [ ] A/B test in production

### 69. Documentation Template
Document fine-tuned model:
```
# Model Card
- Base model: [name]
- Fine-tuning method: [full/LoRA/QLoRA]
- Dataset: [source, size, characteristics]
- Training duration: [steps/time]
- Hardware: [GPUs, memory]
- Performance: [benchmarks, metrics]
- Known limitations: [issues]
- Intended use: [use cases]
- Safety considerations: [warnings]
```

### 70. Model Versioning Strategy
Version control for models:
1. **Semantic versioning**: MAJOR.MINOR.PATCH
2. **Git LFS**: Track large files
3. **DVC**: Data and model versioning
4. **MLflow/Weights & Biases**: Experiment tracking
5. **Model registry**: Staging → production
6. **Rollback plan**: Keep previous versions

---

## Tools & Ecosystem

### 71. HuggingFace Ecosystem Setup
Use HF tools effectively:
1. **Transformers**: Model loading, tokenizers
2. **Datasets**: Data processing and loading
3. **Tokenizers**: Fast tokenization
4. **Accelerate**: Multi-GPU training
5. **PEFT**: LoRA, adapters
6. **TRL**: RLHF, DPO training
7. **Optimum**: Optimization and quantization

### 72. Unsloth for Faster Training
Use Unsloth for 2x+ speed:
```python
from unsloth import FastLanguageModel

model, tokenizer = FastLanguageModel.from_pretrained(
    model_name="unsloth/llama-3-8b",
    max_seq_length=2048,
    load_in_4bit=True
)

# 2x faster, 70% less memory
# Compatible with HuggingFace ecosystem
```

### 73. Axolotl Configuration
Simplify training with Axolotl:
```yaml
# config.yaml
base_model: meta-llama/Llama-2-7b-hf
adapter: lora
datasets:
  - path: data.jsonl
target_modules: [q_proj, v_proj]
learning_rate: 0.0002
```
Single command training with YAML configs.

### 74. Llama-Factory GUI
Use Llama-Factory web UI:
1. Visual dataset preparation
2. Point-and-click training configuration
3. Real-time training monitoring
4. Model export and quantization
5. Chat interface for testing
6. No code required for basic fine-tuning

### 75. Text Generation Inference (TGI)
Deploy with HuggingFace TGI:
```bash
docker run --gpus all \
  -p 8080:80 \
  -v $(pwd)/data:/data \
  ghcr.io/huggingface/text-generation-inference:1.4 \
  --model-id /data/my-model
```
Production-ready inference with optimizations.

---

## Future Directions

### 76. Mixture of Experts (MoE) Fine-Tuning
Fine-tune MoE models:
1. Sparse expert activation patterns
2. Router fine-tuning considerations
3. Expert specialization analysis
4. Load balancing during training
5. Efficient inference routing

### 77. Multimodal Fine-Tuning
Train vision-language models:
1. **Data**: Image-text pairs, visual instruction
2. **Projection layers**: Connect vision encoder to LLM
3. **Alignment**: Contrastive pre-training
4. **Instruction tuning**: Visual question answering
5. **Evaluation**: VQA benchmarks, human assessment

### 78. Long Context Fine-Tuning
Extend context windows:
1. **Position interpolation**: Scale position embeddings
2. **NTK-aware scaling**: Better extrapolation
3. **YaRN**: Yet another RoPE extension
4. **Ring attention**: Memory-efficient long context
5. **Data**: Long documents, books, conversations

### 79. Speculative Decoding Integration
Speed up inference:
1. Draft model generates candidates
2. Target model verifies in parallel
3. Accept or reject tokens
4. 2-3x speedup with quality preservation
5. Smaller draft model (can be LoRA)

### 80. Test-Time Compute Scaling
Improve through inference-time effort:
1. **Chain-of-thought**: Reasoning steps
2. **Self-consistency**: Multiple samples, majority vote
3. **Tree of thoughts**: Systematic exploration
4. **Verification**: Check and refine answers
5. **Reward model reranking**: Score and select best

---

## Quick Reference

### 81. Common Hyperparameters
Reference values for quick starts:
```python
# LoRA
r = 16
alpha = 32
target_modules = ["q_proj", "v_proj"]

# Training
lr = 2e-4
batch_size = 4
gradient_accumulation = 4
effective_batch = 16
warmup_ratio = 0.03

# Optimization
weight_decay = 0.01
max_grad_norm = 1.0
```

### 82. Memory Estimation
Estimate VRAM requirements:
```
# Base model (FP16): ~2 bytes/param
# Gradients: ~2 bytes/param
# Optimizer (Adam): ~8 bytes/param
# Activations: ~batch_size dependent

# QLoRA (4-bit): ~0.5 bytes/param base
# LoRA weights: ~0.1 bytes/param trainable

# Example: 7B model QLoRA:
# ~4GB base + ~0.1GB LoRA + overhead = ~6GB total
```

### 83. Dataset Size Guidelines
Recommended data sizes:
- **LoRA simple task**: 100-1000 examples
- **LoRA complex task**: 10k-50k examples
- **Full fine-tuning**: 100k+ examples
- **Continual pre-training**: 1B+ tokens
- **Quality > Quantity**: Better to have 1k good than 10k mediocre

### 84. Troubleshooting Quick Reference
Common issues and fixes:
- **CUDA OOM**: Reduce batch, enable gradient checkpointing, use QLoRA
- **Loss NaN**: Lower learning rate, check data for bad examples
- **Slow training**: Enable Flash Attention, mixed precision, more workers
- **Poor quality**: Check data quality, increase rank, longer training
- **Mode collapse**: Increase data diversity, lower learning rate

---

## Ethics & Responsibility

### 85. Data Privacy in Fine-Tuning
Protect sensitive data:
1. **PII scrubbing**: Remove before training
2. **Differential privacy**: Add noise to gradients
3. **Federated learning**: Train without data centralization
4. **Synthetic data**: Generate privacy-safe alternatives
5. **Data minimization**: Only use necessary data

### 86. Attribution & Credit
Proper attribution practices:
1. Credit base model creators
2. Acknowledge training data sources
3. Document fine-tuning methodology
4. License compliance (check base model license)
5. Share improvements with community

### 87. Harm Prevention
Prevent malicious use:
1. **Red teaming**: Test for harmful capabilities
2. **Safety filters**: Input/output filtering
3. **Usage monitoring**: Track for abuse patterns
4. **Terms of service**: Clear acceptable use policy
5. **Rapid response**: Ability to revoke access

### 88. Environmental Consideration
Minimize carbon footprint:
1. **Efficient training**: Use optimal batch sizes, early stopping
2. **Green energy**: Train on renewable-powered grids
3. **Model sharing**: Don't duplicate training effort
4. **Efficient inference**: Quantization, distillation
5. **Carbon reporting**: Track and offset emissions

---

**Total: 100+ prompts for taking control of your models through fine-tuning, LoRA, and sovereign AI deployment.**
