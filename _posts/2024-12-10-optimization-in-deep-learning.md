---
layout: post
title: "Optimization in Deep Learning: From SGD to Modern Optimizers"
date: 2024-12-10 16:45:00 +0000
tags: [optimization, deep-learning, SGD, Adam, training]
author: Lausen
reading_time: 18
excerpt: "Optimization is at the heart of training deep neural networks. This post explores the evolution of optimization algorithms from basic stochastic gradient descent to modern adaptive methods like Adam, AdaGrad, and beyond."
---

Training deep neural networks is fundamentally an optimization problem. We start with a randomly initialized model and iteratively adjust its parameters to minimize a loss function. The choice of optimization algorithm can dramatically affect both training speed and final model performance.

## The Optimization Landscape

Deep learning optimization is notoriously challenging due to:

- **High dimensionality**: Modern models have millions or billions of parameters
- **Non-convexity**: Multiple local minima and saddle points
- **Vanishing/exploding gradients**: Gradients can become too small or too large
- **Noisy gradients**: Mini-batch training introduces noise

## Stochastic Gradient Descent (SGD)

The foundation of deep learning optimization is stochastic gradient descent:

```
θ_{t+1} = θ_t - η ∇_θ L(θ_t)
```

Where:
- `θ` represents the model parameters
- `η` is the learning rate
- `∇_θ L(θ_t)` is the gradient of the loss function

### SGD with Momentum

Plain SGD can be slow and get stuck in local minima. Momentum helps by accumulating gradients:

```
v_t = γv_{t-1} + η∇_θ L(θ_t)
θ_{t+1} = θ_t - v_t
```

This allows the optimizer to build up velocity in consistent directions and dampen oscillations.

## Adaptive Learning Rate Methods

### AdaGrad

AdaGrad adapts the learning rate for each parameter based on historical gradients:

```
G_t = G_{t-1} + ∇_θ L(θ_t)²
θ_{t+1} = θ_t - η / √(G_t + ε) * ∇_θ L(θ_t)
```

This gives frequently updated parameters smaller learning rates and infrequently updated ones larger rates.

**Pros**: Great for sparse data and features
**Cons**: Learning rate can become too small over time

### RMSprop

RMSprop addresses AdaGrad's learning rate decay problem by using an exponential moving average:

```
v_t = βv_{t-1} + (1-β)∇_θ L(θ_t)²
θ_{t+1} = θ_t - η / √(v_t + ε) * ∇_θ L(θ_t)
```

### Adam (Adaptive Moment Estimation)

Adam combines the ideas of momentum and RMSprop:

```
m_t = β₁m_{t-1} + (1-β₁)∇_θ L(θ_t)        # First moment
v_t = β₂v_{t-1} + (1-β₂)∇_θ L(θ_t)²       # Second moment

m̂_t = m_t / (1-β₁ᵗ)                        # Bias correction
v̂_t = v_t / (1-β₂ᵗ)                        # Bias correction

θ_{t+1} = θ_t - η * m̂_t / (√v̂_t + ε)
```

**Default hyperparameters**: β₁=0.9, β₂=0.999, ε=10⁻⁸

Adam is widely used due to its good default performance across many problems.

## Learning Rate Scheduling

The learning rate is perhaps the most important hyperparameter. Common scheduling strategies include:

### Step Decay
Reduce learning rate by a factor every few epochs:
```
η_t = η₀ * γ^(floor(t/step_size))
```

### Exponential Decay
```
η_t = η₀ * e^(-λt)
```

### Cosine Annealing
```
η_t = η_min + (η_max - η_min) * (1 + cos(πt/T)) / 2
```

### Warm-up
Start with a small learning rate and gradually increase:
```python
if t < warmup_steps:
    η_t = η₀ * t / warmup_steps
else:
    η_t = scheduled_rate(t)
```

## Modern Developments

### AdamW
Adam with decoupled weight decay, addressing generalization issues:
```
θ_{t+1} = θ_t - η * (m̂_t / (√v̂_t + ε) + λθ_t)
```

### RAdam (Rectified Adam)
Addresses the variance issue in early training phases of Adam.

### Lookahead
A meta-optimizer that can be combined with any base optimizer:
```
φ_{t+1} = φ_t + α(θ_{t+1} - φ_t)
```

### LAMB (Layer-wise Adaptive Moments optimizer for Batch training)
Designed for large batch training, particularly effective for BERT-like models.

## Practical Guidelines

### Choosing an Optimizer

1. **Start with Adam**: Good default choice for most problems
2. **Try SGD with momentum**: Often achieves better generalization
3. **Consider AdamW**: For regularization-sensitive problems
4. **Experiment with learning rates**: Often more important than optimizer choice

### Hyperparameter Tuning

1. **Learning rate**: Most critical parameter
   - Too high: Training becomes unstable
   - Too low: Training is slow and may get stuck

2. **Batch size**: Affects gradient noise and memory usage
   - Larger batches: More stable gradients, less noise
   - Smaller batches: More exploration, better generalization

3. **Momentum parameters**: Usually β₁=0.9, β₂=0.999 work well for Adam

### Common Issues and Solutions

**Exploding gradients**: Use gradient clipping
```python
torch.nn.utils.clip_grad_norm_(model.parameters(), max_norm=1.0)
```

**Vanishing gradients**: 
- Use residual connections
- Proper weight initialization
- Batch normalization

**Slow convergence**:
- Increase learning rate
- Use learning rate scheduling
- Try different optimizer

## Implementation Example

Here's a PyTorch implementation comparing different optimizers:

```python
import torch.optim as optim

# SGD with momentum
optimizer = optim.SGD(model.parameters(), lr=0.01, momentum=0.9)

# Adam
optimizer = optim.Adam(model.parameters(), lr=0.001, betas=(0.9, 0.999))

# AdamW
optimizer = optim.AdamW(model.parameters(), lr=0.001, weight_decay=0.01)

# Learning rate scheduling
scheduler = optim.lr_scheduler.CosineAnnealingLR(optimizer, T_max=100)
```

## Recent Research Directions

### Sharpness-Aware Minimization (SAM)
Seeks flat minima to improve generalization:
```
θ_{t+1} = θ_t - η∇_θ L(θ_t + ρ∇_θ L(θ_t)/||∇_θ L(θ_t)||)
```

### Second-Order Methods
Using curvature information for better updates:
- **L-BFGS**: Limited-memory BFGS
- **K-FAC**: Kronecker-Factored Approximate Curvature

## Conclusion

Optimization in deep learning has evolved significantly from basic SGD to sophisticated adaptive methods. While Adam remains a solid default choice, the optimal optimizer often depends on the specific problem, architecture, and dataset.

Key takeaways:
- **Learning rate is crucial**: Spend time tuning it
- **No single optimizer rules all**: Experiment with different methods
- **Combine techniques**: Use scheduling, warm-up, and regularization
- **Monitor training dynamics**: Watch loss curves and gradient norms

The field continues to evolve, with new optimizers and techniques regularly proposed. Understanding the fundamentals allows practitioners to make informed choices and adapt to new developments.

## References

- Kingma, D. P., & Ba, J. (2014). Adam: A method for stochastic optimization.
- Loshchilov, I., & Hutter, F. (2017). SGDR: Stochastic gradient descent with warm restarts.
- You, Y., et al. (2019). Large batch optimization for deep learning.
- Zhang, M., et al. (2019). Lookahead optimizer: k steps forward, 1 step back.

---

*Next in this series: Understanding the role of batch normalization in optimization and training stability.* 