


# 🛰️ Dual-Image Super-Resolution using EDSR (with Concatenation Fusion)

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue?logo=python)](https://www.python.org/)
[![PyTorch](https://img.shields.io/badge/PyTorch-1.12%2B-red?logo=pytorch)](https://pytorch.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Stars](https://img.shields.io/github/stars/your-username/dual-image-sr-edsr?style=social)](https://github.com/your-username/dual-image-sr-edsr)

A deep learning-based Dual-Image Super-Resolution framework built using a modified **EDSR (Enhanced Deep Super-Resolution)** architecture. The model takes **two temporally shifted low-resolution satellite images** and fuses them to generate a high-resolution output. Fusion is achieved via **simple channel-wise concatenation** followed by deep residual refinement.

---

## 📖 Overview

This project targets the problem of enhancing the spatial resolution of satellite images captured at different times. Since high-res satellites are costly and limited, we leverage **multiple low-resolution inputs** captured over time to reconstruct the corresponding high-resolution image.

This approach is especially useful for missions like **Proba-V**, where the trade-off between spatial and temporal resolution limits imagery quality.

---

## 🗂️ Dataset

This project uses a preprocessed version of the **Proba-V Super-Resolution Dataset**. The structure is as follows:

```

dual\_sr\_dataset/
├── train/
│   ├── low\_res/
│   │   ├── imgsetXXXX\_LR0.png
│   │   ├── imgsetXXXX\_LR1.png
│   └── high\_res/
│       ├── imgsetXXXX\_HR.png
├── test/
│   ├── low\_res/
│   └── high\_res/

```

Each sample consists of:
- `LR0`: Low-res image at time t
- `LR1`: Low-res image at time t+1
- `HR`: Ground truth high-res image

---

## 🧠 Model Architecture

The model combines dual inputs using **channel-wise concatenation** after feature extraction and processes the merged tensor through an **EDSR-style backbone** for refinement.

```

LR1        LR2
│          │
\[Conv]    \[Conv]
│          │
└─> \[Concat + Fusion Block] ──> \[EDSR Backbone] ──> \[Upsample] ──> SR Output

````

### Components:
- `InitialConvBlock`: 3×3 conv + ReLU per LR input
- `FusionBlock`: 1×1 conv to reduce channel size
- `EDSRBackbone`: Stack of Residual Blocks (default: 8)
- `UpsampleBlock`: Conv → PixelShuffle to upscale

---

## ⚙️ Training Details

- **Framework**: PyTorch  
- **Input Size**: LR = 64×64, HR = 256×256  
- **Scaling Factor**: ×4  
- **Loss Function**: L1 / Combined Loss (L1 + PSNR)  
- **Optimizer**: Adam  
- **Learning Rate**: 1e-4  
- **Epochs**: 50  
- **Batch Size**: 4  

Model checkpoints are saved in the `checkpoints/` directory.

---

## 📊 Evaluation Metrics

The performance is evaluated using:

- **PSNR (Peak Signal-to-Noise Ratio)**

Higher values indicate better reconstruction.

---

## ✅ Results

Example: Dual inputs, model output, and ground truth comparison.
## 🔍 Sample Output

Here’s an example of how the model enhances resolution:

![SR Output Sample](https://github.com/user-attachments//home/vnayakde/Downloads/sr_visualization/04a92e37-d06b-4116-949d-c29239e93060.jpeg)

> Sample outputs and intermediate visualizations are stored in the `sr_visualization/` directory.

---

## 📚 References

* [EDSR Paper (CVPR 2017)](https://arxiv.org/abs/1707.02921)
* [Proba-V Dataset](https://kelvins.esa.int/proba-v-super-resolution/)
* [PixelShuffle Docs](https://pytorch.org/docs/stable/generated/torch.nn.PixelShuffle.html)

---


