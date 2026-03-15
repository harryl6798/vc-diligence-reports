# Technical Blueprint: Building a Vision-Language AI for Radiology
**Project Name:** Mecha-style Radiology Platform  
**Target Goal:** Sub-2 second automated diagnostic draft report generation with full mechanistic interpretability.

---

## 1. System Architecture: The "Quiet Integration"
To succeed in a clinical setting, the system must operate as a background service that "injects" value into existing tools (PACS/RIS) without requiring a new UI.

### **A. Data Ingestion Layer (The Gateway)**
*   **DICOM Router:** Deploy an on-premise or cloud-native DICOM router (e.g., **Orthanc** or **dcm4che**). This service listens for `C-STORE` requests from the hospital's Modality (X-ray/CT) or PACS.
*   **HL7 Listener:** Implement an HL7 engine (**Mirth Connect**) to parse `ORM` (Order) and `ADT` (Admission) messages. This provides the "Indication" (e.g., "Rule out pneumonia") and patient history required for context-aware reporting.
*   **De-identification:** A critical step before cloud processing. Use pixel-level scrubbing and metadata cleaning (removing tags like PatientName, DOB) while preserving the `StudyInstanceUID` for reconciliation.

### **B. AI Inference Pipeline**
*   **Model Router:** An orchestrator (e.g., **NVIDIA Triton Inference Server**) that selects the appropriate model based on the DICOM `Modality` and `BodyPartExamined` tags.
*   **Pre-processing:**
    *   **Normalization:** Scaling pixel values (0-1) and resizing to model input (e.g., 512x512 or 1024x1024).
    *   **Slice Selection (CT/3D):** For 3D volumes, use a lightweight "selector" model to identify key slices or generate MIPs (Maximum Intensity Projections) to reduce compute payload.

---

## 2. Model Architecture: MechaNet-style VLMs
The core engine is a **Vision-Language Model (VLM)** that bridges the gap between medical imaging (CV) and clinical language (NLP).

### **A. Vision Encoder (The "Eyes")**
*   **Architecture:** Vision Transformer (**ViT**) or **DenseNet-121** pre-trained on a massive corpus like **MIMIC-CXR** or **Open-I**.
*   **Feature Extraction:** Instead of a simple classification head, the encoder outputs a "latent representation" (a high-dimensional vector) of the image features.

### **B. Language Decoder (The "Voice")**
*   **Backbone:** A medical-tuned LLM (e.g., **Llama 3 8B** or **Mistral 7B**).
*   **Projection Layer:** A linear layer or MLP that maps the Vision Encoder's output into the same "embedding space" as the Language Decoder. This allows the LLM to "see" the image as if it were a sequence of tokens.

### **C. Prior-Aware Reasoning**
*   **Mechanism:** Feed the *current* image embedding and the *previous* report text into the LLM context. This allows the model to generate sentences like "Pleural effusion is stable compared to the study from [Date]."

---

## 3. The Interpretability Layer: SAE-Rad
To prevent "hallucinations" and build clinician trust, we implement **Sparse Autoencoders (SAEs)**.

### **How it works:**
1.  **Decomposition:** The SAE takes the high-dimensional vector from the Vision Encoder and breaks it down into ~16,000 "sparse features."
2.  **Activation:** Only a few features (e.g., 15-20) activate for any given image.
3.  **Mapping:** Each feature is pre-mapped to a clinical concept (e.g., "Feature #402 = Left lower lobe opacity").
4.  **Traceability:** The final report is generated *only* from these activated, labeled features. If the AI claims there is a "pacemaker," the radiologist can click the sentence and see exactly which pixels in the "pacemaker feature" were activated.

---

## 4. Output & Integration
*   **Draft Generation:** The LLM outputs a structured report in the hospital's specific template format (e.g., Findings, Impression, Recommendations).
*   **Delivery:**
    *   **HL7 ORU:** Send the text back to the RIS (Radiology Information System) as a "Preliminary Draft."
    *   **DICOM SR:** Send structured data (measurements) back to the PACS.
*   **Nuance PowerScribe Integration:** Use the PowerScribe SDK to auto-populate the radiologist's reporting window the moment they open the study.

---

## 5. Technical Stack Summary
*   **Frameworks:** PyTorch, HuggingFace Transformers.
*   **Infrastructure:** AWS G5/P4 instances (A100/H100 GPUs).
*   **Orchestration:** Kubernetes + NVIDIA Triton.
*   **Standards:** DICOM, HL7, FHIR.
*   **Compliance:** SOC 2 Type II, HIPAA-encrypted S3 buckets, VPC isolation.

---

## 6. AI Use Cases (Investor Roadmap)
1.  **Triage:** Automatically flag urgent findings (Pneumothorax, ICH) to the top of the worklist.
2.  **Drafting:** Complete 80% of the "negative" (normal) reports, which represent 60-70% of hospital volume.
3.  **QA:** Real-time checking of human reports for discrepancies (e.g., radiologist says "Left lung" but AI sees "Right lung").
4.  **Teaching:** Providing "visual evidence" for junior residents to help them learn faster.
