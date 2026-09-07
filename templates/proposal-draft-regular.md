# Final Year Project Proposal Draft (Research Template)

## Project Registration (Draft)
- **Title of Project:** Identity-Preserving Adaptive Surveillance Restoration & Recognition Pipeline
- **Session:** Fall 2026
- **Program/Department:** BS AI/CS (FAST School of Computing, Karachi)
- **Type of Project (recommended):** **Industrial**
- **Nature of Project:** **Research & Development**
- **SDGs (recommended):** **Industry, Innovation and Infrastructure (SDG 9)** and **Decent Work and Economic Growth (SDG 8)** *(please confirm final selection)*
- **Area of Specialization:** **Artificial Intelligence (AI), Data Science and Analytics**
- **Project Group Members:** PLACEHOLDER: Group member details (Reg. #, names, emails, phone, CGPA, signatures)

## Project Abstract (250–300 words)
Low-quality surveillance imagery (low resolution, motion blur, noise, and poor illumination) significantly degrades face recognition reliability in real-world CCTV settings. Existing restoration studies often optimize visual quality metrics only, while recognition studies commonly evaluate on cleaner or synthetically degraded data. This creates a practical research gap: visually improved outputs may still alter identity-discriminative features and reduce recognition reliability.

This project proposes an identity-preserving, adaptive surveillance pipeline based on Approach B. A frame-quality assessment stage first estimates degradation severity and controls conditional restoration. For restoration, diffusion-based modules are used for super-resolution, deblurring, and low-light enhancement (DreamSR/InvSR, ID-CDM, and π-Diff). Restored and non-restored branches are then passed to face detection (RetinaFace and SFE-DETR), alignment, and recognition ablations (ArcFace, AdaFace, MagFace, CurricularFace, PartialFC). Identity retrieval is performed using embedding search with FAISS/HNSW, while BoT-SORT maintains temporal identity consistency across frames.

The key contribution is a task-aware evaluation framework that jointly reports image quality (PSNR, SSIM, LPIPS) and downstream identity performance (Rank-1, TAR@FAR), with emphasis on surveillance-native benchmarks such as QMUL-SurvFace and QMUL-TinyFace. This allows direct measurement of whether restoration helps or harms recognition in realistic conditions. The expected outcome is a validated pipeline and comparative evidence on when advanced restoration should be applied, skipped, or constrained to preserve identity integrity.

## Introduction
Surveillance analytics increasingly depends on reliable person identification in unconstrained environments. However, CCTV data is often degraded by low resolution, blur, compression, occlusion, and poor lighting. These conditions reduce detection and recognition performance and make identity decisions less trustworthy.

Recent advances in diffusion-based restoration have improved visual fidelity, but high perceptual quality does not always translate to better biometric reliability. In identity-sensitive applications, preserving discriminative facial structure is more important than perceptual sharpness alone. Therefore, this project investigates a principled way to combine modern restoration with recognition under a controlled, evaluation-driven pipeline.

## Problem Statement
Current surveillance face pipelines face a methodological gap: restoration models are usually evaluated by visual metrics, while recognition systems are evaluated separately. There is limited evidence showing whether restoration consistently improves recognition on native surveillance imagery without introducing identity-altering artifacts. The project addresses this by building and evaluating a unified, adaptive pipeline centered on identity preservation.

## Related Work
Literature indicates strong progress in modular restoration and recognition components. Real-ESRGAN provides practical blind SR baselines, while DreamSR/InvSR and one-step diffusion variants improve detail realism. ID-CDM introduces zero-shot realistic deblurring and π-Diff provides structure-preserving low-light enhancement. For recognition, ArcFace established angular-margin learning, and AdaFace/MagFace/CurricularFace improved robustness under varying image quality. Tracking methods such as ByteTrack and BoT-SORT support temporal consistency.

Despite these advances, most prior works either optimize standalone stages or evaluate using non-surveillance-centric datasets. The proposed work differs by combining adaptive restoration control, stage-wise ablations, and surveillance-native recognition evaluation.

## Project Rationale
The project is motivated by the need for trustworthy AI in surveillance, where incorrect identity inference has high operational and ethical cost. By integrating restoration and recognition under a common identity-aware protocol, the study can produce practical guidance on safe restoration usage.

## Aims and Objectives
1. Design an adaptive CCTV pipeline using quality-gated restoration.
2. Implement diffusion-based and baseline modules for SR, deblurring, and low-light enhancement.
3. Compare detector and recognizer variants under shared preprocessing.
4. Evaluate visual quality and identity performance jointly.
5. Quantify restoration trade-offs and define identity-safe operating conditions.

## Scope of the Project
Included scope: face-focused surveillance enhancement, detection, alignment, recognition, tracking, and identity-aware evaluation on public benchmarks and curated test data. Excluded scope: legal deployment policy design, hardware event-camera pipelines, and large-scale production deployment.

## Proposed Methodology and Architecture
1. Data preparation and protocol setup (surveillance-native splits).
2. Frame quality assessment and restoration gating.
3. Conditional restoration (SR, deblur, low-light).
4. Face detection and alignment.
5. Recognition embedding generation and gallery matching.
6. Multi-frame tracking and ID association.
7. Ablation experiments and metric reporting.
8. Statistical analysis of identity gains vs hallucination risks.

## Individual Tasks (Draft)
| Team Member | Activity | Tentative Date |
|---|---|---|
| Member 1 (Lead) | Pipeline integration + experiment orchestration | Sep–Oct 2026 |
| Member 2 | Restoration module setup and tuning | Sep–Nov 2026 |
| Member 3 | Recognition/tracking evaluation and analysis | Oct–Nov 2026 |
| All | Report writing, validation, final presentation | Nov–Dec 2026 |

## Gantt Chart (Narrative Draft)
- **Sep 2026:** Literature finalization, dataset setup, baseline implementation.
- **Oct 2026:** Restoration + detection/recognition integration.
- **Nov 2026:** Ablations, benchmark evaluation, error analysis.
- **Dec 2026:** Final report, demo, and defense preparation.

## Tools and Technologies
Python, PyTorch, OpenCV, diffusers/xformers, FAISS/HNSW, Jupyter, Weights & Biases/TensorBoard, Git/GitHub, CUDA-enabled GPU environment.

## Expected Results
- A reproducible identity-aware surveillance pipeline.
- Comparative performance report for baseline vs diffusion restoration.
- Evidence-based recommendations on adaptive restoration policy.
- Final FYP report and demonstrable prototype.

## References (IEEE-style draft list)
[1] X. Wang et al., “Real-ESRGAN: Training Real-World Blind Super-Resolution with Pure Synthetic Data,” ICCVW, 2021.
[2] Z. Dong et al., “DreamSR: Towards Ultra-High-Resolution Image Super-Resolution via a Receptive-Field Enhanced Diffusion Transformer,” CVPR, 2026.
[3] Z. Yue et al., “InvSR: Arbitrary-steps Image Super-resolution via Diffusion Inversion,” CVPR, 2025.
[4] X. Wang, Y. Chen and Y. Dai, “Zero-Shot Realistic Image Deblurring with Consistency Model (ID-CDM),” Complex & Intelligent Systems, 2025.
[5] Y. Wang et al., “π-Diff: Physically-Inspired Low-Light Image Enhancement with Structure-Preserving Diffusion Priors,” CVPR Workshop, 2026.
[6] J. Deng et al., “RetinaFace: Single-Shot Multi-Level Face Localisation in the Wild,” CVPR, 2020.
[7] J. Yang, C. Jiang and X. Song, “SFE-DETR: An Enhanced Transformer-Based Face Detector for Small Target Faces in Open Complex Scenes,” Sensors, 2025/2026.
[8] J. Deng et al., “ArcFace: Additive Angular Margin Loss for Deep Face Recognition,” CVPR, 2019.
[9] M. Kim et al., “AdaFace: Quality Adaptive Margin for Face Recognition,” CVPR, 2022.
[10] N. Aharon et al., “BoT-SORT: Robust Associations Multi-Pedestrian Tracking,” arXiv, 2022.
