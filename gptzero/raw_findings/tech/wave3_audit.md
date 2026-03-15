# GPTZero - Wave 3: Technical Architecture & System Audit

## Detection Pipeline (7-Layer System)
1. **Perplexity Engine**: Measures word-level predictability (AI minimizes surprise).
2. **Burstiness Analysis**: Measures sentence-level variation (Human writing is "bursty").
3. **GPTZeroX**: Sentence-level classification for mixed human-AI text.
4. **Deep Learning Layer**: Transformer-based linguistic fingerprinting.
5. **Education Module**: ESL-tuned model to reduce false positives for non-native speakers.
6. **Internet Search**: Cross-references against archives and common AI training sets.
7. **GPTZero Shield**: Adversarial defense against homoglyph attacks and zero-width spaces.

## Authorship Moat: GPTZero Docs
- **Authorship Verification**: Tracks "Process" over "Product."
- **Telemetry**: Tracks typing cadence, micro-delays (digraphs), and paste events.
- **Revision History**: Uses Google Docs Revision History API to identify specific user contributions.
- **Moat Strength**: High. Telemetry data is much harder to "humanize" than static text.

## Performance Benchmarks
- **Accuracy**: 95-99% for raw AI text.
- **False Positives**: ~0.2% - 2% (Lower than Turnitin for ESL).
- **o1 Performance**: 98%+ accuracy on OpenAI's reasoning models.
