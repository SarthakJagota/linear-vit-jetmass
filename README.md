# Linear Attention ViT for Jet Mass Regression

## Overview

This project implements a linear-attention Vision Transformer (Performer) for end-to-end jet mass regression using CMS-style jet image data.

The workflow consists of:

1. Self-supervised pretraining on 60k unlabeled jet images  
2. Fine-tuning on labeled jet mass data  
3. Controlled comparison against a model trained from scratch  

The objective is to evaluate whether scaling self-supervised pretraining improves downstream regression performance while maintaining stable and interpretable training behavior.

---

## Model Architecture

The model is a Vision Transformer with linear attention (Performer).

### Why Performer (Linear Attention)?

The Performer architecture was selected due to its linear attention mechanism with **O(N)** complexity in sequence length, compared to the quadratic **O(N²)** complexity of standard self-attention.

For 128×128 jet images with 16×16 patches (64 tokens), this provides:

- Improved memory efficiency  
- Better scalability to higher-resolution inputs  
- Stable training behavior  
- Practical feasibility for large-scale self-supervised pretraining  

This makes Performer well-suited for building scalable foundation-style models for high-energy physics datasets.

---

### Autoencoder (Pretraining)

- Input shape: (8, 128, 128)
- Patch size: 16
- Embedding dimension: 256
- Transformer depth: 4
- Attention heads: 4
- Attention type: Performer (linear attention)
- Loss: Mean Squared Error (reconstruction)
- Learning rate: 1e-4
- Epochs: 10

---

### Regression Model (Fine-tuning)

- Encoder: Pretrained ViT encoder
- Head: Linear layer (256 → 1)
- Loss: Mean Squared Error
- Learning rate: 5e-5
- Weight decay: 1e-4
- Gradient clipping applied
- Epochs: 15

---

## Training Strategy

### Pretraining

- Dataset: 60,000 unlabeled jet images
- Objective: Image reconstruction
- Optimizer: Adam
- Goal: Learn transferable jet representations

### Fine-tuning

- Dataset split: 80% train / 20% validation
- Two models trained:
  - Pretrained (60k)
  - Scratch (random initialization)
- Identical optimization settings for fair comparison
- Stabilized training via reduced learning rate and gradient clipping

---

## Results

Validation Mean Squared Error (MSE):

| Model                | Validation MSE |
|----------------------|---------------|
| Scratch              | 0.1889        |
| Pretrained (60k)     | 0.1759        |

### Key Observation

The pretrained model consistently achieves lower validation error compared to training from scratch.

After stabilizing optimization (reduced learning rate, weight decay, gradient clipping), training curves exhibit smoother convergence while maintaining a clear performance advantage for the pretrained model.

This demonstrates that large-scale self-supervised pretraining improves downstream jet mass regression performance.

---

## How to Run

1. Install dependencies
2. Open the notebook in Google Colab
3. Use GPU runtime
4. Run all cells sequentially

Datasets are downloaded directly from CERNBox within the notebook.

---

## Conclusion

This experiment demonstrates that:

- Linear-attention Vision Transformers can effectively model jet images  
- Self-supervised representation learning improves regression performance  
- Scaling pretraining data enhances generalization  
- Stabilized optimization leads to more interpretable training dynamics  

These results support the use of scalable linear-attention architectures for foundation-style modeling in high-energy physics applications.
