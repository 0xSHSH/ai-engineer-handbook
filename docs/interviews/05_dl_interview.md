# 5️⃣ Deep Learning Interview

> Part of the [Interview Handbook](README.md). Neural network architectures, training mechanics, and framework-level questions.

## 📑 Contents
- [Architectures: CNN, RNN, LSTM, GRU](#architectures-cnn-rnn-lstm-gru)
- [Transformers & Attention](#transformers--attention)
- [Embeddings & Transfer Learning](#embeddings--transfer-learning)
- [Optimization](#optimization)
- [Loss & Activation Functions](#loss--activation-functions)
- [PyTorch vs TensorFlow](#pytorch-vs-tensorflow)
- [Interview Questions](#interview-questions)

---

## Architectures: CNN, RNN, LSTM, GRU

**CNN (Convolutional Neural Network)**
- Convolution kernels slide over input, sharing weights spatially → far fewer parameters than a fully connected layer for image-sized input.
- Pooling (max/avg) downsamples, adds translation invariance.
- Stack of conv → activation → pool blocks builds up from edges → textures → parts → objects.

**RNN (Recurrent Neural Network)**
- Processes sequences step-by-step, carrying a hidden state forward.
- Suffers from **vanishing/exploding gradients** over long sequences (repeated multiplication through backprop-through-time).

**LSTM (Long Short-Term Memory)**
- Adds a cell state plus three gates (forget, input, output) that regulate what's kept, added, and exposed.
- Forget gate specifically mitigates vanishing gradients by allowing near-identity gradient flow when the gate is open.

**GRU (Gated Recurrent Unit)**
- Simplification of LSTM: merges cell/hidden state, uses two gates (reset, update) instead of three.
- Fewer parameters, often comparable performance, faster to train.

**Interview one-liner:** "LSTMs/GRUs were the answer to RNN's vanishing gradient problem before attention made recurrence mostly unnecessary for sequence modeling."

---

## Transformers & Attention

### Self-attention core idea
For each token, compute Query, Key, Value vectors. Attention score = softmax(QKᵀ/√d_k), then weighted sum of Values. This lets every token directly attend to every other token in one step — no sequential bottleneck like RNNs.

```
Attention(Q, K, V) = softmax(QKᵀ / √d_k) · V
```

- **Multi-head attention**: run several attention operations in parallel with different learned projections, letting the model attend to different relationship types (syntax, coreference, etc.) simultaneously.
- **Why divide by √d_k**: keeps dot-product magnitudes stable so softmax doesn't saturate into near-one-hot gradients.
- **Positional encoding**: transformers have no inherent notion of order (unlike RNNs), so sinusoidal or learned position embeddings are added to token embeddings.
- **Encoder vs decoder**: encoder blocks use bidirectional self-attention (BERT-style); decoder blocks use *causal* (masked) self-attention so a token can't see future tokens (GPT-style) — critical for autoregressive generation.

## Embeddings & Transfer Learning
- **Embeddings**: dense vector representations where semantic similarity ≈ geometric proximity (cosine similarity). Learned via prediction objectives (word2vec's skip-gram, or as a byproduct of transformer pretraining).
- **Transfer learning**: take a model pretrained on a large generic corpus, reuse its learned representations for a downstream task.
- **Fine-tuning**: continue training all (or most) weights of a pretrained model on task-specific data — needs more compute/data than the alternatives below but gives the strongest task adaptation.
- **Feature extraction**: freeze the pretrained backbone, train only a new head on top — cheap, works well with small datasets.

## Optimization

| Optimizer | Idea | When |
|---|---|---|
| SGD (+momentum) | Follows negative gradient, momentum smooths oscillation | Simple, well-understood, still competitive with tuning |
| Adam | Per-parameter adaptive learning rates using 1st & 2nd moment estimates | Default choice for most deep learning today |
| AdamW | Adam with decoupled weight decay (proper L2 regularization) | Standard for transformer training |

- **Learning rate schedules**: warmup (start low, ramp up) then decay (cosine/linear) — warmup prevents early instability from large gradients on randomly-initialized weights.
- **Batch size** trade-off: larger batches → more stable gradient estimates, more parallelism, but can generalize worse without LR adjustment; smaller batches → noisier gradients that can act as implicit regularization.

## Loss & Activation Functions

| Function | Type | Use |
|---|---|---|
| Cross-entropy | Loss | Classification |
| MSE | Loss | Regression |
| ReLU | Activation | Default hidden-layer activation; cheap, avoids vanishing gradient for positive inputs (but "dying ReLU" for negatives) |
| GELU | Activation | Smoother than ReLU, standard in transformers (used in BERT/GPT) |
| Sigmoid | Activation | Binary output / gating (LSTM gates) |
| Softmax | Activation | Multi-class output probability distribution |

## PyTorch vs TensorFlow

| | PyTorch | TensorFlow |
|---|---|---|
| Graph | Dynamic (define-by-run) — easier debugging, more Pythonic | Historically static (define-then-run); TF2 added eager mode by default |
| Ecosystem | Dominant in research and most modern LLM tooling (HuggingFace, most papers) | Strong in production/mobile (TF Lite, TF Serving), Keras high-level API |
| Interview relevance | Expect to write a training loop, `nn.Module`, `autograd` from memory | Know Keras `model.fit()` and the eager/graph distinction |

Minimal PyTorch training loop (commonly asked to write from scratch):
```python
import torch, torch.nn as nn

model = nn.Sequential(nn.Linear(10, 32), nn.ReLU(), nn.Linear(32, 1))
optimizer = torch.optim.Adam(model.parameters(), lr=1e-3)
loss_fn = nn.MSELoss()

for epoch in range(epochs):
    for x, y in dataloader:
        optimizer.zero_grad()
        pred = model(x)
        loss = loss_fn(pred, y)
        loss.backward()
        optimizer.step()
```

---

## Interview Questions
1. Why do transformers scale better than RNNs for long sequences? (Parallelizable attention vs sequential recurrence, and shorter gradient paths between distant tokens.)
2. What problem do skip/residual connections solve? (Vanishing gradients in very deep nets; lets gradient flow through an identity shortcut.)
3. Why does batch normalization help training? (Reduces internal covariate shift, allows higher learning rates, smooths the loss landscape.)
4. Explain the difference between fine-tuning and feature extraction, and when you'd choose each.
5. Walk through backpropagation for a 2-layer network by hand (chain rule application) — common whiteboard ask.
6. What's the difference between LayerNorm and BatchNorm, and why do transformers use LayerNorm? (LayerNorm normalizes across features per-sample, independent of batch size — important for variable-length sequences and small/large batch robustness.)

---
*Part of the [AI Engineer Handbook](../../README.md) · [Interview Handbook](README.md) · Next: [LLM Interview](06_llm_interview.md).*
