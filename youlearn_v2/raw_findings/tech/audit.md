# Findings: YouLearn.ai - Technical Architecture & AI Impact Audit

## 1. System Architecture (The "Moat")
- **Architecture**: Source-Grounded RAG (Retrieval-Augmented Generation).
- **Engine**: Mixture-of-Agents (MoA) likely running on low-latency inference providers (Groq/Vercel).
- **Integrations**: Drop & Learn (PDF, YouTube, Audio).
- **Data Flywheel**: 2.2M materials uploaded. YouLearn is training on the *interaction data* between students and specific complex documents—a data moat that horizontal LLMs do not possess.

## 2. AI Impact & 2026 Trends
- **Agentic Shift**: YouLearn is moving toward **Autonomous Socratic Tutors**. Instead of answering, it guides.
- **Sovereign AI**: The model-agnostic approach (GPT-4, Claude, Gemini) allows them to pivot to local/sovereign models if institutional regulations tighten.
- **Inference Optimization**: Use of MoA allows for "High-Quality, Low-Cost" outputs by aggregating smaller models (SLMs) to mimic GPT-4 level reasoning.

## 3. Unit Economics (Estimates)
- **Inference Cost**: High for video-to-text, low for chat (using MoA/SLMs).
- **Gross Margin**: Estimated at 50-60% (standard for AI-native vertical SaaS).
- **Scalability**: RAG indexing scales linearly with content, requiring robust vector DB management (Pinecone/Supabase).
