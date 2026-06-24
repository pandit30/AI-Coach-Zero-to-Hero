# Quiz — Session 049: Semantic Similarity and Cosine Distance

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 7/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

Using the "arrows pointing in a direction" analogy from the session, explain cosine similarity in plain English — including what scores of 1.0, 0.0, and -1.0 mean.

**What to look for:** Every piece of text is like an arrow pointing in a direction from the origin. "Maternity leave policy" and "parental leave for new mothers" point in almost the same direction — same topic. "Maternity leave policy" and "cricket match schedule" point in completely different directions. Cosine similarity measures the angle between those arrows: 1.0 (0° angle) = identical direction = highly similar meaning; 0.0 (90° angle) = perpendicular = no semantic overlap; -1.0 (180° angle) = opposite direction = opposite meanings (rare in practice). For RAG: small angle = retrieve this document; large angle = skip it.

---

## Question 2 — Type: Concept Check

Why does cosine similarity measure the angle between vectors rather than the straight-line distance — and what problem does this solve?

**What to look for:** Straight-line distance is affected by vector magnitude (length). Text of different lengths produces vectors with different magnitudes — a 10-word question might produce a "shorter" vector than a 500-word policy document even if they're about the same topic. If you measured straight-line distance, a long document would always appear "far" from a short query, regardless of semantic similarity. Cosine similarity only measures direction, ignoring length. Result: it's robust regardless of how long or short the texts are. A one-line question can match a 500-word policy section if they're semantically about the same thing.

---

## Question 3 — Type: Application

Using the cosine similarity ranges from the session, classify these four document pairs for a "What is the maternity leave policy?" query — and decide which to retrieve.

Documents: (A) "Maternity leave: 26 weeks for female employees" — similarity 0.92; (B) "Parental benefits include creche allowance" — 0.78; (C) "Annual performance review process" — 0.31; (D) "Office canteen timings and menu" — 0.12.

**What to look for:** The session's exact example. (A) 0.92 → RETRIEVE (highly relevant); (B) 0.78 → RETRIEVE (related); (C) 0.31 → SKIP (unrelated); (D) 0.12 → SKIP (completely unrelated). The typical retrieval threshold from the session: top-K (e.g., top 5) OR score > 0.7. With a 0.7 threshold: A and B are retrieved, C and D are not. Student should recognise 0.7 as a reasonable boundary between "related" and "not related" based on the session's practical ranges.

---

## Question 4 — Type: Application

An ECHO India employee asks in Hindi: "मेरी मातृत्व छुट्टी कितनी होती है?" (How much maternity leave do I get?) Your knowledge base has English policy documents only. Will a multilingual embedding model find the right document? Explain using the session's example.

**What to look for:** Yes — the session gives exactly this example. With a multilingual embedding model (e.g., multilingual-e5): Hindi question "मेरी मातृत्व छुट्टी कितनी होती है?" and English document "Maternity leave: 26 weeks for female employees" achieve cosine similarity 0.87 → RETRIEVE. This works because a multilingual embedding model places semantically similar content from different languages close in vector space. Practical implications for ECHO India: Hindi questions find English policy documents; one knowledge base serves all languages; no need to translate queries or documents before searching.

---

## Question 5 — Type: Concept Check

What are the three retrieval strategies — Top-K, threshold, and hybrid — and what does the session recommend?

**What to look for:** Top-K: always return the K most similar documents regardless of score. Good when you always want to give the model something to work with — prevents "no results" cases. Threshold: only return documents above a similarity score (e.g., 0.7). May return zero results if nothing is relevant — useful when you'd rather say "I don't know" than retrieve weakly relevant content. Hybrid (recommended): Top-K, but instruct the model "If none of these are clearly relevant to the question, say you don't have information on this topic." Best of both worlds: always has something to consider, but tells the model to decline if none are genuinely relevant.

---

## Question 6 — Type: Scenario

Your ECHO India RAG system is returning wrong answers to specific policy questions — the retrieved chunks look "related" but aren't actually answering the question. Similarity scores are in the 0.65–0.75 range. What might be happening and how do you address it?

**What to look for:** The 0.65–0.75 range is borderline — not clearly irrelevant but not highly relevant either. Possible issues: (1) The chunks retrieved are topically adjacent but don't contain the specific answer — e.g., a general HR benefits page instead of the specific maternity leave policy; (2) The threshold is too low — you're retrieving weakly relevant chunks that distract the model. Approaches to address: (1) Raise the threshold to 0.75–0.80; (2) Review chunking strategy — chunks may be too large and covering multiple topics, diluting the specific content; (3) Add the hybrid instruction — tell the model to say it doesn't know if retrieved docs aren't clearly relevant; (4) Try retrieving more chunks (top 5 instead of top 3) to increase the chance of the right one being included.

---

## Question 7 — Type: Application

Your team is debating whether to use keyword search (existing intranet) or semantic/vector search for ECHO India's HR knowledge base. What's the case for switching to semantic search?

**What to look for:** The session's comparison is the direct answer. Keyword search: "parental leave for new fathers" finds only docs with those exact words; misses "paternity leave policy" and "new parent entitlement." Semantic search: finds all semantically related documents regardless of word choice. For ECHO India specifically: (1) Employees ask in casual language, not policy language; (2) Hindi questions find English documents — cross-language search with multilingual embeddings; (3) Policy uses formal HR terminology; users use informal language; (4) Search is more forgiving of typos and phrasing variations. The session says: "This is why modern enterprise search is so much better than the keyword search in your old intranet."
