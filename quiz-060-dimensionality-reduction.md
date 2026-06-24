# Quiz — Session 060: Dimensionality Reduction: PCA, t-SNE, UMAP — Visualising Embeddings

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 4/5) and update PROGRESS.md.

---

## Question 1 — Concept Check

Why can't you directly visualise the quality of your RAG knowledge base just by looking at the data? What do dimensionality reduction algorithms enable?

**What to look for:** Embeddings live in 1,536-dimensional space — impossible for humans to visualise or reason about directly. You can't see whether "payroll" and "salary" chunks are clustering together (which would cause retrieval confusion) or whether Hindi and English documents about the same topic are landing in the same neighbourhood (which determines if cross-language retrieval will work). Dimensionality reduction algorithms (PCA, t-SNE, UMAP) compress these 1,536-dimensional vectors down to 2D or 3D for visualisation. This lets you see the structure of the knowledge base as a scatter plot: which document clusters are well-separated, where there are gaps in coverage, which documents are in unexpected neighbourhoods.

---

## Question 2 — Application

Your RAG system is producing poor results and your engineer runs a UMAP visualisation. Describe three specific things you could learn from the visual that would help diagnose the problem.

**What to look for:** Student should describe specific diagnostic insights from the session: (1) Mixed clusters — if documents about "payroll" and "leave policies" are overlapping in the UMAP plot, both will be retrieved for either query, creating noise. Fix: chunking improvements or a different embedding model; (2) Wrong neighbourhood — if a leave policy chunk appears in the payroll cluster, it's been "misembedded" and won't be retrieved for leave queries. Investigate: was it chunked with payroll sections? (3) No differentiation — one large blob with no distinct clusters means the embedding model can't distinguish between topics in this domain — likely need a domain-specific embedding model; (4) Hindi/English separation — if Hindi and English documents about the same topics cluster separately, cross-language retrieval will fail. Fix: switch to a multilingual embedding model.

---

## Question 3 — Scenario

Your data engineer proposes using t-SNE to compare your current embedding model against a new multilingual model you're evaluating. You need to run this comparison twice a day as the knowledge base updates. What's the problem with this approach and what would you recommend instead?

**What to look for:** The session identifies two key limitations of t-SNE: (1) Slow on large datasets — unsuitable for frequent daily runs on a growing knowledge base; (2) Non-deterministic — different runs give slightly different layouts, making comparison between model A and model B unreliable (the clusters may shift even if the models are identical). For this use case, recommend UMAP instead: faster, mostly deterministic (stable across runs, allowing reliable comparison), and preserves both local cluster coherence and global relative positions better than t-SNE. The session explicitly recommends UMAP as "the modern standard" and "generally preferred over t-SNE for production analysis." PCA could be used for very quick initial exploration but loses non-linear cluster structure.

---

## Question 4 — Application

As PM, you're evaluating whether to switch from your current English-only embedding model to a new multilingual model for the ECHO India HR chatbot. You can't run a full production test yet. How could embedding visualisation help you make this decision?

**What to look for:** The session provides this exact use case: "Can you compare the UMAP plot for our current embeddings vs the new multilingual model? Are the Hindi and English documents about the same topics clustering together now?" Practical process: (1) Run UMAP on both models' embeddings of the same knowledge base; (2) For the English-only model: check whether Hindi query embeddings land near English document embeddings on the same topic — if they're in separate regions, cross-language retrieval will fail; (3) For the multilingual model: check whether same-topic documents in Hindi and English are now co-located in the same cluster; (4) Check whether existing topic clusters (leave policies, payroll, performance reviews) remain well-separated in the new model — a multilingual model shouldn't degrade English-to-English retrieval. This gives strong signal before committing to a production switch.

---

## Question 5 — Scenario

A new PM joins the team and argues "This visualisation stuff sounds nice but it's not a PM's job — that's for data scientists. Why should I care?" How do you convince them?

**What to look for:** Student should make the PM-specific case using the session's framing: (1) When RAG quality is poor, someone needs to ask the right diagnostic questions — "can you visualise the embedding space and show me whether the relevant documents are clustering near the query vectors?" — that person is the PM, directing the team's investigation; (2) When evaluating a new embedding model or knowledge base expansion, the PM needs to interpret the visual to make the build vs. buy decision — you can't interpret what you've never seen; (3) When expanding the knowledge base ("where does this new set of recruitment documents land in the embedding space?"), the PM should be able to read the visual and decide if the expansion is working correctly. The session's explicit point: "You don't need to implement this yourself — ask your engineer to run a UMAP plot when RAG quality is unclear. Reading the visual helps you make better product decisions."

