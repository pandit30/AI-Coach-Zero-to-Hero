# Session 54 — Chunking Strategies: Why How You Cut Documents Changes Everything
**Act:** 4 — Memory & Retrieval | **Date Completed:** 2026-05-24
**Previous:** Session 53 — Basic RAG Pipeline | **Next:** Session 55 — Hybrid Search

---

## The Key Idea

Chunking is splitting your documents into pieces before embedding. It sounds simple but it's one of the most important decisions in a RAG system. Chunks too large → embedding becomes unfocused, retrieval becomes imprecise. Chunks too small → you lose context, retrieved pieces don't make sense on their own. The right chunking strategy can double RAG accuracy.

---

## Why Chunking Matters So Much

An embedding model converts a chunk of text into a single vector. That vector must represent the entire chunk's meaning.

If the chunk is a full 50-page document: the vector averages everything — "leave policies, expense policies, performance review processes, code of conduct..." — and represents nothing specifically well. When you search for "leave policy," this vector matches moderately, but so does every other large document.

If the chunk is one precise paragraph about earned leave: the vector represents exactly that topic. When you search for "leave policy," this chunk ranks very high and is retrieved correctly.

```
┌──────────────────────────────────────────────────────────────────────┐
│              CHUNK SIZE — THE PRECISION TRADE-OFF                    │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  TOO LARGE (entire document):                                        │
│  Vector: vague, captures everything and nothing specifically         │
│  Retrieval: imprecise — everything matches moderately               │
│  LLM context: too much irrelevant information mixed in             │
│                                                                      │
│  TOO SMALL (single sentence):                                        │
│  Vector: precise but lacks context                                  │
│  Retrieval: finds the right sentence but not enough context        │
│  LLM context: fragments without enough info to answer              │
│                                                                      │
│  JUST RIGHT (~500-1,000 tokens / 300-700 words):                   │
│  Vector: specific to one focused topic or section                  │
│  Retrieval: finds precisely relevant chunks                        │
│  LLM context: enough info to answer, not cluttered with irrelevant │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Chunking Strategies — Four Approaches

**Strategy 1: Fixed Size Chunking (simplest)**

Split every 500 tokens, overlap by 50 tokens.

Overlap prevents losing context at chunk boundaries — the same information appears in both the end of chunk N and the start of chunk N+1.

```
Chunk 1: tokens 1-500
Chunk 2: tokens 450-950   ← 50-token overlap with chunk 1
Chunk 3: tokens 900-1400  ← 50-token overlap with chunk 2
```

**When to use:** Unstructured documents, logs, raw text. Fast and simple. Not ideal for documents with natural structure (sections, paragraphs).

**Strategy 2: Semantic Chunking (smarter)**

Split at natural semantic boundaries — paragraphs, sections, topic shifts. Use an LLM or embedding similarity to detect when the topic changes and split there.

**When to use:** Documents with clear structure (policies, articles, product docs). More accurate but slower to implement.

**Strategy 3: Document Structure Chunking (for PDFs, Word docs)**

Use the document's own structure: headings become chunk boundaries. Each section/sub-section = one chunk. Preserve hierarchy in metadata.

```
Chunk: "Section 4.2 — Earned Leave"
Metadata: {section: "4.2", parent_section: "4 — Leave Policies",
           doc: "HR_Policy.pdf", page_start: 12}
```

**When to use:** Well-structured documents (policies, handbooks, manuals). Best when headings are meaningful and consistent.

**Strategy 4: Sliding Window (for high recall)**

Every 100 tokens, create a new chunk of 300 tokens. Massive overlap. Creates many more chunks but ensures no information is missed.

**When to use:** When recall is critical and you can afford more storage and compute. Medical/legal documents where missing any detail is costly.

---

## The Parent-Child Chunking Pattern

One advanced but powerful approach: create two levels of chunks.

**Small chunks (child):** 100-200 tokens, for precise retrieval. The vector search finds these.

**Large chunks (parent):** 500-1,000 tokens, for LLM context. When a child chunk is retrieved, you actually send its parent (the larger surrounding context) to the LLM.

```
┌──────────────────────────────────────────────────────────────────────┐
│              PARENT-CHILD CHUNKING                                   │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Search with small chunks (precise matching):                       │
│  Child chunk: "Earned leave: 21 days per year, max 45 days          │
│  accumulation."                                                      │
│  → Small, specific, matches leave questions precisely               │
│                                                                      │
│  But answer with parent chunk (more context):                       │
│  Parent: "Section 4: Leave Policies [all leave types, encashment,   │
│  application process, approval workflow, exceptions...]"             │
│  → LLM gets full context to give a complete answer                 │
│                                                                      │
│  This combines: retrieval precision + generation context            │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Chunk Size Guidelines by Document Type

| Document Type | Recommended Strategy | Chunk Size |
| --- | --- | --- |
| HR policy manual | Document structure (by section) | Full section (~500-800 tokens) |
| Product FAQ | Fixed size with overlap | 300-500 tokens |
| Support ticket history | Per ticket | 100-300 tokens per ticket |
| Legal contracts | Sliding window | 500 tokens, 100-token stride |
| News/blog articles | Semantic paragraph breaks | 300-600 tokens |
| Code documentation | Per function/class | Variable |

---

## Key Takeaway

Chunking is splitting documents into pieces before embedding — and it's one of the highest-impact decisions in a RAG system.

Rules of thumb:
- ~500-1,000 tokens per chunk for most use cases
- Always use overlap (50-100 tokens) with fixed-size chunking to avoid losing context at boundaries
- Use document structure (headings) when documents are well-structured
- Parent-child chunking: retrieve with small chunks, respond with larger context

Bad chunking is one of the most common causes of poor RAG performance. Before blaming the LLM or the embedding model, check your chunks first.
