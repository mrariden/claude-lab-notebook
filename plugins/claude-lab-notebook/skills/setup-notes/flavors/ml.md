# ML Flavor

Machine Learning experiment tracking and research documentation.

## Configuration

```yaml
name: ml
display_name: Machine Learning
description: ML experiment tracking, hyperparameter logging, and research documentation
```

## Directory Structure

```yaml
directories:
  - experiments    # Training runs, hyperparameter sweeps
  - decisions      # Architecture choices, framework decisions
  - troubleshooting # CUDA errors, convergence issues
  - research       # Papers, techniques, external references
```

## Protocol Examples

Use these domain-specific examples in the note-taking protocol.

### When to Load Additional Files

**User says:** "I'm working on learning rates"
**You respond:** "I see from INDEX.md we did LR experiments on 2025-01-10. Should I read `experiments/2025-01-10-lr-sweep.md` for details?"

**User says:** "I'm getting a CUDA error"
**You respond:** "Should I check `troubleshooting/cuda-errors.md` for known solutions?"

**User says:** "Why did we choose this architecture?"
**You respond:** "Should I read `decisions/2025-01-12-architecture.md` for the rationale?"

### Progressive Disclosure Example Session

```
[Session starts]
Claude: [Reads notes/INDEX.md - 1K tokens]
Claude: [Reads notes/quick-reference.md - 500 tokens]
Claude: "I've loaded your project notes index. What are you working on?"

User: "I want to improve training speed"

Claude: "From INDEX.md, I see we explored batch size on 2025-01-09
and data loading on 2025-01-11. Should I read those experiments?"

User: "Yes, read the batch size one"

Claude: [Uses view on notes/experiments/2025-01-09-batch-size.md - 2K tokens]
Claude: "Based on that experiment, batch_size=32 works best.
Going higher causes OOM. Want to try a different approach?"

[Works on task...]

[Session ends]
Claude: "Should I create an experiment note for today's work?"

User: "Yes"

Claude: [Creates notes/experiments/2025-01-15-dataloader-optimization.md]
Claude: "The experiment note has been created. Should I update INDEX.md
since we found DataLoader num_workers=4 is optimal?"

Total context used: ~5K tokens instead of 50K+ if all files loaded
```

## INDEX.md Examples

```markdown
## All Experiments
<!-- Each entry has: Title, Tags, Finding, File link -->
<!-- Example:
**2025-01-15: Optimizer comparison**
- Tags: optimizer, adamw, sgd, training
- Finding: AdamW 15% better than SGD for this architecture
- File: [experiments/2025-01-15-optimizer-comparison.md](experiments/2025-01-15-optimizer-comparison.md)
-->

## All Decisions
<!-- Example:
**2025-01-14: Choose AdamW optimizer**
- Tags: optimizer, architecture, performance
- Finding: AdamW chosen for stability and performance
- File: [decisions/2025-01-14-optimizer-choice.md](decisions/2025-01-14-optimizer-choice.md)
-->

## All Troubleshooting Guides
<!-- Example:
**CUDA out of memory errors**
- Tags: cuda, memory, gpu, oom
- Finding: Reduce batch_size to 32 or lower for RTX 3090
- File: [troubleshooting/cuda-memory-errors.md](troubleshooting/cuda-memory-errors.md)
-->

## All Research
<!-- Example:
**Attention mechanisms**
- Tags: attention, transformer, papers
- Finding: Multi-head attention key for performance
- File: [research/attention-mechanisms.md](research/attention-mechanisms.md)
-->
```

## quick-reference.md Format

```yaml
# Configuration format for "What's Working Right Now"
format: yaml
example: |
  model: gpt2-medium
  batch_size: 32
  learning_rate: 0.0001
  optimizer: adamw
  scheduler: cosine
```

## Tool Integration

- **W&B**: `wandb run: https://wandb.ai/user/project/runs/xyz`
- **TensorBoard**: `tensorboard --logdir=runs/`
- **GPU Monitoring**: `nvidia-smi`, `gpustat`
- **Experiment Tracking**: MLflow, Neptune, Comet
