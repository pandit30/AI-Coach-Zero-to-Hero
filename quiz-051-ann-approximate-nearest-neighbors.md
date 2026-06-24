# Quiz — Session 051: ANN: Fast Search in Billion-Item Spaces

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 4/5) and update PROGRESS.md.

---

## Question 1 — Concept Check

Your engineering team says the new vector database can search 1 million vectors in under 5 milliseconds. A colleague is sceptical: "That can't be accurate — it must be cutting corners." How do you explain what's actually happening, and why the trade-off is acceptable for your HR chatbot?

**What to look for:** Student should explain that ANN (Approximate Nearest Neighbor) indexing is being used — not brute-force comparison of every vector. The key trade-off is speed vs. accuracy: ANN returns approximately 95–99% of the true top results (not exact). For an HR chatbot, the student should note that getting the 6th-best match instead of the true 5th-best is essentially unnoticeable — the accuracy loss is worth the 1,000–5,000x speed improvement. Contrast: brute force on 1 million vectors would take 2–5 seconds per query; ANN does it in 1–5ms.

---

## Question 2 — Scenario

Your RAG system is retrieving marginally relevant results for some queries. Your engineer suggests "lowering the ef value to speed things up." What does this mean, and what concern would you raise?

**What to look for:** Student should recognise that ef (search breadth parameter) controls the accuracy-speed trade-off in ANN search. A lower ef = faster but lower recall. For example, ef=40 gives ~95% accuracy at ~1ms vs ef=100 giving ~99% at ~3ms. The concern to raise: if the system is already returning marginal results, reducing ef will make retrieval accuracy worse — the true best matches will be skipped more often. The fix should address chunking or embedding quality, not sacrifice more recall.

---

## Question 3 — Concept Check

What are the three main families of ANN algorithms, and which is most widely used in modern vector databases? Explain why.

**What to look for:** The three families are: (1) Tree-based (KD-Tree, Annoy) — split vector space into a binary tree, works well at low dimensions but degrades at 1,536+ dimensions; (2) Hashing-based (LSH) — hash similar vectors to same bucket, fast but variable accuracy; (3) Graph-based (HNSW) — build a graph connecting vectors to nearest neighbours, best speed-accuracy balance for high-dimensional data. HNSW is the most widely used — it powers Qdrant, Weaviate, and Chroma. Student should explain that graph-based works best for the high-dimensional vectors (1,536D) produced by modern embedding models.

---

## Question 4 — Application

Your product team is building a legal document search tool where finding the absolute best matching clause in a contract database is critical — a missed match could mean a legal risk. How would you configure ANN search differently from your ECHO India HR chatbot, and why?

**What to look for:** For legal/medical/high-stakes use cases, the student should recommend higher accuracy settings — e.g., ef=200 for ~99.9% accuracy at ~10ms, vs the balanced default of ef=100 at ~99% accuracy and 3ms for the HR chatbot. They should explain the key distinction: for the HR chatbot, whether result #5 or #6 is returned makes little difference to user experience; for legal document retrieval, missing the most relevant clause has material consequences. The session explicitly flags "medical/legal decision systems where the best match MUST be found" as the case for higher accuracy settings.

---

## Question 5 — Scenario

A vendor pitching their vector database claims "sub-millisecond search across 500 million documents." What questions would you ask to understand what's really being claimed?

**What to look for:** Student should ask: (1) Is this ANN or exact nearest-neighbor search? (Almost certainly ANN — exact search at 500M would be impractical.) (2) What is the recall rate at that speed? (What percentage of true top matches are actually returned?) (3) What ef/accuracy settings are being used for this benchmark? (Vendors often cherry-pick the fastest setting, not the balanced one.) (4) What dimensionality are the vectors? (Higher dimensions generally slow things down.) The session teaches PMs to evaluate vendor claims critically — "this is ANN, not exact search — that's fine for most use cases" but the recall figure matters.

