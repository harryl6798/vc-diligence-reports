# Mecha Health: Technical Component Teardown

## 1. The Core AI Engine: SAE-Rad & Mechanistic Interpretability
The technical moat of Mecha Health rests on a proprietary framework known in academia as **SAE-Rad** (Sparse Autoencoders for Interpretable Radiology). This is a stark departure from competitors who use "black box" Vision-Language Models (VLMs) like GPT-4V or Gemini Pro.

### 1.1 The Sparse Autoencoder (SAE) Pipeline
- **Base Vision Model**: Uses a pre-trained Vision Transformer (ViT) to process the raw pixels of a DICOM image (e.g., a chest X-ray).
- **The SAE Layer**: Instead of feeding the ViT's dense, uninterpretable latent representations directly into a language model, Mecha uses **Gated Sparse Autoencoders**. This layer "undoes" the superposition of features, forcing the model to activate only a sparse handful of highly interpretable, discrete concepts (e.g., Feature #1024 = "Pleural Effusion," Feature #56 = "Enlarged Heart").
- **LLM Aggregation**: An off-the-shelf LLM is then used simply to compile these discrete, verified features into a fluent, structured medical report.

### 1.2 The "Interpretability" Moat
By decomposing the image into discrete features *before* text generation, the system achieves **Mechanistic Interpretability**. A radiologist can trace any sentence in the draft report back to the exact physical pixels that activated the specific SAE feature. This eliminates the "hallucination" risk inherent in standard VLMs and builds clinical trust.

## 2. Infrastructure & Data Plumbing

### 2.1 Clinical Integration (DICOM & HL7/FHIR)
- **Ingestion**: Retrieves medical images directly from Picture Archiving and Communication Systems (PACS) using native **DICOM** and **DICOMweb** protocols. The median ingestion time is under 2 seconds.
- **Privacy Layer**: Includes a "zero-touch" Protected Health Information (PHI) scrubbing module that de-identifies DICOM metadata before the image hits the inference engine.
- **Delivery**: The output is not trapped in a separate dashboard. Mecha maps the generated report to **HL7 FHIR** resources (`ImagingStudy`, `Observation`, `DiagnosticReport`) or legacy **HL7 v2 (ORM/ORU)** messages, injecting the draft directly into the radiologist's existing dictation template.

## 3. The Compute-Efficiency Moat
Because Mecha Net (SAE-Rad) relies on finding 15-20 sparse features rather than processing billions of dense visual tokens through a massive multimodal LLM, the model is **two orders of magnitude smaller** than Google's Med-PaLM or OpenAI's GPT-4V. This results in:
1.  **Lower Inference Costs**: GPU burn per scan is drastically reduced, enabling higher gross margins.
2.  **Edge/On-Premise Potential**: The smaller footprint allows the model to run closer to the data (e.g., within a hospital's Enterprise VPC), easing compliance and security concerns.
