# π-Diff: Physically-Inspired Low-Light Image Enhancement with Structure Preserving Diffusion Priors

> **CVPRW 2026**

π-Diff is an **unsupervised, training-free framework for low-light image enhancement (LLIE)** that combines diffusion priors with a physically-inspired image signal processing (ISP) recovery strategy.

Instead of treating the output of a diffusion model as the final enhanced image, π-Diff interprets it as a **noiseless structural proxy** and subsequently performs physically explainable illumination and color restoration.

---

## 📌 Paper Information

| Information     | Details                                                                                            |
| --------------- | -------------------------------------------------------------------------------------------------- |
| **Title**       | π-Diff: Physically-Inspired Low-Light Image Enhancement with Structure Preserving Diffusion Priors |
| **Conference**  | CVPRW 2026                                                                                         |
| **Authors**     | Min Wang, Xue Wang, Guoqing Zhou, Qing Wang                                                        |
| **Institution** | School of Computer Science, Northwestern Polytechnical University, Xi'an, China                    |
| **Task**        | Low-Light Image Enhancement                                                                        |
| **Framework**   | Unsupervised & Training-Free                                                                       |

---

## 📖 Overview

Low-Light Image Enhancement (LLIE) aims to improve the visual quality of images captured under poor illumination.

Images captured in low-light environments often suffer from:

* Insufficient exposure
* Color distortion
* Noise
* Loss of fine details
* Low contrast
* Under-exposed regions

Recent **diffusion model (DM)-based approaches** have shown strong restoration capabilities. However, directly using diffusion models to generate enhanced images can introduce several problems.

### Problems with Existing Diffusion-Based LLIE Methods

Existing approaches often couple multiple tasks into the diffusion denoising process:

```text
Denoising
    +
Illumination Restoration
    +
Color Restoration
    +
Exposure Enhancement
        ↓
   Final Image
```

This makes the prediction process highly uncertain and can result in:

* Blurred details
* Under-exposed regions
* Color distortion
* Hallucinated structures
* Inconsistent scene colors

π-Diff addresses these limitations by **decoupling structural recovery from physical image enhancement**.

---

# 💡 Key Innovation

## Structural Proxy Instead of Direct Generation

The central idea of π-Diff is a paradigm shift in how diffusion outputs are interpreted.

### Conventional Diffusion-Based Enhancement

```text
Low-Light Image
      ↓
Diffusion Model
      ↓
Enhanced Image
```

The diffusion model is directly responsible for both:

1. Recovering image structures
2. Enhancing illumination and color

This increases prediction uncertainty.

---

### π-Diff

π-Diff separates these two processes:

```text
              Low-Light Image
                    │
                    ▼
            Diffusion Process
                    │
                    ▼
        Noiseless Structural Proxy
                    │
          ┌─────────┴─────────┐
          ▼                   ▼
   Illumination Map       Reflectance Map
          │                   │
          ▼                   │
 Adaptive Light Gain          │
          │                   │
          ▼                   │
    Gamma Correction          │
          │                   │
          └─────────┬─────────┘
                    ▼
          Enhanced Image
```

The diffusion output is therefore **not treated as the final enhanced image**.

Instead, it acts as a **noiseless structural proxy** that preserves important scene structures and high-frequency details.

The actual enhancement is then performed using a physically interpretable recovery process.

---

# 🔬 Methodology

π-Diff consists of two major concepts:

1. **Structure-preserving diffusion prior**
2. **Physics-inspired enhancement**

---

## 1. Structure-Preserving Diffusion Prior

The diffusion model is used to obtain a clean structural representation from the low-light image.

Instead of forcing the diffusion model to predict the correct exposure and color directly, π-Diff uses its reverse diffusion output as a structural reference.

### Main purpose

The structural proxy helps preserve:

* Edges
* Fine details
* Object boundaries
* High-frequency structures
* Scene geometry

This reduces the tendency of the enhancement process to generate blurry or hallucinated content.

---

## 2. Intrinsic Image Decomposition

The image is decomposed into two important components:

### Illumination

Represents the amount of light present in different regions of the scene.

```text
Illumination → Lighting / Exposure
```

### Reflectance

Represents the underlying scene properties and colors.

```text
Reflectance → Object / Scene Color
```

Conceptually:

```text
Image ≈ Reflectance × Illumination
```

This decomposition allows π-Diff to treat **lighting and color separately**.

---

## 3. Adaptive Light Gain

Low-light images suffer from insufficient photon information.

π-Diff dynamically estimates an **adaptive light gain** to compensate for insufficient illumination.

Instead of applying a fixed brightness increase to the entire image, the enhancement adapts to the estimated lighting conditions.

Conceptually:

```text
Low Illumination
       ↓
Estimate Light Gain
       ↓
Adaptive Illumination Compensation
       ↓
Improved Exposure
```

This provides a more physically meaningful way of recovering brightness.

---

## 4. Gamma Correction

After illumination compensation, gamma correction is applied for exposure remapping.

Gamma correction helps transform the intensity distribution into a more visually appropriate range.

Conceptually:

```text
Input Illumination
       ↓
Adaptive Gain
       ↓
Gamma Correction
       ↓
Enhanced Illumination
```

This avoids simply increasing brightness uniformly across the entire image.

---

## 5. Reflectance Consistency

A major objective of π-Diff is to preserve the consistency of the reflectance map.

Because reflectance contains information about the scene's intrinsic colors, maintaining it helps prevent:

* Color shifts
* Unnatural tones
* Over-saturation
* Color distortion

Therefore, illumination can be enhanced while the underlying scene colors remain stable.

---

# 🧠 Why π-Diff Works

The key advantage comes from **decoupling structural restoration from image enhancement**.

| Problem                  | Why It Happens                                                                 | π-Diff Solution                                |
| ------------------------ | ------------------------------------------------------------------------------ | ---------------------------------------------- |
| **Blurry details**       | Diffusion networks may average across multiple plausible solutions             | Use the diffusion output as a structural proxy |
| **Hallucinated content** | Low-light enhancement is an ill-posed problem with multiple possible solutions | Decouple denoising from enhancement            |
| **Color distortion**     | Denoising, illumination and color restoration are mixed together               | Separate reflectance from illumination         |
| **Under-exposure**       | Direct generation may not correctly estimate lighting                          | Adaptive light gain + gamma correction         |
| **Detail loss**          | Enhancement can alter high-frequency structures                                | Structure-preserving diffusion prior           |

---

# 🔄 Overall Pipeline

The complete π-Diff process can be summarized as:

```text
                Low-Light Input
                       │
                       ▼
              Diffusion Prior
                       │
                       ▼
          Noiseless Structural Proxy
                       │
                       ▼
            Intrinsic Decomposition
                 ┌─────┴─────┐
                 ▼           ▼
          Illumination    Reflectance
                 │           │
                 ▼           │
        Adaptive Light Gain  │
                 │           │
                 ▼           │
          Gamma Correction   │
                 │           │
                 └─────┬─────┘
                       ▼
              Enhanced Image
```

The important design principle is:

> **Diffusion provides structural information, while the physically-inspired recovery process performs enhancement.**

---

# ⚖️ Comparison with Existing Methods

| Method        | Type                         | Key Approach                                      | Main Issue                        |
| ------------- | ---------------------------- | ------------------------------------------------- | --------------------------------- |
| **GDP**       | Unsupervised Diffusion Model | Simple degradation model + direct generation      | Direct generation                 |
| **AGLLDiff**  | Unsupervised Diffusion Model | Diffusion-based enhancement                       | Potential hallucination           |
| **QuadPrior** | Unsupervised Diffusion Model | Stable Diffusion prior                            | Still relies on direct generation |
| **π-Diff**    | Unsupervised Diffusion Model | Structural proxy + physical ISP-inspired recovery | Sampling-dependent inference      |

### Key Difference

```text
GDP
Low-Light → Diffusion → Enhanced Image

AGLLDiff
Low-Light → Diffusion → Enhanced Image

QuadPrior
Low-Light → Stable Diffusion → Enhanced Image

π-Diff
Low-Light
    ↓
Diffusion
    ↓
Structural Proxy
    ↓
Physical Enhancement
    ↓
Enhanced Image
```

π-Diff therefore separates **what should be structurally recovered** from **how the image should be physically enhanced**.

---

# ✅ Strengths

### No Paired Training Data

π-Diff is an **unsupervised and training-free framework**, reducing the need for paired low-light/normal-light training datasets.

### Fine-Grained Detail Preservation

Using the diffusion output as a structural proxy helps retain fine image structures and high-frequency details.

### Physically Interpretable

The enhancement process is based on concepts such as:

* Illumination
* Reflectance
* Adaptive light gain
* Gamma correction
* ISP-inspired processing

This makes the enhancement process more interpretable than purely generative approaches.

### Reduced Hallucination

Because the diffusion model is not directly responsible for generating the final enhanced image, the framework reduces the risk of introducing unsupported visual content.

### Real-World Applicability

The framework is evaluated on public real-world low-light image benchmarks and demonstrates strong enhancement performance.

---

# ⚠️ Limitations

Despite its advantages, π-Diff has several limitations.

### Long Inference Time

Diffusion-based processing can require multiple sampling steps, resulting in relatively high inference time.

### Sampling Dependency

The estimation of enhancement parameters can depend on the diffusion sampling process.

### Computational Cost

Diffusion models generally require more computation than conventional single-pass enhancement networks.

### Multiple Diffusion Steps

The framework may require sufficient diffusion steps to obtain a reliable structural proxy, which can limit real-time deployment.

---

# 📊 Results

π-Diff demonstrates competitive performance against existing low-light image enhancement approaches on public real-world benchmarks.

The reported results show that the proposed combination of:

* Diffusion-based structural priors
* Intrinsic decomposition
* Adaptive illumination compensation
* Gamma correction
* Reflectance consistency

can produce visually improved low-light images while preserving structural details.

---

# 🎯 Main Contributions

The main contributions of π-Diff can be summarized as follows:

1. **Structural Proxy Paradigm**
   Reinterprets the reverse diffusion output as a **noiseless structural proxy** instead of directly using it as the final enhanced image.

2. **Physics-Inspired Enhancement**
   Introduces an ISP-inspired recovery strategy based on illumination estimation, adaptive light gain and gamma correction.

3. **Structure and Color Preservation**
   Separates illumination enhancement from reflectance to maintain intrinsic scene colors and fine-grained structures.

4. **Training-Free Framework**
   Performs low-light enhancement without requiring paired training data.

5. **Strong Real-World Performance**
   Demonstrates competitive results on public real-world LLIE benchmarks.

---

# 🧩 Core Concept in One Sentence

> **π-Diff uses diffusion models to recover a clean structural representation and then performs physically-inspired illumination enhancement, rather than asking the diffusion model to directly generate the final bright image.**

---

# 📚 Summary

π-Diff presents a different way of applying diffusion models to low-light image enhancement.

Instead of:

```text
Diffusion Model
      ↓
Direct Image Generation
```

it proposes:

```text
Diffusion Model
      ↓
Structural Proxy
      ↓
Physical ISP-Inspired Recovery
      ↓
Enhanced Image
```

This decoupling helps address common problems associated with direct diffusion-based enhancement, including **blurred details, hallucination, color distortion and incorrect exposure**.

The combination of **structure-preserving diffusion priors** and **physically interpretable enhancement** makes π-Diff an interesting approach for real-world low-light image restoration.

---
