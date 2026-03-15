# Findings: Products and Services - Mecha Health

## Core Products
Mecha Health provides an end-to-end AI reporting platform designed to integrate seamlessly into existing radiology workflows.

### 1. MechaNet (XR & CT)
- **MechaNet XR:** A generalist foundation model for X-ray analysis (Chest, Abdomen, Spine, Extremities). It is a vision-language model (VLM) that generates full narrative reports from pixel data.
- **MechaNet CT:** A foundation model capable of reading entire CT volumes (e.g., Chest CT) and generating reports in seconds.

### 2. AI Reporting Platform
- **Integration:** Environment-agnostic platform that delivers draft reports directly into PACS (Picture Archiving and Communication Systems) and RIS (Radiology Information Systems).
- **Sub-Second Processing:** Images are processed in less than a second, ensuring a draft is ready the moment the radiologist opens the study.
- **Prior-Aware Comparison:** Automatically identifies clinically relevant changes by comparing the current scan with previous exams (priors).

### 3. CARE Score (Evaluation Tool)
The **Clinically Aligned Radiology Evaluation (CARE)** score is a proprietary metric used to assess the clinical quality of AI reports. It uses LLMs to normalize medical terminology and checks for bi-directional entailment between AI and ground-truth reports to identify hallucinations or omissions.

## Sources
- [Radiology Business - Mecha Health and AmeriRad Partnership](https://radiologybusiness.com)
- [Mecha Health - Product Page](https://mecha-health.ai/products)
- [American Radiology Solutions - News](https://americanradiologysolutions.com)
