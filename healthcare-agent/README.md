# Public Healthcare Voice Agent

Conversational voice assistant designed for public healthcare appointment management. "Sabela" provides automated scheduling, patient verification, and calendar integration with native bilingual support (Galician and Spanish).

## Key Capabilities

- **Bilingual & Native**: Operates natively in Galician and Spanish with automated language adaptation based on patient preference.
- **Real-Time Appointment Workflows**: Live availability queries (Mon-Fri), booking, status listing, and cancellation via SQLModel.
- **Low-Latency Streaming**: Bidirectional voice streaming via FastRTC for natural conversational turn-taking.
- **Modular AI Stack**:
  - **LLM Orchestration**: LangGraph state graph with Groq (Llama / open-weights) and OpenAI support
  - **Speech-to-Text (STT)**: Groq Whisper, Azure Speech, Moonshine (local fallback)
  - **Text-to-Speech (TTS)**: Azure Speech, RunPod (Orpheus), Kokoro (local fallback)
- **Deployment & Persistence**: SQLModel ORM (SQLite / PostgreSQL), containerized via Docker and managed with `uv`.

## Architecture & Tech Stack

| Layer | Technology |
|---|---|
| **Voice Streaming & Orchestration** | FastRTC, LangGraph, Groq / OpenAI |
| **Speech Processing** | Whisper (STT), Azure Speech / Kokoro (TTS) |
| **Backend & Persistence** | Python 3.11+, FastAPI, SQLModel ORM (SQLite / PostgreSQL) |
| **Tooling & Infrastructure** | Docker, uv package manager |
| **Healthcare Standards** | Adaptable to HL7 / FHIR scheduling endpoints |

## Project Structure

```
sergas-agent/
├── src/realtime_phone_agents/
│   ├── agent/           # LangGraph workflows & FastRTC integration
│   ├── avatars/         # Agent persona & prompt configurations
│   ├── infrastructure/  # DB models & persistence layer
│   └── services/        # Business logic (appointments, patient verification)
├── data/                # SQLite persistence
├── notebooks/           # Workflow validation & testing
├── scripts/             # DB seeding, test utilities
└── pyproject.toml       # uv-managed dependencies
```

## Setup & Execution

```bash
# Install dependencies
uv sync

# Seed database with synthetic test data
uv run scripts/seed_db.py

# Validate agent workflows via notebook
jupyter notebook notebooks/

# Launch voice interface
uv run scripts/run_gradio_application.py
```

## System Requirements

- Python 3.11+
- `uv` package manager
- Groq API credentials (minimum); optional Azure / OpenAI keys
- Docker (optional, for containerized execution)
