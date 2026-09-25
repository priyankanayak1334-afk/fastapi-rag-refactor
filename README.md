### Enterprise AI Assistant Architecture & Refactor

A modular, production-grade RAG (Retrieval-Augmented Generation) backend built with **FastAPI** and **Pydantic**. This repository demonstrates a complete architectural refactor focused on eliminating hardcoded constants, establishing strict runtime type safety, handling edge-case validation, and implementing automated testing metrics. 

### 🏛️ System Architecture Layout

The application processes natural language requests sequentially through decoupled boundary layers: 

[ Client Request ]
       │
       ▼
┌──────────────────────────────┐
│  FastAPI Input Validator     │ <── Validates schema boundaries & rejects injections
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐       ┌──────────────────────────────┐
│    RAG Retrieval Engine      │ ◄───> │ Central Configuration Layer  │
└──────────────┬───────────────┘       │    (config.py / .env)        │
               │                       └──────────────┬───────────────┘
               ▼                                      │
┌──────────────────────────────┐                      │
│     Prompt Builder Matrix    │ <────────────────────┘ Injects model dimensions,
└──────────────┬───────────────┘                        thresholds & chunk settings
               │
               ▼
┌──────────────────────────────┐
│     LLM Orchestrator Layer   │
└──────────────┬───────────────┘
               │
               ▼
[ Grounded JSON Response ]

### 🔍 Codebase Enhancements

During this design review session, five structural core code improvements were made to transition the system to an enterprise baseline: 

1. **Centralized Environment Bounds:** Transferred distributed embedding limits, model names (gpt-4o), and threshold constants into a single Pydantic configuration settings file.
2. **Deterministic Payload Validation:** Swapped native dictionary parameters for strict Pydantic schemas (QueryRequest), returning structured 422 Unprocessable Entity payloads automatically upon validation failures.
3. **Proactive Exception Boundaries:** Wrapped database query runtime executions inside standard try/except blocks to mask database-level errors with safe 503 Service Unavailable signals.
4. **Complete Type Signatures:** Annotated all backend processing parameters with Python type hints and explicit docstrings.
5. **Automated Verification Harness:** Implemented standalone validation checks utilizing pytest to protect downstream dependencies against system performance regressions.

### ⚙️ Configuration Matrix

The system dynamically relies on the following configurations managed in config.py: 

ParameterDefault ValuePurpose
**LLM_MODEL_NAME**
gpt-4oPrimary text generation module engine
**CHUNK_SIZE**
512Token limit chunking resolution
**SIMILARITY_THRESHOLD**
0.75Minimum vector calculation filter bounds
**MAX_CONTEXT_DOCUMENTS**
3Maximum context slices returned by database lookup

### 🚀 Getting Started

### 1. Prerequisites

Ensure you have Python 3.10+ installed on your computer. 

### 2. Setup the Workspace Environment

Install the core application frameworks and developer automation tools: 

bash

pip install fastapi uvicorn pytest pydantic-settings python-dotenv httpx nest-asyncio

Use code with caution.

### 3. Running Automated Tests

Run your regression validation test suites to ensure 100% processing uniformity: 

bash

pytest test_main.py -v

Use code with caution.

### 4. Running the Development API Server

Launch the live asynchronous server worker pool via your terminal: 

bash

uvicorn main:app --reload

Use code with caution.

Once initialized, access the integrated documentation interface directly at http://localhost:8000/docs.
