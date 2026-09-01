# Efficient Real-World Deblurring using Single Images — AIM 2025 Challenge Report

**Authors:** Daniel Feijoo, Paula Garrido-Mellado, Marcos V. Conde, Jaesung Rim, Alvaro Garcia, Sunghyun Cho, Radu Timofte
**Venue:** ICCV Workshops — AIM 2025

---

## 📌 General Overview

The **AIM 2025 Challenge on Efficient Real-World Deblurring using Single Images** focuses on developing image deblurring algorithms that can remove motion blur from real-world photographs while satisfying strict computational constraints.

The primary goal is to develop **practical deblurring models capable of running on mobile and edge devices**.

Motion blur frequently occurs in low-light environments because cameras require longer exposure times. This is particularly problematic for smartphones because their small image sensors collect less light and therefore often require longer shutter speeds.

As a result, efficient image deblurring has become increasingly important for mobile photography and other resource-constrained vision applications.

### Challenge Statistics

| Metric                       |           Value |
| ---------------------------- | --------------: |
| Registered participants      |              71 |
| Teams with valid submissions |               4 |
| Best PSNR                    |  **31.1298 dB** |
| Maximum parameters           |        **< 5M** |
| Maximum computational cost   | **< 200 GMACs** |

---

# 🎯 Problem Being Solved

Real-world image deblurring is considerably more difficult than synthetic deblurring.

### Problems with Synthetic Blur

Synthetic datasets often fail to accurately represent real camera degradation, including:

* Continuous camera and object motion
* Saturated pixels
* Sensor noise
* Image Signal Processor (ISP) effects
* Real exposure behavior
* Complex motion trajectories

Therefore, models trained only on synthetic blur may perform poorly on photographs captured using real cameras.

### Computational Challenges

Modern learning-based deblurring models can also be computationally expensive.

Large receptive fields and high-resolution feature processing can result in:

* High FLOPs
* Large memory requirements
* High inference latency
* Difficulty deploying on smartphones
* Difficulty deploying on edge devices

Previous deblurring challenges such as **NTIRE 2019–2021** generally did not enforce such strict computational constraints.

The AIM 2025 challenge therefore emphasizes:

1. **Real-world data**
2. **Computational efficiency**
3. **Practical deployment**

---

# 📊 Dataset — RSBlur

## Data Collection

The challenge uses the **RSBlur** dataset, which contains real-world blurred and sharp image pairs.

A dual-camera system is used to capture the data.

```text
                    Incoming Light
                          │
                          ▼
                    ┌───────────┐
                    │Beam Splitter│
                    └─────┬─────┘
                          │
              ┌───────────┴───────────┐
              ▼                       ▼
      Short-Exposure Sensor    Long-Exposure Sensor
              │                       │
              ▼                       ▼
        Sharp Image             Blurred Image
        (Ground Truth)           (Motion Blur)
```

### Sensors

**Short-exposure sensor**

* Captures sharp images
* Used as ground truth

**Long-exposure sensor**

* Captures real-world motion blur
* Used as the input image

This setup produces paired images that contain realistic camera blur rather than artificially generated blur.

---

## Dataset Statistics

| Split              | Number of Paired Images |
| ------------------ | ----------------------: |
| Training           |                   8,887 |
| Validation         |                   1,120 |
| Original Test      |                   3,360 |
| Challenge Test Set |                 **420** |
| Test Scenes        |                  **84** |

### Important

The challenge introduced a **new test set containing scenes disjoint from the original dataset**.

This prevents models from simply memorizing the original training/test scenes and provides a better measurement of generalization.

---

# 📏 Challenge Evaluation

The challenge evaluates models using a combination of:

* **PSNR**
* **SSIM**
* **LPIPS**

The overall score is defined as:

$$
Score = \lambda_1 \cdot PSNR +
\lambda_2 \cdot SSIM +
\lambda_3 \cdot LPIPS
$$

### Metrics

| Metric | Meaning                             | Better |
| ------ | ----------------------------------- | ------ |
| PSNR   | Pixel-level reconstruction accuracy | Higher |
| SSIM   | Structural similarity               | Higher |
| LPIPS  | Perceptual similarity               | Lower  |

These metrics evaluate different aspects of image restoration.

**PSNR** focuses on pixel-level accuracy, while **SSIM** measures structural similarity and **LPIPS** evaluates perceptual similarity.

---

# ⚡ Computational Constraints

The challenge imposes strict efficiency requirements.

The constraints are based on the **NAFNet-C16-L28** baseline.

| Configuration  | Parameters |    MACs |  PSNR |
| -------------- | ---------: | ------: | ----: |
| NAFNet-C16-L14 |      2.68M |  94.98G | 32.23 |
| NAFNet-C16-L28 |      4.35M | 146.33G | 32.42 |
| NAFNet-C32-L28 |     17.11M | 566.33G | 32.83 |

### Challenge Limits

```text
Parameters: < 5 Million
MACs:       < 200 GMACs
Resolution: 1920 × 1200
```

The purpose of these constraints is to encourage models that are not only accurate but also practical for real-world deployment.

---

# 🏆 Challenge Results

## Final Rankings

| Rank | Team          | Method                | Params |    MACs |       PSNR |      SSIM |
| ---: | ------------- | --------------------- | -----: | ------: | ---------: | --------: |
| 🥇 1 | MiVideoDeblur | **NAFRepLocal**       |  4.76M | 198.25G | **31.130** | **0.843** |
| 🥈 2 | MAILab        | **RestormerL**        |  1.41M | 199.39G | **31.100** |     0.840 |
| 🥉 3 | IPIU          | **Data Augmentation** |  4.35M | 146.33G |     30.492 |     0.832 |
|    4 | XD2025PBL     | **SA-NAFNet**         |  4.51M |  172.2G |     30.189 |     0.819 |
|    — | Baseline      | NAFNet-C16-L28        |  4.35M | 146.33G |     30.173 |     0.826 |

### Key Observations

* The top three teams significantly outperformed the baseline.
* The winning method achieved approximately **1 dB improvement** over the baseline.
* Every top-performing method remained within the computational constraints.
* Efficient architectures can achieve competitive restoration quality without extremely large models.

---

# 🥇 Method 1 — NAFRepLocal

## Winner — MiVideoDeblur

**Team:** MiVideoDeblur
**Organization:** Xiaomi Inc., China

NAFRepLocal is based on **NAFNet** and introduces modifications designed to improve restoration quality while maintaining computational efficiency.

---

## Architecture

The network consists of an encoder, middle block, and decoder.

```text
Input
  │
  ▼
┌─────────────────┐
│ Encoder         │
│ 32 → 64 → 128 → 256 │
│ 4 × NAFBlocks   │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Middle Block    │
│ 512 Channels    │
│ NAFBlock        │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Decoder         │
│ 256 → 128 → 64 → 32 │
│ 4 × NAFBlocks   │
└────────┬────────┘
         │
         ▼
      Output
```

---

## Key Innovations

### 1. SCA-L and SCA-G

The original NAFNet uses **Simplified Channel Attention (SCA)** with global average pooling.

NAFRepLocal introduces **SCA-L**, which performs a more local operation.

It also adds **SCA-G after the middle NAFBlock** to capture global information.

Therefore, the model benefits from both:

```text
Local information
       +
Global information
       ↓
Better feature representation
```

---

### 2. Reparameterization

The model uses reparameterization to separate training-time complexity from inference-time complexity.

During training:

```text
Complex structure
      ↓
Better learning capacity
```

During inference:

```text
Complex structure
      ↓
Folded/reparameterized
      ↓
Simpler network
      ↓
Faster inference
```

This allows the model to benefit from a more sophisticated training structure without maintaining the same computational complexity during deployment.

---

### 3. EMA — Exponential Moving Average

The model uses **Exponential Moving Average (EMA)** to maintain a smoothed version of the model weights during training.

Conceptually:

```text
Current weights ─────┐
                    ├──► EMA weights
Previous EMA weights ┘
```

EMA can provide more stable model parameters and improve final restoration performance.

---

# 🏋️ NAFRepLocal Training

The winning model uses **four-stage progressive training**.

| Stage | Input Size | Configuration              | Steps |   Time |
| ----: | ---------- | -------------------------- | ----: | -----: |
|     1 | 512×512    | 5×5 convolution            |  400K |  ~37 h |
|     2 | 1024×1024  | —                          |  400K | ~137 h |
|     3 | 1024×1024  | 3×3 convolution            |  400K | ~132 h |
|     4 | 1024×1024  | SCA-G + reparameterization |   50K |   ~8 h |

### Training Configuration

| Component     | Setting          |
| ------------- | ---------------- |
| Optimizer     | AdamW            |
| Learning Rate | Cosine annealing |
| Final LR      | 1e-7             |
| Loss          | PSNR loss        |
| Augmentation  | Random flips     |

The progressive strategy gradually increases the difficulty and complexity of training.

---

# 🥈 Method 2 — RestormerL

## Second Place — MAILab

**Team:** MAILab
**Organizations:** TU Darmstadt and hessian.AI, Germany

RestormerL is a lightweight version of the **Restormer** architecture.

The main strategy is aggressive reduction of model size while retaining enough capacity for high-quality restoration.

---

## Architecture Modifications

| Component          | Original Restormer | RestormerL            |
| ------------------ | ------------------ | --------------------- |
| Transformer blocks | [4, 6, 6, 8]       | **[2, 2, 2, 4]**      |
| Channels           | [48, 96, 192, 384] | **[16, 32, 64, 128]** |
| Refinement block   | Yes                | **Removed**           |
| Activation         | GELU               | **SiLU**              |

This significantly reduces the model's parameter count.

### Result

```text
Original Restormer
       ↓
Reduced layers
       ↓
Reduced channels
       ↓
Removed refinement block
       ↓
RestormerL
```

Despite its small size, RestormerL achieved a PSNR of **31.100 dB**.

---

## Progressive Learning

| Iteration | Patch Size | Batch Size |
| --------: | ---------: | ---------: |
|    0–100K |    256×256 |         96 |
| 100K–200K |    384×384 |         64 |
| 200K–300K |    512×512 |         32 |

### Training Configuration

| Component     | Setting          |
| ------------- | ---------------- |
| Hardware      | 8 × H100 GPUs    |
| Training Time | ~18 hours        |
| Optimizer     | AdamW            |
| Loss          | L1               |
| Initial LR    | 3e-4             |
| Final LR      | 1e-6             |
| LR Schedule   | Cosine annealing |

---

# 🥉 Method 3 — IPIU

## Third Place — Data Augmentation

**Team:** IPIU
**Organization:** Xidian University, China

The IPIU approach is based on a NAFNet-style architecture and focuses heavily on **training augmentation and inference-time augmentation**.

---

## Architecture Enhancements

The method includes:

* Layer Normalization
* GELU activation
* Simplified architecture
* Removal of unnecessary nonlinear activations

The goal is to stabilize training while maintaining a lightweight architecture.

---

## Training

| Component     | Setting                     |
| ------------- | --------------------------- |
| Loss          | L1 pixel loss               |
| Optimizer     | Adam                        |
| Learning Rate | 1e-3                        |
| Augmentation  | Cropping, scaling, flipping |
| Hardware      | NVIDIA RTX 3090             |
| Training Time | ~69 hours                   |

---

## Test-Time Augmentation

IPIU also applies **TTA — Test-Time Augmentation**.

The input image is transformed using:

* Horizontal flipping
* Vertical flipping
* Scaling

The model generates predictions for these transformed versions, which can then be combined to improve the final result.

```text
Original Image
      │
 ┌────┼────┐
 ▼    ▼    ▼
Flip Scale Flip
 │    │    │
 └────┼────┘
      ▼
   Combined
   Prediction
```

---

# 4️⃣ Method 4 — SA-NAFNet

## Fourth Place — XD2025PBL

SA-NAFNet extends NAFNet by integrating **Spatial Attention**.

The method focuses on improving the model's ability to identify and reconstruct important spatial regions.

---

## Combined Loss

The training objective combines:

```text
L1 Pixel Loss
      +
Perceptual Loss
      +
Edge Loss
      ↓
Combined Loss
```

### Why?

**L1 loss**

Focuses on pixel-level reconstruction.

**Perceptual loss**

Encourages perceptually realistic image structures.

**Edge loss**

Encourages sharper boundaries and fine details.

---

## Multi-Step Training

| Step | Focus                | Loss                   |
| ---: | -------------------- | ---------------------- |
|  1–2 | Pixel-level details  | L1                     |
|  3–4 | Structural coherence | L1 + Perceptual + Edge |
|    5 | Final refinement     | All losses balanced    |

### Training Configuration

| Component     | Setting             |
| ------------- | ------------------- |
| Optimizer     | Adam                |
| Initial LR    | 1e-3                |
| Final LR      | 1e-6                |
| Scheduler     | Cosine annealing    |
| Hardware      | 4× 2080Ti + 2× 3090 |
| Training Time | ~20 hours           |

---

# 🔍 Common Themes Across Top Methods

Several patterns appear across the highest-ranked solutions.

## 1. NAFNet is a Strong Foundation

Three of the four submitted approaches are directly based on or heavily inspired by NAFNet.

This suggests that NAFNet provides an excellent balance between:

* Restoration quality
* Parameter efficiency
* Computational efficiency
* Implementation simplicity

---

## 2. Progressive Training

Several approaches use progressive or staged training.

Instead of immediately training at the highest resolution:

```text
Small resolution
       ↓
Medium resolution
       ↓
Large resolution
       ↓
Fine refinement
```

This can make optimization easier while gradually exposing the model to more difficult restoration problems.

---

## 3. Strong Data Augmentation

Augmentation techniques include:

* Random cropping
* Scaling
* Horizontal flipping
* Vertical flipping
* Test-time augmentation

These techniques help models generalize to different blur patterns and image conditions.

---

## 4. Attention Mechanisms

Attention is used to improve feature representation.

Examples include:

* SCA-L
* SCA-G
* Spatial Attention

The general idea is:

```text
Input Features
      ↓
Attention
      ↓
Important information emphasized
      ↓
Better reconstruction
```

---

## 5. Multi-Loss Training

Some approaches combine multiple objectives:

```text
Pixel Accuracy
      +
Perceptual Quality
      +
Edge Sharpness
      ↓
Better Restoration
```

This is particularly useful when the goal is not simply to minimize pixel error but also to recover visually meaningful details.

---

# 💡 Why the Methods Work

## NAFRepLocal — "Smart Optimization"

The winning approach combines:

* NAFNet
* Local channel attention
* Global channel attention
* Reparameterization
* EMA
* Progressive training

Its main strength is achieving high restoration quality while staying below the strict computational limits.

---

## RestormerL — "Aggressive Pruning"

RestormerL dramatically reduces:

* Transformer layers
* Channel dimensions
* Refinement components

Despite having only **1.41M parameters**, it reaches almost the same PSNR as the winning method.

This demonstrates that **a carefully designed small model can outperform larger models in efficiency-constrained settings**.

---

## IPIU — "Augmentation is Key"

IPIU demonstrates that a relatively simple architecture can achieve strong results through:

* Strong training augmentation
* Scaling
* Cropping
* Flipping
* Test-time augmentation

This shows that improvements do not always require architectural complexity.

---

## SA-NAFNet — "Multi-Loss Focus"

SA-NAFNet combines:

* Spatial attention
* Pixel loss
* Perceptual loss
* Edge loss
* Progressive training

This approach focuses strongly on recovering structural and fine-grained details.

---

# 🧠 Technical Concepts

## NAFNet

**NAFNet** stands for **Nonlinear Activation Free Network**.

Unlike traditional CNN architectures that heavily rely on activation functions such as ReLU, NAFNet simplifies the architecture using components such as:

* SimpleGate
* Simplified Channel Attention (SCA)
* Lightweight convolutional blocks

The result is a computationally efficient image restoration network.

---

## SimpleGate

SimpleGate is a simple feature transformation mechanism used in NAFNet.

Instead of applying a conventional nonlinear activation:

```text
Feature
  ↓
Split into two parts
  ↓
A × B
  ↓
Output
```

This provides a lightweight way of introducing feature interaction without using traditional nonlinear activation functions.

---

# 🎯 Simplified Channel Attention — SCA

SCA is an attention mechanism used in NAFNet.

### SCA-G — Global Attention

Global average pooling summarizes the entire spatial feature map.

```text
Feature Map
     ↓
Global Average Pooling
     ↓
Global Channel Statistics
     ↓
Channel Attention
```

This provides global contextual information.

---

### SCA-L — Local Attention

SCA-L performs attention using more localized information.

Conceptually:

```text
Feature Map
     ↓
Local statistics
     ↓
Local channel information
     ↓
Attention
```

This helps preserve local structures that may be lost when relying only on global information.

---

# 🔄 Reparameterization

Reparameterization allows a model to use different structures during training and inference.

### Training

```text
Multiple/complex branches
          ↓
Higher training flexibility
```

### Inference

```text
Complex branches
       ↓
Parameter folding
       ↓
Simpler equivalent operation
       ↓
Faster inference
```

For example, a larger convolution such as a **5×5 convolution** can be transformed/folded into an efficient inference representation.

The main advantage is:

> **Training complexity without necessarily keeping the same inference complexity.**

---

# 📈 EMA — Exponential Moving Average

EMA maintains a moving average of model parameters during training.

A simplified formulation is:

$$
\theta_{EMA}
=
\alpha\theta_{EMA}
+
(1-\alpha)\theta
$$

where:

* $\theta$ = current model parameters
* $\theta_{EMA}$ = averaged parameters
* $\alpha$ = smoothing factor

This produces smoother parameters that can provide better validation and test performance.

---

# ⚙️ MACs

**MAC** means **Multiply-Accumulate operation**.

It represents a basic computational operation:

$$
a \times b + c
$$

MACs are commonly used to estimate the computational complexity of neural networks.

For example:

```text
100 GMACs
```

means approximately **100 billion multiply-accumulate operations** for the specified input resolution.

The AIM 2025 challenge limits models to:

```text
< 200 GMACs
```

for **1920×1200** images.

---

# 🔬 Relevance to Our FYP

Efficient image deblurring is particularly relevant to our FYP because blurry facial images can negatively affect downstream face recognition.

Our pipeline can conceptually be represented as:

```text
Camera Image
      │
      ▼
┌───────────────┐
│ Blur Detection│
└───────┬───────┘
        │
        ▼
┌────────────────┐
│ Image Deblurring│
└───────┬────────┘
        │
        ▼
┌────────────────┐
│ Face Detection │
└───────┬────────┘
        │
        ▼
┌────────────────┐
│ Face Recognition│
└───────┬────────┘
        │
        ▼
┌────────────────┐
│ Face Embedding │
└───────┬────────┘
        │
        ▼
┌────────────────┐
│ Database Match │
└────────────────┘
```

---

# 👤 Stage 5 — Face Recognition

Motion blur can significantly reduce the quality of facial features.

For example:

```text
Blurred Face
     ↓
Distorted facial features
     ↓
Poor face embedding
     ↓
Lower recognition accuracy
```

An efficient deblurring model can be placed before face recognition:

```text
Blurred Face
     ↓
Deblurring
     ↓
Sharper Face
     ↓
Face Embedding
     ↓
Recognition
```

This could potentially improve the reliability of the recognition system.

---

# 🗄️ Stage 7 — Database Matching

After deblurring and face recognition, the system can generate face embeddings.

Conceptually:

```text
Deblurred Face
      ↓
Face Encoder
      ↓
Embedding Vector
      ↓
Database Search
      ↓
Similarity Matching
      ↓
Identity
```

The challenge's focus on efficiency is also relevant here because real-world retrieval systems need to balance:

* Accuracy
* Latency
* Memory
* Computational cost

Efficient preprocessing can therefore be important for building a practical end-to-end vision system.

---

# 📋 Method Comparison

| Aspect            | NAFRepLocal                | RestormerL         | IPIU                | SA-NAFNet         |
| ----------------- | -------------------------- | ------------------ | ------------------- | ----------------- |
| Base Architecture | NAFNet                     | Restormer          | NAFNet              | NAFNet            |
| Key Innovation    | Reparameterization + SCA-L | Aggressive pruning | Strong augmentation | Spatial Attention |
| Parameters        | 4.76M                      | **1.41M**          | 4.35M               | 4.51M             |
| MACs              | 198.25G                    | 199.39G            | **146.33G**         | 172.2G            |
| PSNR              | **31.130**                 | 31.100             | 30.492              | 30.189            |
| Training Time     | ~314h                      | ~18h               | ~69h                | ~20h              |
| TTA               | No                         | No                 | **Yes**             | No                |

---

# 🏁 Final Takeaways

The AIM 2025 challenge demonstrates that **real-world image deblurring can be performed efficiently without relying on extremely large neural networks**.

The most important lessons are:

1. **NAFNet is a strong baseline for efficient image restoration.**
2. **Model efficiency is as important as restoration quality for mobile deployment.**
3. **Progressive training can help models learn increasingly difficult restoration tasks.**
4. **Attention mechanisms can improve local and global feature reconstruction.**
5. **Reparameterization can provide training-time flexibility while keeping inference efficient.**
6. **Strong data augmentation can significantly improve generalization.**
7. **Multi-loss training can improve structural and perceptual quality.**
8. **A model with fewer parameters can still achieve competitive performance.**
9. **Real-world datasets are important because synthetic blur does not capture every camera degradation effect.**
10. **Efficient deblurring can serve as a useful preprocessing stage for downstream computer vision tasks such as face recognition.**

---

# 📚 Overall Conclusion

The AIM 2025 Challenge shifts the focus of image deblurring from simply achieving the highest possible restoration quality toward achieving a **practical balance between quality and computational efficiency**.

The winning NAFRepLocal approach demonstrates that careful architectural design, attention mechanisms, reparameterization, EMA, and progressive training can produce high-quality real-world deblurring within a strict **5M parameter and 200 GMAC** budget.

For our FYP, the most relevant takeaway is that **an efficient NAFNet-based deblurring model can potentially be integrated before face detection and recognition to improve the quality of blurred facial inputs without introducing excessive computational overhead.**
