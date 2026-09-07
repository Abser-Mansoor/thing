# Final Year Project Proposal Draft (Research & Development Template)

## Project Registration (Draft)
- **Title of Project:** Identity-Preserving Adaptive Surveillance Restoration & Recognition Pipeline
- **Session:** Fall 2026
- **Type of Project (recommended):** **Traditional** *(please confirm: Traditional / Industrial / Continuing)*
- **Nature of Project:** **Research & Development**
- **SDGs (recommended):** **Industry, Innovation and Infrastructure (SDG 9)** and **Decent Work and Economic Growth (SDG 8)** *(please confirm final selection)*
- **Area of Specialization:** **Artificial Intelligence (AI), Data Science and Analytics**
- **Project Group Members:** *To be inserted from Readme (Reg. #, names, emails, phone, CGPA, signatures)*

## Project Abstract
This project develops and evaluates an identity-preserving surveillance analytics pipeline for degraded CCTV imagery. The system targets low resolution, blur, and poor lighting while maintaining recognition reliability. A quality-aware control stage decides whether and which restoration modules to apply. The restoration branch uses advanced models for super-resolution, deblurring, and low-light enhancement; the recognition branch performs face detection, alignment, embedding generation, and identity matching with temporal tracking.

The development contribution is a modular, end-to-end implementation where each stage can be swapped and benchmarked. The research contribution is an evaluation protocol that links restoration quality with downstream identity performance. Instead of reporting only image metrics, the project quantifies recognition impact under realistic surveillance settings. Expected outputs include a working prototype pipeline, comparative benchmarks of component choices, and deployment-oriented insights for adaptive restoration policy.

## Introduction
AI-based surveillance systems require robust recognition in uncontrolled conditions. Traditional face pipelines degrade when inputs are noisy, blurred, or dark. Recent restoration models can improve visuals but may alter identity-critical details. Therefore, the project combines practical software development with empirical research to build a reliable adaptive pipeline and measure identity-preserving performance.

## Problem Statement
There is a gap between restoration quality optimization and identity-aware system validation in surveillance. Existing methods rarely provide integrated evidence that enhancement improves recognition on native surveillance data without causing identity drift. The project addresses this by implementing a controllable full pipeline and testing it with joint visual and biometric metrics.

## Motivation
The project aims to reduce manual forensic effort, improve recognition reliability, and provide a structured framework for evaluating restoration safety in identity-sensitive applications. It also offers hands-on development of a deployable AI pipeline with measurable research value.

## Aims and Objectives
1. Build a modular surveillance restoration-recognition pipeline.
2. Add quality-gated adaptive logic for conditional restoration.
3. Integrate alternative models for key stages and run ablations.
4. Evaluate system behavior on realistic surveillance benchmarks.
5. Produce final technical report, code artifacts, and demonstrable prototype.

## Scope of the Project
- In scope: restoration, detection, recognition, tracking, and quantitative evaluation.
- Out of scope: production-scale cloud deployment, policy/legal governance modules, and event-camera-only methods.

## Proposed Methodology
- Requirement analysis and dataset protocol design.
- Module integration for restoration and recognition.
- Quality-gated orchestration logic.
- Controlled experiments and cross-model ablation.
- Result analysis, optimization, and documentation.

## System Architecture
Proposed architecture is a modular pipeline with these logical layers:
1. **Input & Quality Layer:** ingest CCTV frames and estimate degradation.
2. **Adaptive Restoration Layer:** apply/skip SR, deblurring, and low-light enhancement.
3. **Face Analytics Layer:** detect, align, recognize, and match identities.
4. **Tracking & Decision Layer:** maintain temporal identity consistency and output ranked matches.
5. **Evaluation Layer:** report image and identity metrics for each configuration.

Workflow: **Frame → Quality Assessment → Conditional Restoration → Detection/Alignment → Recognition/Matching → Tracking → Evaluation**.

## Individual Tasks (Draft)
| Team Member | Activity | Tentative Date |
|---|---|---|
| Member 1 (Lead) | Core architecture, module integration, experiment management | Sep–Oct 2026 |
| Member 2 | Restoration branch implementation and tuning | Sep–Nov 2026 |
| Member 3 | Recognition, matching, tracking, and analytics dashboards | Oct–Nov 2026 |
| All | Testing, documentation, final demo and defense prep | Nov–Dec 2026 |

## Gantt Chart (Narrative Draft)
- **Sep:** Requirements, datasets, baseline setup.
- **Oct:** Feature-complete pipeline integration.
- **Nov:** Experimental runs, ablations, optimization.
- **Dec:** Consolidation, report, and presentation.

## Tools and Technologies
Python, PyTorch, OpenCV, diffusion libraries, FAISS, experiment tracking tools, Git/GitHub, CUDA GPU stack.

## Expected Deliverables
1. End-to-end prototype implementation.
2. Experimental benchmark report (image + identity metrics).
3. Comparative analysis of restoration strategies.
4. Final FYP documentation and presentation artifacts.

## References (if needed)
Use IEEE references for the core models used in implementation and evaluation (DreamSR, InvSR, ID-CDM, π-Diff, RetinaFace, ArcFace, AdaFace, BoT-SORT, and surveillance benchmarks).
