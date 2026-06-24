# Session 70 — LangChain & LlamaIndex: The Agent Frameworks
**Act:** 5 — AI That Acts | **Date Completed:** 2026-05-24
**Previous:** Session 69 — MCP | **Next:** Session 71 — CrewAI, AutoGen, AgentOS

---

## The Key Idea

LangChain and LlamaIndex are the two most widely-used frameworks for building AI applications with LLMs. They provide pre-built components — chains, agents, memory, retrievers, document loaders — so you don't have to write everything from scratch. Think of them like Django or Rails for web development: you could build a web server from raw sockets, but frameworks let you focus on your app, not the infrastructure.

---

## LangChain — The General-Purpose Agent Framework

LangChain (launched 2022) is the most popular framework for building LLM applications. Its core abstraction is the **chain** — a sequence of operations connected together.

```
┌──────────────────────────────────────────────────────────────────────┐
│              WHAT LANGCHAIN PROVIDES                                  │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  CHAINS:                                                             │
│  Pre-built sequences: input → prompt → LLM → output → next step    │
│  Example: ConversationChain, RetrievalQA, MapReduceChain           │
│                                                                      │
│  AGENTS:                                                             │
│  Agent loop with tool support — the ReAct loop pre-built           │
│  Define tools → pass to agent → agent decides when to call them    │
│                                                                      │
│  MEMORY:                                                             │
│  ConversationBufferMemory, ConversationSummaryMemory,               │
│  VectorStoreMemory — all the patterns from Session 63              │
│                                                                      │
│  DOCUMENT LOADERS:                                                   │
│  Load PDFs, Word docs, web pages, databases, YouTube transcripts   │
│  → 100+ loaders pre-built                                          │
│                                                                      │
│  TEXT SPLITTERS:                                                     │
│  All the chunking strategies from Session 54 — pre-built           │
│                                                                      │
│  RETRIEVERS:                                                         │
│  Hybrid search, reranking, parent-child — pre-built                │
│                                                                      │
│  MODEL INTEGRATIONS:                                                 │
│  Claude, GPT-4, Gemini, Llama — one interface for all              │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## LangChain in Practice — A RAG Pipeline in ~20 Lines

What would take 200+ lines of raw code becomes:

```python
# LangChain makes RAG dramatically simpler

from langchain.document_loaders import PyPDFLoader
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain.embeddings import BedrockEmbeddings
from langchain.vectorstores import Chroma
from langchain.chains import RetrievalQA
from langchain.chat_models import ChatAnthropic

# Load document
loader = PyPDFLoader("echo_india_hr_policy.pdf")
documents = loader.load()

# Chunk it
splitter = RecursiveCharacterTextSplitter(chunk_size=1000, chunk_overlap=200)
chunks = splitter.split_documents(documents)

# Embed and store
vectorstore = Chroma.from_documents(chunks, BedrockEmbeddings())

# Build RAG chain
qa_chain = RetrievalQA.from_chain_type(
    llm=ChatAnthropic(model="claude-sonnet-4-6"),
    retriever=vectorstore.as_retriever(k=5)
)

# Query it
result = qa_chain.run("How many earned leave days do senior engineers get?")
```

The framework handles: loading, chunking, embedding, storage, retrieval, prompt construction, LLM call, and response parsing. You focus on what you're building.

---

## LlamaIndex — The RAG-First Framework

LlamaIndex (originally GPT Index) is purpose-built for retrieval and knowledge-intensive applications. Where LangChain is general-purpose, LlamaIndex is the specialist for indexing and retrieving knowledge.

```
┌──────────────────────────────────────────────────────────────────────┐
│              LANGCHAIN vs LLAMAINDEX — WHEN TO USE EACH              │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  LANGCHAIN                         LLAMAINDEX                        │
│                                                                      │
│  → General-purpose agent apps     → Knowledge-heavy RAG systems     │
│  → Complex multi-step chains      → Multi-document retrieval        │
│  → Tool-using agents              → Knowledge graphs                │
│  → Conversation management        → Structured data querying        │
│  → More integrations overall      → Better retrieval quality        │
│                                                                      │
│  Real world: Many teams use BOTH — LlamaIndex for the retrieval    │
│  layer, LangChain for the agent/chain orchestration layer.         │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## LlamaIndex — Key Concepts

**Index** — LlamaIndex's core abstraction. An index is a structured representation of your documents, optimised for retrieval.

- VectorStoreIndex: semantic search (most common)
- SummaryIndex: hierarchical summarisation for very long documents
- KnowledgeGraphIndex: extracts entities/relationships (Session 58)
- PropertyGraphIndex: structured graph with typed relationships

**Query Engine** — Takes a question, retrieves from the index, generates an answer.

**Chat Engine** — Conversational wrapper around a query engine — maintains conversation history.

```python
# LlamaIndex example

from llama_index.core import VectorStoreIndex, SimpleDirectoryReader
from llama_index.llms.anthropic import Anthropic

# Load all documents in a folder
documents = SimpleDirectoryReader("hr_policies/").load_data()

# Build index
index = VectorStoreIndex.from_documents(documents)

# Create query engine
query_engine = index.as_query_engine(llm=Anthropic(model="claude-sonnet-4-6"))

# Query
response = query_engine.query("What is the maternity leave policy?")
```

---

## LangSmith — Observability for LangChain

LangSmith is LangChain's observability platform (Session 66's tracing tool). It traces every step in a chain or agent run:

- Which model was called, with what prompt, and what it returned
- Which tools were invoked, with what inputs and outputs
- Latency for each step
- Token usage and cost per step

This is the observability layer that makes debugging LangChain applications possible. "Never deploy without observability" — LangSmith is the implementation of that rule for LangChain apps.

---

## When NOT to Use Frameworks

Frameworks add convenience but also add abstraction and complexity. When to skip them:

```
┌──────────────────────────────────────────────────────────────────────┐
│              WHEN TO USE THE API DIRECTLY (NO FRAMEWORK)            │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  → Simple, single-step LLM calls (just call the API directly)     │
│  → Performance-critical paths (frameworks add overhead)            │
│  → Full control over every detail is required                      │
│  → You're building your own orchestration layer                    │
│  → The framework's abstractions fight your requirements            │
│                                                                      │
│  Use a framework when: you need standard patterns (RAG, agents,    │
│  memory) and want to move fast without reinventing plumbing.       │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Key Takeaway

LangChain and LlamaIndex are the dominant frameworks for building LLM applications. LangChain is general-purpose — chains, agents, memory, integrations. LlamaIndex specialises in retrieval and indexing, with better RAG quality for knowledge-heavy applications.

Frameworks accelerate development by providing pre-built components for the common patterns covered in Acts 4 and 5. The trade-off: less control, added abstraction overhead.

For ECHO India: use LlamaIndex for the HR policy RAG layer, LangChain for agent orchestration and tool use.
