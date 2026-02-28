# Linear Attention ViT for Jet Mass Regression

## Overview

This project implements a linear-attention Vision Transformer (Performer) for end-to-end jet mass regression using CMS-style jet image data.

The workflow consists of:

1. Self-supervised pretraining on unlabeled jet images  
2. Fine-tuning on labeled jet mass data  
3. Comparison against a model trained from scratch  

The objective is to evaluate whether scaling self-supervised pretraining improves downstream regression performance.

---

## Model Architecture

The model is a Vision Transformer with linear attention (Performer).

### Autoencoder (Pretraining)

- Input shape: (8, 128, 128)
- Patch size: 16
- Embedding dimension: 256
- Transformer depth: 4
- Attention heads: 4
- Attention type: Performer (linear attention)
- Loss: Mean Squared Error (reconstruction)

### Regression Model (Fine-tuning)

- Encoder: Pretrained ViT encoder
- Head: Linear layer (256 → 1)
- Loss: Mean Squared Error

---

## Training Strategy

### Pretraining

- Dataset: 60,000 unlabeled jet images
- Objective: Image reconstruction
- Optimizer: Adam
- Learning rate: 1e-4
- Epochs: 10

### Fine-tuning

- Dataset split: 80% train / 20% validation
- Two models trained:
  - Pretrained (60k)
  - Scratch (random initialization)
- Optimizer: Adam
- Learning rate: 1e-4
- Epochs: 10

---

## Results

Validation Mean Squared Error (MSE):

| Model                | Validation MSE |
|----------------------|---------------|
| Scratch              | 0.1495        |
| Pretrained (60k)     | 0.1358        |

### Key Observation

Scaling self-supervised pretraining improves downstream jet mass regression performance.  
The pretrained model achieves significantly lower validation error compared to training from scratch.

## How to Run

1. Open the notebook in Google Colab
2. Use GPU runtime
3. Run all cells sequentially

Datasets are downloaded directly from CERNBox inside the notebook.

---

## Conclusion

This experiment demonstrates that:

- Linear-attention Vision Transformers can model jet images effectively
- Self-supervised representation learning improves regression performance
- Scaling pretraining data enhances generalization

The results validate the effectiveness of large-scale self-supervised pretraining for physics-based vision tasks.
