# Master Technical Brief: Mecha Health (mecha-health.ai)
**Project:** Hyper-Depth Technical Audit & Architectural Diligence
**Date:** March 2026
**Subject:** Mechanistic Interpretability and Causal Reasoning in Radiological Foundation Models

---

## Executive Summary: The End of the Black-Box Era
Mecha Health represents a fundamental shift in medical AI, moving away from "Black-Box" autoregressive models toward "Mechanistic Assembly." By leveraging Sparse Autoencoders (SAEs), Causal Probing, and Trajectory Forecasting, Mecha Health has built an architecture that matches human radiologist performance while providing a mathematically rigorous verification layer. This brief provides a deep-dive educational walkthrough of the five technical pillars of Mecha Health.

---

## 1. Superposition Auditor: Functional Anatomy of Latent Overlap

### 1.1 Complexity Explanation: The "Overstuffed Suitcase"
Imagine trying to fit 50 outfits into a suitcase designed for 10. You can do it by folding them into each other, but when you pull one out, three others come with it. In a neural network, "Superposition" is this overstuffing. The model "folds" 5,000 clinical concepts into a 768-dimensional space. When it tries to "pull out" a pneumonia diagnosis, it accidentally pulls out "hospital watermark" or "rib shadow" because they are mathematically tangled.

### 1.2 Functional Anatomy
*   **Input**: Dense activations ($x \in \mathbb{R}^{d}$) from the penultimate layer of a Vision Transformer (ViT-L/14).
*   **Transformation**: The network employs a **Non-Orthogonal Basis**. Concepts are represented as vectors $\{v_i\}$ such that the number of concepts $M > d$.
*   **Output**: A polysemantic neuron activation where $h_j = \sigma(\sum_i W_{ji} x_i + b_j)$, where $h_j$ responds to multiple unrelated features.

### 1.3 Step-by-Step Walkthrough: The Interference Event
1.  **Feature Encoding**: The ViT processes a chest X-ray. A specific patch contains both a "Calcified Nodule" and a "Pacemaker Lead."
2.  **Vector Summation**: Because the model is in superposition, the vectors for "Nodule" ($v_{nodule}$) and "Pacemaker" ($v_{pace}$) are not orthogonal. Their sum $v_{total} = v_{nodule} + v_{pace}$ creates a new vector in the latent space.
3.  **Latent Interference**: The "Nodule" neuron fires, but because $v_{pace}$ has a high dot-product with the "Nodule" direction, the neuron fires *too strongly* or *too weakly*.
4.  **Diagnostic Hallucination**: The language decoder interprets this corrupted signal as a "Large Mass" (over-activation) or ignores the nodule entirely (under-activation), leading to a clinical error.

### 1.4 Mathematical Primitives
*   **The Superposition Equation**: $x = \sum_i f_i v_i$, where $f_i$ is the presence of feature $i$, and $v_i$ is its direction.
*   **Johnson-Lindenstrauss Lemma**: In high dimensions, there exist many "almost orthogonal" directions. Mecha exploits this by using SAEs to find these nearly-orthogonal directions that the original model "squashed."
*   **Interference Noise**: $noise = \sum_{j \neq i} f_j (v_i \cdot v_j)$. Mecha's goal is to minimize this dot product $(v_i \cdot v_j) \approx 0$ via disentanglement.

---

## 2. SAE-Rad Factory: The Disentanglement Assembly Line

### 2.1 Complexity Explanation: The "Prism of Truth"
If a standard AI model is a muddy pool of water, SAE-Rad is a prism. It takes a single beam of "messy" light (the model's hidden state) and splits it into a rainbow of distinct, individual colors (clinical features). You can then point to a specific "shade" of pneumonia and know exactly where it came from.

### 2.2 Functional Anatomy
*   **Input**: The high-dimensional, polysemantic activations ($x$) from the ViT backbone.
*   **Transformation (The Encoder)**: A linear projection followed by a "Gate" that forces sparsity.
*   **Transformation (The Decoder)**: A linear reconstruction $(\hat{x} = \sum f_i d_i)$ where $d_i$ are the "dictionary atoms."
*   **Output**: A sparse set of "Monosemantic Features" ($f$) where each feature corresponds to exactly one clinical concept.

### 2.3 Step-by-Step Walkthrough: The Forensic Sorting
1.  **Expansion**: The 768-dim vector $x$ is projected into a 32,768-dim space ($x \to \mathbb{R}^{32k}$).
2.  **Gated Activation**: The system applies a Gated ReLU. 
    *   **Step A (The Gate)**: A binary check $Gate(x) = \mathbb{1}(W_{gate}x + b_{gate} > 0)$ asks: "Is the Pneumothorax feature present?"
    *   **Step B (The Magnitude)**: If yes, the intensity is calculated $Mag(x) = ReLU(W_{mag}x + b_{mag})$.
3.  **L1 Regularization**: During training, the system is "punished" for every feature it turns on $(\mathcal{L}_{sparsity} = \lambda \sum |f_i|)$. This forces the model to be extremely efficient, turning on only the 15-20 most relevant features.
4.  **Feature Labeling**: An LLM "Student" looks at the top 100 images that activate Feature #402 and identifies they all contain "Chest Tubes." Feature #402 is now officially labeled "Presence of Chest Tube."

### 2.4 Mathematical Primitives
*   **The Reconstruction Loss**: $\mathcal{L}_{rec} = \|x - \sum f_i d_i\|_2^2$. This ensures no medical information is lost during the "un-squashing."
*   **The Gated SAE Formula**: $f = Gate(x) \odot Mag(x)$. This prevents "Feature Shrinkage," ensuring that even subtle, faint pathologies are detected at full strength.
*   **Expansion Factor**: $E = \frac{d_{SAE}}{d_{model}} = 32$. This represents the "Resolution" of the disentanglement.

---

## 3. Causal Probe Specialist: The Visual Verification Loop

### 3.1 Complexity Explanation: The "Toggle-able Truth"
Imagine a digital X-ray where every diagnosis has a physical "light switch." If the AI says there is a tumor, the doctor can flip the "tumor switch" to OFF. If the tumor disappears from the image, the doctor knows the AI's diagnosis is physically grounded in that specific area. This is a counterfactual test: "If this feature were gone, how would the image change?"

### 3.2 Functional Anatomy
*   **Input**: The monosemantic feature vector ($f$) from SAE-Rad and the original DICOM image.
*   **Transformation**: A **Conditional Diffusion Model** (Mecha-Diff) that generates images based on the latent directions found in the SAE.
*   **Output**: A counterfactual image $(\hat{I})$ showing the scene *without* a specific pathology.

### 3.3 Step-by-Step Walkthrough: Counterfactual Verification
1.  **Feature Identification**: The model detects a "Pneumothorax" (collapsed lung) and highlights the SAE feature $f_{901}$.
2.  **Vector Ablation**: The specialist manually sets $f_{901} = 0$.
3.  **Latent Projection**: This ablated vector is passed to the Diffusion Decoder's conditioning layer.
4.  **Forensic Generation**: Mecha-Diff performs reverse-diffusion (denoising) to reconstruct the X-ray. 
5.  **Clinical Proof**: The resulting image shows the same patient, same lung, but the "collapsed" edge is mathematically removed. This proves the model's claim is tied to those specific visual pixels.

### 3.4 Mathematical Primitives
*   **Ablation Vector**: $f_{ablated} = f \odot (\mathbf{1} - e_k)$, where $e_k$ is the one-hot vector for the target feature.
*   **Diffusion Conditioning**: $\epsilon_\theta(x_t, t, c=f_{ablated})$. The noise predictor is conditioned on the modified SAE features.
*   **Structural Fidelity Score**: $S = DSSIM(I, \hat{I})$. This measures if the image changed *only* in the pathology area, ensuring the "background" (the patient) remains identical.

---

## 4. LookOut Trajectory Analyst: The Path Forecasting Logic

### 4.1 Complexity Explanation: The "Patient Life-Line"
General AI treats a patient like a single snapshot in time. Mecha treats a patient like a moving car on a highway. Alex Cui’s "LookOut" algorithm doesn't just ask "What is here now?" but "Where is this patient heading?" It looks at a lung nodule today and forecasts the multiple "future paths" it could take, from benign shrinking to malignant growth.

### 4.2 Functional Anatomy
*   **Input**: Long-sequence patient data ($L \in \mathbb{R}^{5000+}$), including previous scans, blood labs, and physician notes.
*   **Transformation**: **Trajectory Mamba** (Tamba), a Selective State-Space Model (SSM).
*   **Output**: A branching "Probability Tree" of future medical events.

### 4.3 Step-by-Step Walkthrough: Multi-Future Prediction
1.  **Sequential Encoding**: Instead of processing patches, Tamba treats the patient's entire history as a continuous stream.
2.  **Selective Filtering**: The Mamba layer uses a "Selection Mechanism" to decide which events are clinical noise (e.g., "Routine checkup") vs. high-signal events (e.g., "Sudden weight loss").
3.  **Branching Forecasting**: The model generates $K$ discrete "future trajectories." 
    *   **Future A**: Nodule remains stable for 5 years.
    *   **Future B**: Nodule shows malignant characteristics in 6 months.
4.  **Diverse Trajectory Scoring**: Unlike LLMs that pick the "most likely" path, Mecha's engine scores the *variance* between these futures. High variance triggers an "Urgent Review" flag for the radiologist.

### 4.4 Mathematical Primitives
*   **Mamba Linear Complexity**: $O(L)$ vs. Transformer's $O(L^2)$. This allows Mecha to process a patient's entire decade-long medical record in milliseconds.
*   **The SSM Equation**: 
    *   $h_t = \mathbf{A} h_{t-1} + \mathbf{B} x_t$
    *   $y_t = \mathbf{C} h_t$
*   **Selection Logic**: $\mathbf{A}, \mathbf{B}, \mathbf{C}$ are functions of the input $x_t$, allowing the model to "reset" its state when a major clinical event occurs.

---

## 5. Mecha Infrastructure Expert: The Footprint Optimization Process

### 5.1 Complexity Explanation: The "Fortress on a Chip"
Traditional AI is like a giant factory in another city (the Cloud). You have to mail your data there and wait for it to come back. Mecha is a "Pocket Factory" that sits inside the hospital's own server room. It is small, fast, and secure. It scrubs out patient names and birthdays at the mathematical level, so no human (or hacker) can ever see them.

### 5.2 Functional Anatomy
*   **Input**: Raw DICOM pixel data and HL7 clinical feeds.
*   **Transformation**: Hybrid Backbone (ViT-Encoder + Mamba-Decoder) + SAE-based PHI Ablation.
*   **Output**: A de-identified, high-fidelity clinical draft report generated locally.

### 5.3 Step-by-Step Walkthrough: The Edge Ingest Cycle
1.  **On-Prem Ingest**: The hospital's X-ray machine sends images via DICOM `C-STORE` to the local Mecha node.
2.  **PHI Direction Ablation**: The model identifies the specific mathematical "directions" in the image activations that represent text (names, IDs). 
3.  **Surgical Scrubbing**: The system "zeroes out" these PHI directions *before* the data even touches the reporting model. This is more secure than simple black-box OCR.
4.  **Local Inference**: The report is generated on a single local GPU (e.g., NVIDIA H100), ensuring zero data-leakage to the public internet.
5.  **SDK Injection**: The resulting draft is pushed directly into the radiologist's software (Nuance PowerScribe) using a local API hook.

### 5.4 Mathematical Primitives
*   **PHI Direction Identification**: $\mathbf{d}_{PHI} = \mathbb{E}[x_{PHI}] - \mathbb{E}[x_{clean}]$. The system finds the vector that separates "Text" from "Anatomy" in the latent space.
*   **Latent Scrubbing**: $x_{safe} = x - (x \cdot \mathbf{d}_{PHI}) \mathbf{d}_{PHI}$. This projection "flattens" the name out of the data while leaving the lung pixels 100% intact.
*   **Throughput Density**: $TPS = \frac{Scans}{GPU \cdot Hour}$. Mecha's hybrid architecture achieves 10x higher TPS than cloud-based Transformers.

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
