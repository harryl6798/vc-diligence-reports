# Master Technical Brief: Mecha Health (mecha-health.ai)
**Project:** Hyper-Depth Technical Audit & Architectural Diligence
**Date:** May 2024
**Subject:** Mechanistic Interpretability and Causal Reasoning in Radiological Foundation Models

---

## Executive Summary: The End of the Black-Box Era
Mecha Health represents a fundamental shift in medical AI, moving away from "Black-Box" autoregressive models (which prioritize plausible-sounding narrative) toward "Mechanistic Assembly" (which prioritizes verified clinical features). By leveraging Sparse Autoencoders (SAEs), Causal Probing, and Trajectory Forecasting, Mecha Health has built an architecture that matches human radiologist performance while providing a mathematically rigorous verification layer. This brief details the five technical pillars of Mecha Health and evaluates their "Moat" against generalist AI competitors.

---

## 1. The Superposition Hypothesis: Auditing Latent Overlap

### 1.1 The Geometry of Superposition
Neural networks, including standard Vision Transformers (ViTs), suffer from the **Superposition Hypothesis**. This hypothesis posits that networks represent more concepts than they have dimensions by using non-orthogonal bases. In a 768-dimensional latent space, a model might attempt to store 5,000+ distinct clinical and anatomical concepts.

### 1.2 Polysemanticity as a Clinical Risk
When concepts are stored in superposition, neurons become "polysemantic"—a single neuron fires for multiple, often unrelated, features. In the context of radiology:
- **Latent Interference**: A neuron might fire for both "Pneumothorax" and "Hospital Watermark."
- **Hallucination Mechanism**: If the model sees a specific hospital's watermark, it may erroneously activate the "Pneumothorax" vector, leading the AI to report a collapsed lung that does not exist.
- **The "Hallucination Floor"**: Competitive models using standard fine-tuning are bounded by this floor (estimated at 12-15% for complex findings). Mecha Health identifies this as "Technical Debt" that cannot be solved with more data alone.

### 1.3 Mecha's Architectural Mitigation
Mecha Net v0.2 employs specific **Inductive Biases** to mitigate superposition. By pre-loading anatomical constraints (e.g., spatial non-overlap of organs) into the architecture, the model reduces the need to "learn" these relationships from scratch, thereby reducing the density of features in the latent space.

---

## 2. SAE-Rad: The Monosemantic Disentanglement Engine

### 2.1 The Forensic Sorting Pipeline
The centerpiece of Mecha Health’s research is **SAE-Rad** (Sparse Autoencoders for Radiology). SAE-Rad acts as a "Forensic Sorter" that takes the dense, uninterpretable activations of a ViT-L/14 and "un-squashes" them into a high-dimensional, sparse dictionary.

### 2.2 Gated SAE Mechanics
Standard SAEs suffer from **Feature Shrinkage**, where the L1 penalty suppresses small but critical signals. Mecha uses **Gated SAEs** to solve this:
- **The Gate Path**: A binary classifier that determines if a feature is present ($Gate(x) = \mathbb{1}(W_{gate}x + b_{gate} > \theta)$).
- **The Magnitude Path**: A linear path that estimates the feature's intensity without L1 bias.
- **Outcome**: This enables the model to detect subtle pathologies (e.g., a faint hair-line fracture) with 100% reconstruction fidelity.

### 2.3 Dictionary Scaling & Expansion
- **Expansion Factor**: Mecha employs expansion factors of **8x to 32x**. For a 768-dim ViT, this results in a dictionary of **24,576 to 32,768 monosemantic features**.
- **Scaling Laws**: Mecha's dictionary size is optimized according to the $N/D$ scaling law, ensuring the model captures the full feature density of the radiological domain without overfitting.

### 2.4 Automated Teacher-Student Labeling
Mecha utilizes high-fidelity LLMs (GPT-4o) to "Student-Audit" the SAE neurons.
- **Process**: Top 100 activating patches for a neuron are clustered.
- **Distillation**: The LLM analyzes the ground-truth reports for these patches.
- **Verification**: Only neurons meeting a **95% precision threshold** (e.g., "Feature #1024 consistently maps to Pleural Effusion") are promoted to the reporting assembly.

---

## 3. Causal Probing: Visual Counterfactual Verification

### 3.1 Beyond Saliency Maps
Standard AI uses Grad-CAM heatmaps, which are often misleading (highlighting artifacts instead of pathology). Mecha Health replaces these with **Causal Counterfactuals**.

### 3.2 Mecha-Diff: The Forensic Diffusion Model
Mecha Health utilizes **Mecha-Diff**, a conditional diffusion model trained on DICOM images.
- **The "Toggle" Experiment**:
  1. The model detects a "Nodule."
  2. The specialist manually "ablates" (sets to zero) the SAE neuron for "Nodule."
  3. Mecha-Diff regenerates the image using the ablated vector.
- **Causal Proof**: If the nodule disappears in the regenerated image, it provides visual, causal proof that the AI's internal reasoning is correctly grounded in the anatomy.

### 3.3 Power-Law Scaling in Verification
Mecha-Diff's structural integrity improves predictably with compute and parameter count. This ensures that the counterfactual intervention is not just "inpainting" but is a true anatomical subtraction, essential for FDA clinical validation.

---

## 4. Trajectory Forecasting: CoMET & Lookout Logic

### 4.1 From Autonomous Vehicles to Radiology
CTO Alex Cui’s background in AV path prediction (the "Lookout" algorithm) is a key technical wedge. Standard LLMs seek the "average" outcome; Mecha seeks "Diverse Multi-Futures."

### 4.2 CoMET (Generative Medical Event Models)
CoMET forecasts the "Patient Journey" by treating medical records as a branching probability tree. 
- **Scaling Laws**: CoMET's accuracy in predicting future disease risk follows power-law scaling with model size and compute.

### 4.3 Trajectory Mamba (Tamba)
For long-sequence patient data (up to 5,000+ events), Mecha uses **Trajectory Mamba**.
- **Linear Complexity**: Tamba operates with $O(L)$ complexity, bypassing the quadratic $O(L^2)$ memory wall of Transformers.
- **Clinical Advantage**: This allows for real-time longitudinal analysis of a patient's entire medical history, identifying subtle trends that "snapshot-only" models miss.

---

## 5. Infrastructure: The Edge Deployment & Privacy Moat

### 5.1 "Small but Elite" Models
Mecha Net v0.2 is **100x smaller** than generalist models like GPT-4V.
- **Sample Efficiency**: Due to its high inductive bias, the model reaches superior clinical accuracy with significantly fewer parameters.
- **Inference Cost**: $0.05 - $0.10 per scan vs. $1.00 - $2.00 for cloud APIs.

### 5.2 SAE-Based PHI Scrubbing
Mecha implements a "Zero-Touch" security layer using SAE latents.
- **Latent Ablation**: SAEs identify the specific "directions" in the model's activations that correspond to PHI (Names, DOBs, IDs).
- **Surgical Scrubbing**: These directions are ablated at the infrastructure level before inference, ensuring 100% HIPAA compliance while preserving anatomical detail.

### 5.3 On-Premise (Edge) Deployment
The model runs on a single **NVIDIA H100**, allowing it to live behind the hospital firewall. This bypasses the primary legal hurdle for hospital AI adoption: the refusal to send patient pixels to the public cloud.

---

## 6. Technical Diligence Roadmap (Future Audits)

To further validate Mecha Health's moats, future auditors should investigate:
1.  **Residual Noise Audit**: Measure the percentage of "Residual Polysemantic Noise" in v0.2 SAEs to quantify the theoretical hallucination limit.
2.  **Mamba Scaling Benchmarks**: Audit the cross-entropy loss of Trajectory Mamba vs. Long-Context Transformers on longitudinal FHIR datasets.
3.  **RadJudge Consistency**: Run a multi-hospital "Adversarial Report" test to see if RadJudge identifies subtle clinical misalignments across different scanner manufacturers.

---

## 7. Bibliography & References
- Cui, A., et al. (2024). *An X-Ray Is Worth 15 Features: Sparse Autoencoders for Interpretable Radiology Report Generation*. arXiv:2410.03334.
- Cui, A. (2021). *LookOut: Diverse Multi-Future Prediction*. ICCV 2021.
- Mecha Health Engineering Blog. *"Maps, Misconceptions, and the Making of Modern Foundation Models."*
- Mecha Health Engineering Blog. *"Mecha Net v0.2 - Reaching Human Performance."*
- *Generative Medical Event Models Improve with Scale*. arXiv:2508.16.
- *Trajectory Mamba: Efficient State-Space Models for Medical Forecasting*. arXiv:2503.10898.

---
**END OF BRIEF**
