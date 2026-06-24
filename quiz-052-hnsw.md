# Quiz — Session 052: HNSW: The Algorithm Powering Vector Search

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 4/5) and update PROGRESS.md.

---

## Question 1 — Concept Check

In plain English, explain how HNSW works. Why does it borrow the idea of a "small world" from social networks?

**What to look for:** Student should explain: HNSW builds a layered graph where vectors connect to their nearest neighbours. The top layer has very few nodes with long-range connections spanning the whole vector space; the bottom layer has all nodes with short, precise local connections. Searching works top-down like zooming into a map — high layers navigate fast across the space, lower layers narrow to precise local results. The "small world" analogy: just as you can reach anyone on Earth through ~6 degrees of separation because networks have both local circles AND long-range connections (e.g. someone you know in a different city), HNSW intentionally builds this structure to skip quickly across vector space then zoom in precisely.

---

## Question 2 — Application

Your engineer says "index build is taking 6 hours and we need to speed it up." They're considering changing ef_construction. What are the implications and what would you suggest they consider?

**What to look for:** ef_construction controls search breadth during index building — how many candidates are examined when placing each new vector. Higher ef_construction = better index quality (neighbours are more accurate) but slower build time. The suggestion to reduce ef_construction will speed up the build at the cost of slightly lower recall quality in the final index. Student should recommend: (1) First check whether the quality loss is acceptable — test recall before reducing; (2) Consider whether the 6-hour build is a one-time setup or frequent re-indexing; (3) ef_construction is "baked in" at build time and can't be changed at query time (unlike ef_search). Default is 100; reducing to 64 would speed up the build.

---

## Question 3 — Scenario

Your CPO asks why your vector database is using "only 99% accuracy" and not 100%. She's concerned the product is cutting corners. What do you tell her?

**What to look for:** Student should explain: 100% accuracy requires brute-force comparison of every vector — at 500,000+ documents this would take seconds per query and would be unusable in real-time. HNSW with default settings (~M=16, ef_search=100) achieves ~99% recall at 2–5ms per query. The 1% "missed" matches mean that occasionally a vector ranked #6 is returned instead of the true #5 — in practice, both documents are highly relevant, and users never notice. The session frames this as "99% recall at 3ms is ideal" for production RAG. The only cases where 99.9% is needed are medical/legal decision systems — which is a deliberate product choice, not a corner being cut.

---

## Question 4 — Concept Check

What is the M parameter in HNSW, and how does increasing M affect the system's behaviour? What is the practical cost?

**What to look for:** M is the number of connections per node in the HNSW graph. Higher M = more connections per vector = better recall (the model can navigate to more candidates) but costs more memory and makes the index build slower. Typical range is 8–64, default is 16. The trade-off the session outlines: M=32 with ef_construction=300 gives ~99.9% recall at 10–20ms; M=8 with ef_construction=64 gives ~95% recall at <1ms. Unlike ef_search (which can be tuned at query time), M is fixed at index build time — so it requires rebuilding the index to change.

---

## Question 5 — Application

At what point in a product's lifecycle should you consider reconfiguring HNSW parameters (M, ef_construction, ef_search), and what signals would tell you it's time?

**What to look for:** Student should identify: (1) ef_search can be tuned at query time — this is the first lever to try if latency or recall issues emerge in production without rebuilding the index; (2) M and ef_construction require an index rebuild — revisit when the knowledge base grows significantly (e.g., 10× more vectors), when recall quality degrades measurably, or when you're switching from a latency-first to quality-first use case; (3) Signals to watch: user feedback on irrelevant results (may indicate low recall), query latency exceeding SLA (may indicate ef_search is too high), memory usage growth (may indicate M is too high for available resources).

