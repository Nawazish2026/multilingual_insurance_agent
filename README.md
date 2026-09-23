# AI Engineer Assessment — HealthGuard Insurance AI System

A comprehensive AI-powered insurance solution featuring a knowledge-grounded voice agent, production-ready knowledge base, multilingual voice bots, and real-time call insights with live nudges.

## 🏗️ Architecture

```
┌──────────────────────────────────────────────────────────────────┐
│                    HealthGuard AI System                         │
├──────────────┬──────────────┬──────────────┬────────────────────┤
│   Q1: Voice  │  Q2: Knowledge│ Q3: Multi-   │ Q4: Live Insights │
│   Agent      │  Base (RAG)   │ lingual Bots │ & Nudges          │
│              │               │              │                    │
│ • Vapi.ai    │ • ChromaDB    │ • Philippines│ • Streaming ASR    │
│ • GPT-4o    │ • OpenAI Emb  │   (Taglish)  │ • Signal Extract   │
│ • Deepgram  │ • Hybrid      │ • Indonesia  │ • Nudge Engine     │
│ • ElevenLabs│   Retrieval   │   (Bahasa+   │ • Real-time        │
│              │ • PII Detect  │    Javanese) │   Dashboard        │
│              │ • Citations   │              │ • Latency P50/P95  │
├──────────────┴──────┬───────┴──────────────┴────────────────────┤
│                     │ Q1 uses Q2's KB for grounded answers       │
│                     │ Q4 analyzes recorded calls from Q1/Q3      │
└─────────────────────┴───────────────────────────────────────────┘
<img width="3845" height="1181" alt="image" src="https://github.com/user-attachments/assets/1eb1bea7-7ab2-4fb9-9726-b70bd14e3ad8" />



```

## 📂 Project Structure

```
ai-engineer-assessment/
├── knowledge_base/          # Q2: Production-Ready Knowledge Base
│   ├── schema.py            # KB record & chunk schemas (Pydantic)
│   ├── scraper.py           # Web extraction & document parsing
│   ├── cleaner.py           # Data cleaning, dedup, standardization
│   ├── pii_detector.py      # PII detection & masking
│   ├── chunker.py           # Semantic chunking with overlap
│   ├── indexer.py           # OpenAI embeddings → ChromaDB
│   ├── retriever.py         # Hybrid search (semantic + keyword)
│   ├── test_retrieval.py    # Build pipeline + 7 retrieval tests
│   └── data/
│       ├── raw/             # Source data (health insurance domain)
│       └── processed/       # Cleaned records, chunks, test results
│
├── voice_agent/             # Q1: Knowledge-Grounded Voice Agent
│   ├── agent.py             # Vapi.ai assistant configuration
│   ├── kb_connector.py      # Flask server: webhook + web demo
│   ├── config/
│   │   ├── system_prompt.md # Agent persona & conversation rules
│   │   └── business_rules.json
│   └── transcripts/         # Test call recordings
│
├── multilingual_bots/       # Q3: Native-Language Voice Bots
│   ├── philippines/         # Taglish bot (life insurance)
│   │   └── config.py        # Scripts, terms, ASR/TTS, localization
│   ├── indonesia/           # Bahasa bot (multifinance)
│   │   └── config.py        # Formal/colloquial, Javanese accent
│   └── shared/              # Shared ASR/TTS utilities
│
├── live_insights/           # Q4: Live Insights & Nudges
│   ├── pipeline.py          # Streaming orchestration pipeline
│   ├── transcriber.py       # Real-time ASR (Deepgram)
│   ├── signal_extractor.py  # Intent, compliance, sentiment, etc.
│   ├── nudge_engine.py      # Nudge generation with controls
│   ├── server.py            # Dashboard server
│   └── dashboard/
│       └── index.html       # Real-time web dashboard
│
├── docs/                    # Architecture & documentation
├── .env.example             # Environment variable template
├── requirements.txt         # Python dependencies
└── README.md                # This file
```

## 🚀 Quick Start

### 1. Setup

```bash
# Clone and enter directory
cd ai-engineer-assessment

# Create virtual environment
python3 -m venv venv
source venv/bin/activate  # Linux/Mac
# venv\Scripts\activate   # Windows

# Install dependencies
pip install -r requirements.txt

# Configure API keys
cp .env.example .env
# Edit .env with your API keys
```

### 2. Build Knowledge Base (Q2)

```bash
python -m knowledge_base.test_retrieval
```

This will:
- Load 21 health insurance records
- Clean, deduplicate, and standardize
- Scan for PII (1 record flagged)
- Create 128 semantic chunks
- Index in ChromaDB
- Run 7 retrieval tests

### 3. Start Voice Agent (Q1)

```bash
python voice_agent/kb_connector.py
```

Opens at `http://localhost:8000` with:
- **Web chat demo** — text-based conversation with agent Sarah
- **KB search** — test knowledge base queries directly
- **Vapi webhook** — endpoint for Vapi.ai function calls

### 4. Start Live Insights Dashboard (Q4)

```bash
python live_insights/server.py
```

Opens at `http://localhost:8001` with:
- Real-time transcript feed
- Color-coded nudge cards
- Latency metrics (P50/P95)
- Demo call simulation

## 🔑 API Keys Required

| Service | Purpose | Get Key |
|---------|---------|---------|
| OpenAI | GPT-4o + Embeddings | https://platform.openai.com |
| Deepgram | ASR / Streaming | https://deepgram.com |
| Vapi.ai | Voice Agent (optional) | https://vapi.ai |
| ElevenLabs | TTS (optional) | https://elevenlabs.io |

> **Note:** The system works in development mode without API keys using mock embeddings and responses. For production-quality results, add your API keys to `.env`.

## 📋 Test Results Summary

### Knowledge Base (Q2)
- **21 records** across 9 categories (product, qualification, policy, FAQ, objection, claims, compliance, partnership, pricing)
- **128 chunks** with semantic boundaries and overlap
- **PII detection**: 1 record flagged (email address)
- **Retrieval tests**: 7/7 return results (accuracy improves with real OpenAI embeddings)

### Voice Agent (Q1)
- **4 function tools**: KB search, lead creation, callback scheduling, human transfer
- **Compliance**: Call recording disclosure, no-guarantee statements, cooling-off period
- **Test coverage**: Cooperative, objection, out-of-scope, escalation scenarios

### Multilingual Bots (Q3)
- **Philippines**: Taglish scripts with `po/opo` politeness, Filipino date/currency, code-switching
- **Indonesia**: Formal/colloquial registers, Javanese `monggo/nggih`, Rp formatting
- **4+ localization examples per market** showing adaptation vs. translation

### Live Insights (Q4)
- **6 signal types**: compliance risk, missing disclosure, frustration, cross-sell, buying signal, callback need
- **Nudge controls**: 0.65 confidence threshold, 15s global cooldown, 3/min rate limit, 30s expiry, duplicate suppression
- **Estimated false positive rate**: ~5% compliance, ~15-20% frustration, ~25% cross-sell

## ⚠️ Known Limitations

1. **Mock embeddings**: Without OpenAI API key, retrieval uses hash-based embeddings (not semantic) — results are functional but not optimal
2. **No real telephony**: Vapi.ai integration requires API key for actual phone calls; web demo uses text-based chat
3. **ASR accuracy**: Multilingual bots report expected accuracy; actual testing requires real audio with native speakers
4. **Nudge latency**: Rule-based signal extraction is fast (<5ms); LLM-based extraction would be more accurate but add ~500ms latency
5. **No real-time audio streaming**: Q4 uses simulated streaming (replaying transcript); real Deepgram WebSocket streaming requires API key

## 🔮 Production Improvements

1. **KB**: Move to managed vector DB (Pinecone/Weaviate), add automatic re-indexing pipeline, implement feedback loop for retrieval quality
2. **Voice Agent**: Add conversation state machine, implement A/B testing for scripts, add analytics dashboard
3. **Multilingual**: Fine-tune ASR models on actual Filipino/Indonesian financial speech, hire native speakers for script validation
4. **Live Insights**: Add LLM-based signal extraction for higher accuracy, implement WebSocket streaming for sub-second latency, add supervisor alerting

## 🏛️ Evaluation Criteria Addressed

| Area | Weight | How Addressed |
|------|--------|---------------|
| Business understanding | 15% | Health insurance domain with realistic qualification, objection handling, compliance rules |
| Research/domain | 10% | Industry terminology, regulatory requirements, cultural localization |
| End-to-end completeness | 15% | All 4 questions implemented with working code, Q1↔Q2 connected |
| Output quality | 20% | Grounded answers with citations, controlled nudges, localized scripts |
| Functional implementation | 15% | Working KB pipeline, web demo, dashboard, test results |
| AI-tool usage | 10% | RAG architecture, hybrid search, LLM grounding, signal extraction |
| Feasibility & depth | 10% | Production improvement plan, latency measurement, false-positive analysis |
| Presentation | 5% | Architecture diagrams, structured README, clear test results |
