# Mecha Health: Visual Design Specification (Phase 10.5)

To ensure the visual arguments in our master deliverables capture the intimate technical and commercial details synthesized from our atomic fact harvest, we define the following specifications.

## 1. Technical Architecture: The SAE-Rad Pipeline
This diagram visualizes the transition from "Black Box VLM" to "Mechanistic Interpretability."
- **Input Node**: "Raw DICOM Image (Pixels/Voxels)" with a sub-note "Zero-Touch PHI Scrub".
- **The Engine (The Gated Stack)**:
    - **Base Layer**: "Vision Transformer (ViT)" -> Extracts dense latent representations.
    - **The Moat Layer**: "Gated Sparse Autoencoder (SAE)" -> Decomposes dense latents into 15-20 interpretable, discrete clinical features (e.g., Feature #1024: Pleural Effusion).
    - **Aggregation Layer**: "Off-the-shelf LLM" -> Compiles discrete features into a fluent report.
- **Output Node**: "Draft Radiology Report" mapped to "HL7 FHIR DiagnosticReport".
- **Visual Logic**: Use an "Assembly Line" pattern. Emphasize the "SAE Layer" as the proprietary moat that provides interpretability.

## 2. Market Dynamics: The Displacement Path
This diagram maps how Mecha Health is displacing legacy dictation workflows.
- **Legacy Quadrant (Slow Giant)**: "Nuance/PowerScribe (Microsoft)" and "Aidoc". Label: "Manual Dictation & Triage."
- **Disruption Quadrant (Mecha Health)**: "Mecha Net XR". Label: "Automated Draft Generation."
- **Displacement Path**: Arrows showing the workflow shift from "Radiologist as Creator" (speaking every finding) to "Radiologist as Editor" (reviewing a pre-filled draft).
- **Nodes**: Annotate Nuance with "Vulnerability: High Time Cost" and Mecha with "Wedge: 12x Productivity Multiplier."

## 3. Growth & Unit Economics: The Efficiency Curve
This graph proves the capital efficiency of Mecha Health.
- **X-Axis**: Time (2024 Launch to 2025E).
- **Y-Axis**: Scans Processed / Throughput.
- **Trajectory Line**: Showing the leap from "1 scan / hour" to "1 scan / 5 mins."
- **Milestones**: 
    - **Nov 2025**: $4.1M Seed (Valia Ventures).
    - **Early 2026**: AmeriRad Partnership.
- **Annotated Margin**: A side-bar showing the "Compute-Efficiency Moat" (Model is 100x smaller than Med-PaLM, leading to low GPU burn and high gross margins on a $0.50-$2.00 per-scan pricing model).
