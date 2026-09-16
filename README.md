<p align="center">
  <img src="https://img.shields.io/badge/Distill-Agentic_Document_Intelligence-667eea?style=for-the-badge&logoColor=white" alt="Distill" />
</p>

<h1 align="center">⚗️ Distill</h1>

<p align="center">
  <b>An Agentic Document-Intelligence Platform for Conversational Data Analysis</b><br/>
  <i>Using Retrieval-Augmented Generation, Text-to-SQL, and Dynamic Code Synthesis</i>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/React-19-61DAFB?style=flat-square&logo=react&logoColor=white" />
  <img src="https://img.shields.io/badge/TypeScript-6.0-3178C6?style=flat-square&logo=typescript&logoColor=white" />
  <img src="https://img.shields.io/badge/FastAPI-0.110+-009688?style=flat-square&logo=fastapi&logoColor=white" />
  <img src="https://img.shields.io/badge/LangGraph-Agentic_AI-7C3AED?style=flat-square" />
  <img src="https://img.shields.io/badge/Groq-LLM_Inference-F55036?style=flat-square" />
  <img src="https://img.shields.io/badge/ChromaDB-Vector_Search-10B981?style=flat-square" />
  <img src="https://img.shields.io/badge/SQLite-Database-003B57?style=flat-square&logo=sqlite&logoColor=white" />
  <img src="https://img.shields.io/badge/Plotly-Charts-3F4F75?style=flat-square&logo=plotly&logoColor=white" />
  <img src="https://img.shields.io/badge/Tests-40%2F40_Passing-48BB78?style=flat-square" />
</p>

<p align="center">
  <a href="#-features">Features</a> •
  <a href="#-architecture">Architecture</a> •
  <a href="#-tech-stack">Tech Stack</a> •
  <a href="#-getting-started">Getting Started</a> •
  <a href="#-example-queries">Examples</a> •
  <a href="#-testing">Testing</a> •
  <a href="#-safety--guardrails">Safety</a>
</p>

---

## 🧠 What is Distill?

**Distill** is an autonomous AI agent that lets you upload your files — CSVs, PDFs, Word documents, Excel sheets — and ask questions in plain English. Unlike simple "chat with your PDF" tools, Distill has **three reasoning paths** and intelligently routes every question to the right one:

| You Ask | Distill Uses | Example |
|---------|-------------|---------|
| 📊 A data/numbers question | **Text-to-SQL** | *"What are the top 5 products by profit?"* |
| 📄 A document/policy question | **RAG Pipeline** | *"What is the return policy for furniture?"* |
| 🧮 A computation/chart request | **Code Synthesis** | *"Create a bar chart of sales by category"* |
| 🔗 A complex multi-step question | **Multi-hop Chaining** | *"Get sales by region and visualize it"* |

> **One router. Three reasoning paths. Zero manual switching.**

---

## ✨ Features

<table>
<tr>
<td width="50%">

### 🎯 Core Intelligence
- **LLM-Powered Router** — Classifies questions and picks the right tool automatically
- **Text-to-SQL** — Schema-grounded query generation with exact aggregation
- **RAG (Document Q&A)** — MMR-diversified retrieval with source citations
- **Code Synthesis** — Sandboxed Python execution with Plotly chart generation
- **Multi-hop Reasoning** — Chains SQL → Code, RAG → Code for complex queries

</td>
<td width="50%">

### 🛡️ Trust & Reliability
- **Self-Correction Loops** — Failed SQL/code retried up to 3× with traceback feedback
- **Hallucination Guard** — Refuses to answer when document context is insufficient
- **Confidence Scoring** — Calibrated 0–100 weighted score per response
- **PII Detection & Redaction** — Blocks SSNs, redacts emails and phone numbers
- **Prompt Injection Defense** — Blocks "ignore previous instructions" attacks
- **SQL Injection Guard** — Blocks DROP, DELETE, ALTER operations

</td>
</tr>
<tr>
<td>

### 💻 Full-Stack Experience
- **Modern React UI** — Dark theme, glassmorphism, Framer Motion animations
- **Interactive Plotly Charts** — Zoomable, hoverable, embedded in chat
- **File Upload** — Drag-and-drop CSV, PDF, DOCX, XLSX, TXT
- **Voice Input** — Web Speech API for hands-free queries
- **Session Management** — Multi-session chat history with chronological grouping

</td>
<td>

### ⚙️ Production-Ready
- **Conversation Memory** — Context-aware follow-up questions
- **Dynamic Schema Discovery** — Auto-detects columns, types, sample values
- **PDF Report Export** — Generate formatted PDF reports of analysis
- **Business Insights** — Auto-generated executive insights with action items
- **Deployable** — Render (backend) + Vercel (frontend)

</td>
</tr>
</table>

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                    PRESENTATION TIER                            │
│            React 19 · TypeScript · Vite · Zustand               │
│         Framer Motion · Plotly.js · Tailwind CSS v4             │
└────────────────────────────┬────────────────────────────────────┘
                             │ HTTP / REST
┌────────────────────────────▼────────────────────────────────────┐
│                    APPLICATION TIER                              │
│               FastAPI · Uvicorn · Guardrails                    │
│                                                                  │
│    ┌──────────────────────────────────────────────────────┐     │
│    │              LangGraph Agent (StateGraph)             │     │
│    │                                                      │     │
│    │              ┌──────────────┐                        │     │
│    │              │  ROUTER NODE │                        │     │
│    │              └──────┬───────┘                        │     │
│    │       ┌─────────┬───┴───┬──────────┐                │     │
│    │       ▼         ▼       ▼          ▼                │     │
│    │   ┌───────┐ ┌───────┐ ┌───────┐ ┌────────┐        │     │
│    │   │  SQL  │ │  RAG  │ │ CODE  │ │ DIRECT │        │     │
│    │   │ Node  │ │ Node  │ │ Node  │ │  Node  │        │     │
│    │   └───┬───┘ └───┬───┘ └───┬───┘ └───┬────┘        │     │
│    │       └─────────┴───┬─────┴─────────┘              │     │
│    │                     ▼                                │     │
│    │            ┌──────────────┐                          │     │
│    │            │ SYNTHESIZER  │                          │     │
│    │            └──────────────┘                          │     │
│    └──────────────────────────────────────────────────────┘     │
└────────────────────────────┬────────────────────────────────────┘
                             │
┌────────────────────────────▼────────────────────────────────────┐
│                       DATA TIER                                  │
│        SQLite (WAL mode)          ChromaDB (384-dim vectors)    │
│     Structured tabular data      Document embeddings & search   │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🛠️ Tech Stack

<table>
<tr>
<th>Frontend</th>
<th>Backend & Data</th>
<th>AI & Agent Core</th>
</tr>
<tr>
<td>

| Technology | Purpose |
|-----------|---------|
| React 19 + TypeScript | SPA UI |
| Vite 8 | Build tooling |
| Tailwind CSS v4 | Styling |
| Zustand | State management |
| Framer Motion | Animations |
| Plotly.js | Interactive charts |
| react-dropzone | File upload |

</td>
<td>

| Technology | Purpose |
|-----------|---------|
| FastAPI + Uvicorn | REST API server |
| SQLite + SQLAlchemy | Relational database |
| ChromaDB | Vector storage |
| SentenceTransformers | Text embeddings |
| pdfplumber | PDF parsing |
| pandas | Data processing |
| fpdf2 | PDF report export |

</td>
<td>

| Technology | Purpose |
|-----------|---------|
| LangGraph (StateGraph) | Agent orchestration |
| Groq API | LLM inference |
| LangChain | LLM framework |
| Plotly (Python) | Chart generation |
| PBKDF2-SHA256 | Password hashing |

</td>
</tr>
</table>

---

## 📁 Project Structure

```
distill/
├── agent/                          # 🧠 AI Agent Core
│   ├── graph.py                    # LangGraph StateGraph orchestrator
│   ├── extras.py                   # Guardrails, Confidence, Insights, Export
│   └── tools/
│       ├── sql_tool.py             # Text-to-SQL with self-correction
│       ├── rag_tool.py             # RAG with ChromaDB + hallucination guard
│       └── code_tool.py            # Sandboxed Python execution
│
├── frontend/                       # 💻 React Frontend
│   ├── src/
│   │   ├── App.tsx                 # Root component & routing
│   │   ├── store.ts                # Zustand global state
│   │   ├── api.ts                  # Typed API client
│   │   ├── components/             # UI components (ChatMessage, Sidebar, etc.)
│   │   └── pages/                  # Landing, Login, Register, Chat pages
│   ├── package.json
│   └── vite.config.ts
│
├── utils/                          # 🔧 Data Pipeline
│   ├── generate_data.py            # Synthetic Superstore dataset generator
│   ├── db_loader.py                # CSV/Excel → SQLite loader
│   ├── generate_docs.py            # Sample PDF document generator
│   └── doc_loader.py               # PDF → ChromaDB pipeline
│
├── auth/                           # 🔐 Authentication
│   └── auth_manager.py             # PBKDF2 password hashing + user management
│
├── chat_history/                   # 💬 Session Persistence
│   └── history_manager.py          # SQLite-backed chat history (WAL mode)
│
├── server.py                       # 🚀 FastAPI backend server
├── file_processor.py               # 📂 Multi-format file ingestion hub
├── requirements.txt                # Python dependencies
├── render.yaml                     # Render deployment config
├── vercel.json                     # Vercel deployment config
│
├── test_phase1.py                  # ✅ SQL Tool tests (8/8)
├── test_phase2.py                  # ✅ RAG Tool tests (9/9)
├── test_phase3.py                  # ✅ Code Tool tests (8/8)
├── test_phase4.py                  # ✅ Agent integration tests (8/8)
└── test_phase6.py                  # ✅ Enterprise features tests (7/7)
```

---

## 🚀 Getting Started

### Prerequisites

- **Python** 3.10+
- **Node.js** 18+
- **Groq API Key** — [Get one free](https://console.groq.com/keys)

### 1. Clone & Install

```bash
git clone https://github.com/n-shawarn/Distill.git
cd Distill

# Backend dependencies
pip install -r requirements.txt

# Frontend dependencies
cd frontend && npm install && cd ..
```

### 2. Set Up Environment

```bash
cp .env.example .env
# Edit .env and add your Groq API key:
# GROQ_API_KEY=gsk_your_key_here
```

### 3. Generate Sample Data

```bash
python -c "from utils.generate_data import generate_all; generate_all()"
python -c "from utils.db_loader import load_csvs_to_sqlite; load_csvs_to_sqlite()"
python -c "from utils.generate_docs import generate_all_docs; generate_all_docs()"
```

### 4. Launch

```bash
# Terminal 1 — Backend
python server.py
# → Running on http://localhost:8000

# Terminal 2 — Frontend
cd frontend && npm run dev
# → Running on http://localhost:5173
```

Open **http://localhost:5173** and start chatting! 🎉

---

## 💡 Example Queries

### 📊 SQL (Tabular Analytics)
```
What are the top 5 products by total profit?
Show total sales by region for 2024
Which customers spent more than $5000 in the East region?
What is the monthly sales trend?
```

### 📄 RAG (Document Q&A)
```
What is the return policy for furniture items?
What were the Q1 2024 revenue targets by region?
What is the restocking fee for technology products?
```

### 🧮 Code (Computation & Charts)
```
Calculate compound interest on ₹50,000 at 8.5% for 3 years
Create a bar chart of sales by product category
Show me a pie chart of order distribution by region
Calculate the mean, median, and standard deviation of sales
```

### 🔗 Multi-hop (Chained Tools)
```
Get total sales by category and create a bar chart
Compare profit margins across regions and visualize the results
```

---

## 🧪 Testing

All **40 tests pass** across 5 phases:

| Phase | Component | Tests | Focus |
|-------|-----------|:-----:|-------|
| 1 | SQL Tool | 8/8 ✅ | Aggregation, JOINs, date filters, injection blocking |
| 2 | RAG Tool | 9/9 ✅ | Retrieval, source tracking, hallucination guard |
| 3 | Code Tool | 8/8 ✅ | Computation, Plotly charts, sandbox security |
| 4 | LangGraph Agent | 8/8 ✅ | Routing, multi-hop, conversation memory |
| 6 | Enterprise Features | 7/7 ✅ | PII detection, guardrails, confidence scoring |

```bash
python test_phase1.py   # SQL Tool
python test_phase2.py   # RAG Tool
python test_phase3.py   # Code Tool
python test_phase4.py   # Agent Integration
python test_phase6.py   # Enterprise Features
```

---

## 🛡️ Safety & Guardrails

| Layer | Protection | How It Works |
|-------|-----------|-------------|
| 🔍 **PII Detection** | Emails, phones, SSNs | Regex pattern matching before processing |
| 🚫 **PII Redaction** | Auto-redact sensitive data | `[REDACTED_EMAIL]`, `[REDACTED_SSN]` tokens |
| 🛑 **Prompt Injection** | "Ignore instructions" attacks | Pattern matching + blocking |
| 💉 **SQL Injection** | DROP, DELETE, ALTER | Read-only enforcement (SELECT/WITH only) |
| 🔒 **Code Sandbox** | Dangerous imports blocked | Custom `__import__` hook blocks `os`, `subprocess`, `sys` |
| 📏 **Input Validation** | Query length limits | Max 2000 characters |
| 🤥 **Hallucination Guard** | No context = no answer | Similarity threshold (0.70) + explicit refusal |

---

## 📐 Confidence Scoring

Every response includes a calibrated confidence score:

| Tool Path | Weight | Formula |
|-----------|:------:|---------|
| SQL | 0.40 | `min(95, 70 + rows × 2)` |
| RAG | 0.35 | `min(95, confidence + chunks × 5)` |
| Code | 0.25 | `max(60, 90 − retries × 15)` |

$$\text{Overall Confidence} = \frac{\sum(\text{Score}_i \times \text{Weight}_i)}{\sum \text{Weight}_i}$$

**High** ≥ 75% &nbsp;•&nbsp; **Medium** 45–74% &nbsp;•&nbsp; **Low** < 45%

---

## 🌐 Deployment

| Tier | Platform | Config |
|------|----------|--------|
| Frontend | [Vercel](https://vercel.com) | `vercel.json` — static CDN |
| Backend | [Render](https://render.com) | `render.yaml` — Python web service |

The backend supports `LIGHTWEIGHT_MODE` for memory-constrained environments (Render free tier, 512MB RAM) — uses hash-based embeddings instead of neural models.

---

## 🗺️ Roadmap

- [ ] Multi-hop chained reasoning across SQL, RAG & Code
- [ ] QLoRA fine-tuning benchmark vs. Groq-hosted inference
- [ ] Role-based multi-user shared workspaces
- [ ] Streaming token-by-token response rendering
- [ ] JWT-based authentication
- [ ] PostgreSQL / MySQL support

---

## 👤 Author

**Nandini Shawarn**

Independent Full-Stack & AI Engineering Project — July 2026

---

<p align="center">
  <sub>Built with ❤️ using LangGraph, Groq, React, and ChromaDB</sub>
</p>
