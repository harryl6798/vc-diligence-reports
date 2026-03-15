# Cashmere.io - Wave 2 & 3: Founder Alpha & Technical Architecture

## 1. Founder Alpha (The "Publishing-AI" Bridge)

### Jonathan Munk (CEO & Co-founder)
- **Background**: 15+ years in EdTech, Consumer Tech, and Strategy.
- **Key Signal (EdTech Alpha)**: Spent 6 years at **Degreed** (LXP leader) as Chief Brand Officer and GM of New Services. Degreed's focus on "skills-based learning" is a precursor to the granular "inference-based" knowledge Cashmere enables.
- **Key Signal (Execution Alpha)**: CEO of **BookClub** (2022-2024), where he scaled social learning for enterprises. 
- **Right to Win**: Munk understands the "Economic Buyer" in EdTech and Publishing (Pearson, Wiley, etc.) better than a pure AI researcher.

### Jonathan Woahn (Co-founder)
- **Background**: 5+ years specifically at the intersection of Books and AI.
- **Key Signal (Thought Leadership)**: A leading voice in the Society for Scholarly Publishing (SSP). His "3C's Framework" (Consent, Credit, Compensation) is the philosophical foundation of Cashmere.
- **Right to Win**: Woahn has built the "Social Capital" within the publishing industry required to secure high-value licenses (Wiley, Harvard Business Publishing).

---

## 2. Technical Architecture (The Infrastructure Moat)

Cashmere is not a "wrapper"; it is a **middleware layer** for "Inference-Time Licensing."

### 2.1 Omnipub: The AI-Native Format
- **Function**: Converts unstructured text (PDF/EPUB) into a **proprietary Knowledge Graph**.
- **Differentiation**: Instead of bulk vector embeddings (standard RAG), Omnipub maintains semantic relationships and granular metadata (e.g., "this is a clinical reference," "this is a summary").
- **Token-Level Tracking**: Enables billing and rights enforcement at the individual token level.

### 2.2 Fiber: The Licensed Discovery Gateway
- **Function**: A governance layer that sits between LLMs and the Omnipub catalog.
- **Mechanism**: When an AI (e.g., Perplexity) requests data, Fiber:
  1. Checks for a valid license (BYOL or Enterprise).
  2. Retrieves *only* the authorized snippets (never the full text).
  3. Ensures attribution/citation is embedded in the response.
- **Security**: Full-text archives are never exposed to the LLM provider, preventing "model training leakage."

### 2.3 Business Model (The Inference Shift)
- **Thesis**: The market for *using* content (inference) is 100x larger than the market for *training* on content.
- **Revenue Model**:
  - **Usage-Based**: Pay-per-query or pay-per-token fees charged to AI companies.
  - **Revenue Share**: A cut of the licensing fees facilitated for publishers.
  - **Enterprise SaaS**: Subscription fees for publishers to manage their digital rights via the Cashmere dashboard.

---

## 3. Evidence Audit
- **Moat Strength**: HIGH. The "Omnipub" format and existing partnerships (Perplexity, Wiley) create a network effect.
- **Risk**: Adoption speed. If AI companies refuse to pay for "inference" and stick to scraped data, the model stalls.
- **Gap**: Direct unit economics (e.g., "What is the margin on a $0.05 query?").
