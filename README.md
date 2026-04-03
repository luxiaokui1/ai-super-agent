# AI Super Agent Platform

A full-stack AI application platform built with **Spring AI** and **Vue 3**, featuring an AI-powered relationship counselor and an autonomous super agent capable of planning, reasoning, and executing complex tasks.

![Java 21](https://img.shields.io/badge/Java-21-orange)
![Spring Boot 3](https://img.shields.io/badge/Spring%20Boot-3.4-green)
![Spring AI](https://img.shields.io/badge/Spring%20AI-1.0-blue)
![Vue 3](https://img.shields.io/badge/Vue-3-brightgreen)

## Highlights

- **ReAct Agent** — Autonomous agent that thinks, plans, and acts in a loop until the task is complete (inspired by [OpenManus](https://github.com/manusai/OpenManus))
- **RAG Knowledge Base** — Full ETL pipeline: document loading, chunking, embedding, vector storage, and retrieval-augmented generation
- **Tool Calling** — 6 built-in tools: web search, web scraping, file I/O, terminal execution, resource download, PDF generation
- **MCP Protocol** — Model Context Protocol server/client integration for extensible tool ecosystem
- **Multi-turn Conversation** — Persistent chat memory with configurable window size
- **SSE Streaming** — Real-time Server-Sent Events for responsive AI interactions
- **Structured Output** — Type-safe response parsing (e.g., relationship reports)

## Architecture

```
Client Request
    │
    ▼
┌─────────────────────────────────────────────┐
│  Spring AI Processing Pipeline              │
│  Advisor → ChatClient → Memory/RAG/Tools    │
│                    │                        │
│         ┌─────────┼─────────┐               │
│         ▼         ▼         ▼               │
│    Chat Memory   RAG    Tool Calling        │
│                         / MCP               │
└─────────────────────────────────────────────┘
    │
    ▼
  LLM (Qwen via DashScope)
```

## Tech Stack

### Backend
| Technology | Purpose |
|---|---|
| Java 21 + Spring Boot 3 | Core framework |
| Spring AI 1.0 | AI integration framework |
| LangChain4j | Alternative AI framework |
| DashScope (Qwen) | LLM provider |
| PGVector | Vector database for RAG |
| Ollama | Local model deployment (optional) |
| Kryo | High-performance serialization for chat memory |
| Jsoup | Web scraping |
| iText | PDF generation |
| Knife4j | API documentation |

### Frontend
| Technology | Purpose |
|---|---|
| Vue 3 | UI framework |
| Vite | Build tool |
| Vue Router | Client-side routing |
| Axios + SSE | API communication |

## Features

### 1. AI Love Advisor
A multi-turn conversational AI that provides relationship counseling, powered by:
- Custom system prompt with role-specific behavior
- RAG knowledge base with relationship Q&A documents
- Conversation memory (in-memory or file-based with Kryo serialization)
- Structured output for generating relationship reports

### 2. AI Super Agent (YuManus)
An autonomous ReAct agent that can:
- Break down complex tasks into steps
- Select and invoke tools dynamically
- Search the web and scrape content
- Generate PDF documents
- Download resources
- Execute terminal commands
- Loop until the task is complete (max 20 steps)

## Getting Started

### Prerequisites
- **Docker** (recommended) or **Java 21** + **Maven**
- **Node.js 18+** (for frontend)
- **DashScope API Key** (free tier available)

### 1. Get a Free DashScope API Key

1. Go to [Alibaba Cloud Bailian Console](https://bailian.console.aliyun.com/)
2. Sign up / log in with your account
3. Navigate to **API-KEY Management** and create a new key
4. Copy the key (starts with `sk-`)

> DashScope provides **free credits** for new users — enough to run this project.

### 2. Run Backend with Docker

```bash
# Clone the repository
git clone https://github.com/luxiaokui1/ai-super-agent.git
cd ai-super-agent

# Build the Docker image
docker build -t ai-super-agent .

# Run with your API key as environment variable
docker run -d --name ai-super-agent \
  -p 8124:8124 \
  -e DASHSCOPE_API_KEY=your-api-key-here \
  ai-super-agent
```

The backend will be available at `http://localhost:8124/api`

### 2b. Run Backend without Docker

```bash
# Set your API key
export DASHSCOPE_API_KEY=your-api-key-here

# Build and run
./mvnw clean package -DskipTests
java -jar target/yu-ai-agent-0.0.1-SNAPSHOT.jar
```

### 3. Run Frontend

```bash
cd yu-ai-agent-frontend
npm install
npm run dev
```

Open `http://localhost:5174` in your browser.

### 4. API Documentation

Once the backend is running, visit:
- Swagger UI: `http://localhost:8124/api/swagger-ui.html`
- API Docs: `http://localhost:8124/api/v3/api-docs`

## API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/api/ai/love_app/chat/sync` | Synchronous chat with Love Advisor |
| `GET` | `/api/ai/love_app/chat/sse` | SSE streaming chat with Love Advisor |
| `GET` | `/api/ai/manus/chat` | SSE streaming chat with Super Agent |
| `GET` | `/api/health` | Health check |

## Project Structure

```
├── src/main/java/com/yupi/yuaiagent/
│   ├── agent/            # ReAct Agent (BaseAgent → ToolCallAgent → YuManus)
│   ├── advisor/          # Custom Spring AI Advisors (logging, re-reading)
│   ├── app/              # Love Advisor application logic
│   ├── chatmemory/       # File-based chat memory with Kryo
│   ├── controller/       # REST API controllers
│   ├── rag/              # RAG pipeline (loaders, splitters, retrievers)
│   └── tools/            # Tool implementations (search, scrape, PDF, etc.)
├── src/main/resources/
│   ├── document/         # RAG knowledge base documents
│   └── application.yml   # Configuration
├── yu-ai-agent-frontend/ # Vue 3 frontend
└── Dockerfile
```

## Configuration

Key settings in `application.yml`:

```yaml
spring:
  ai:
    dashscope:
      api-key: ${DASHSCOPE_API_KEY:your-api-key}  # Set via environment variable
      chat:
        options:
          model: qwen-plus  # or qwen-max, qwen-turbo
```

Optional configurations (uncomment in `application.yml` as needed):
- **PGVector** — For persistent vector storage
- **MCP Client** — For Model Context Protocol services
- **Ollama** — For local LLM deployment

