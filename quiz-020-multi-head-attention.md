# Quiz — Session 020: Multi-Head Attention

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/5) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

What is the limitation of single-head attention that multi-head attention solves? Use the Priya sentence from the session to show how multiple types of relationships need to be tracked simultaneously.

**What to look for:** Single attention can only focus on one type of relationship at a time — but language has many simultaneous relationships in any sentence. The session's example sentence: "Priya from the Bengaluru ECHO India office, who leads AI products, said the launch will happen next quarter." To understand this fully, you need to track simultaneously: Who (Priya → leads → AI products), Where (Bengaluru, ECHO India office), What (launch will happen), When (next quarter), Reference ("who" refers to "Priya"). One attention head can capture one of these. Multi-head attention runs many heads in parallel — each one learning to notice something different — and combines their outputs. The orchestra analogy: one listener focusing only on melody misses rhythm, harmony, and dynamics. Multi-head attention has 8, 16, or 32 specialist listeners each focused on a different aspect of the same musical performance. Strong answers explain: the fundamental limitation is that attention weights are a single probability distribution — you can't simultaneously attend strongly to multiple different aspects using one distribution.

---

## Question 2 — Type: Application

The session shows a table of real findings from GPT-2 research: some heads track the previous word, some track sentence beginnings, some follow pronoun referents, some track grammatical roles. What does this tell a PM about whether to trust AI model interpretability claims from vendors?

**What to look for:** The research finding matters because: (1) Nobody programmed these head specialisations — they emerged from training. This means the model has developed genuine internal representations of linguistic structure, not just surface statistics. (2) But these are observed patterns, not designed features — a vendor claiming "our model understands grammar" is a post-hoc observation about emergent behaviour, not a guarantee. (3) Interpretability is partial: researchers can see that some heads track pronouns, but can't fully explain why a model gives a specific output for a specific input. Questions to ask vendors: "What evidence do you have that the model handles X reliably?" rather than "Does the model understand X?" — because emergent understanding is probabilistic, not guaranteed. Strong answers note the deeper insight: the fact that heads specialise is evidence that the model is doing genuine linguistic processing — but the specific specialisations vary by model, training data, and architecture, so claims from one model don't transfer to another.

---

## Question 3 — Type: Scenario

Your engineering team is scaling up a model and asks: "Should we double the number of layers or double the number of attention heads?" Based on the session's explanation of what each dimension does, what's your reasoning framework for this trade-off?

**What to look for:** What more layers do: each transformer layer applies self-attention + feedforward network, building progressively richer representations (syntax → semantics → world knowledge, as in Session 19). More layers = more levels of abstraction, better at complex reasoning. What more heads do: each head notices a different type of relationship in the same layer. More heads = more simultaneous aspects of meaning captured per layer. The trade-off: heads get model_dimension / num_heads dimensions each — more heads means each head has a smaller "slice" to specialise in. There's a point of diminishing returns. More layers generally adds more "thinking depth." The framework: if the model is failing on complex, multi-step reasoning → more layers likely helps. If the model is failing on capturing complex linguistic relationships within sentences → more heads may help. In practice: both dimensions are scaled together in larger models (GPT-3: 96 heads, 96 layers; Claude 3: est. 64–96 heads, 80+ layers). Strong answers reference the session's table: larger models use more heads AND more layers — they're not alternatives but complementary dimensions.

---

## Question 4 — Type: Concept Check

After all heads compute their attention, the outputs are concatenated and projected back through a linear layer. Explain why this final projection step is necessary — and what would be lost without it.

**What to look for:** Without the final projection: each head produces an output of size model_dimension / num_heads (a fraction of the full model dimension). Concatenating 8 heads would give 8 × (model_dimension/8) = model_dimension — so the shape is right. But the concatenated output is just a stacking of independently learned subspaces — there's no mechanism for the heads to communicate with each other or for their outputs to be recombined into a coherent joint representation. The linear projection layer applies a learned weight matrix that combines information across all heads — learning which aspects from which heads should be weighted together. Without it, head 3's finding about temporal relationships and head 7's finding about coreference would exist in separate subspaces with no way for the next layer to integrate them. Strong answers note: the projection layer is a learned combination step — it's another set of parameters that the model trains to integrate multi-perspective information optimally for the task.

---

## Question 5 — Type: Application

A product manager colleague says: "Our chatbot uses a 12-head attention model — is that enough, or should we push for a 96-head model?" What questions would you ask to evaluate whether this matters for your specific use case, and what does the session tell you about how head count relates to model quality?

**What to look for:** Questions to ask: (1) What is the model dimension / embedding size? More heads with the same model dimension means each head has fewer dimensions — there's a ceiling where adding heads stops helping. (2) What is the primary failure mode? If the chatbot misidentifies who "they" refers to in complex conversations → coreference tracking (a head-level concern). If it fails at multi-step reasoning → more layers may matter more than heads. (3) What is the model size (parameters)? A 12-head model with 7B parameters is very different from a 12-head model with 70B. Head count alone is not the differentiating variable — total parameters, training data, and layers matter more. From the session: GPT-2 (small) uses 12 heads, GPT-3 uses 96 heads. But the bigger architectural differences are in total parameters and training. Strong answers push back on the framing: head count is a proxy for model architecture, but the real question is whether the model handles your specific use cases well — test it directly rather than reasoning from head count.
