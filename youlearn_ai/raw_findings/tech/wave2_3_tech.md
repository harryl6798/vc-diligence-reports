# Findings: YouLearn.ai Wave 2 & 3 - Alpha & Architecture

## Founder Alpha Signals
- **Achyut Krishna Byanjankar**: Exceptional rapid development skills. Built 6 AI apps in 6 weeks. "Apollo" project featured by LangChain and Vercel. Strong "Bridge" talent between RAG orchestration and deployment.
- **Soami Kapadia**: Deep expertise in high-performance inference. Built `groq-moa` (940 stars) during Groq internship. Expertise in "Mixture-of-Agents" architectures and real-time voice AI.
- **David Yu**: High-leverage distribution. 200M+ views as a content creator in the education niche. Strong "Economic Buyer" understanding.

## Technical Architecture (The Moat)
- **Engine**: Source-Grounded Retrieval-Augmented Generation (RAG).
- **Core Stack**:
    - **Frontend**: Next.js / Vercel (Edge runtime for low latency).
    - **Backend**: Python (FastAPI) + Celery (Async processing for large PDF/Video ingestion).
    - **AI Orchestration**: LlamaIndex / LangChain.
    - **Transcription**: Whisper / Deepgram.
    - **Vector DB**: Pinecone / Supabase (pgvector).
- **Differentiation**: Unlike general LLMs, YouLearn enforces **verifiable chains**. Responses contain embedded citations linking back to specific coordinates in PDFs or timestamps in videos. This creates a "Trust-but-Verify" UI that is essential for high-stakes test prep.

## Technical Risks
- **Provider Concentration**: High reliance on OpenAI/Anthropic/Vercel APIs.
- **Compute Overhead**: Video-to-text and vector indexing for 2.2M materials implies significant ingestion costs.
- **Hallucination Floor**: While RAG reduces errors, semantic retrieval can still pull irrelevant context if indexing chunks are too small or too large.
