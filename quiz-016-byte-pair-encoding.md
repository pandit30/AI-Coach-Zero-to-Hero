# Quiz — Session 016: Byte Pair Encoding (BPE)

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/5) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

Explain the BPE algorithm as if to a non-technical product manager. What problem does it solve, and what is the merging process — walk through the "low lower lowest" example from the session.

**What to look for:** Problem: you need a fixed vocabulary that can represent any text without being too large (word-level) or requiring too-long sequences (character-level). BPE starts from individual characters and repeatedly merges the most common adjacent pair until the vocabulary reaches its target size. The "low lower lowest" example: Start: [l][o][w][ ][l][o][w][e][r][ ][l][o][w][e][s][t]. Step 1: "l"+"o" appears 3 times → merge into "lo". Step 2: "lo"+"w" appears 3 times → merge into "low". Step 3: "low"+"e" appears 2 times → merge into "lowe". Continue until vocabulary size (e.g. 50,000) is reached. Result: common words like "the", "low", "is" get their own token; rare words get broken into common subword pieces. Strong answers explain why this is better than word-level: any new word — a brand name, slang, technical term — can still be tokenized by breaking it into known subword pieces.

---

## Question 2 — Type: Application

The session says BPE handles "unknown words" better than pure word-level tokenization. Give a concrete example relevant to a product context — a new product name, technical term, or compound — and explain the three properties of subword tokenization that make it robust.

**What to look for:** Unknown word example: "GPT-4o" wasn't in training data. It tokenizes as "G"+"PT"+"-"+"4"+"o" — still representable from known subword pieces. Three properties from the session: (1) Unknown words: any new word can be broken into known subword pieces — no "out of vocabulary" failure. (2) Related words share subwords: "run", "running", "runner", "runs" all contain "run". Because they share tokens, the model learns they're related. (3) Morphology for free: suffixes like "-ing", "-tion", "-er" become their own common tokens — the model learns what these suffixes mean across thousands of words without needing explicit grammar rules. Strong answers note the PM relevance: your product's technical terminology (e.g. "payslip", "payroll", "appraisal") may or may not have its own tokens depending on frequency in training data — rare domain terms will be split into smaller pieces, potentially with less accurate semantic representations.

---

## Question 3 — Type: Scenario

Your team is choosing between using Claude (SentencePiece, ~32K vocabulary) and Gemini (SentencePiece, ~256K vocabulary) for a product serving Hindi, Tamil, and Telugu-speaking users in India. What does the vocabulary size difference mean for your use case, and which model do you lean toward for this reason?

**What to look for:** Vocabulary size directly affects multilingual efficiency. Larger vocabulary = more common subwords from more languages can have their own dedicated token, rather than being split into smaller pieces. Claude's ~32K vocabulary: Devanagari, Tamil, Telugu words are more likely to be broken into smaller subword pieces — less efficient tokenization, higher token count per Indian language sentence, higher cost, potentially lower quality. Gemini's ~256K vocabulary: more space for Indian language subwords to have dedicated tokens — more efficient for Hindi/Tamil/Telugu. From the session's table: Gemini has the largest vocabulary at ~256K. The session explicitly states: "Larger vocabularies are generally better for multilingual support — more languages can have their common subwords represented." For an Indian-language-heavy product, lean toward Gemini or models specifically trained on more Indian language data. Strong answers note cost implication: higher token count per Indian language message also means higher API cost — vocabulary size directly affects unit economics.

---

## Question 4 — Type: Concept Check

The session says BPE vocabulary is built from the training corpus. What does this mean for a product building an AI feature in a specialised domain — say, legal contracts or medical records — and what would "a model trained with more of your domain's text in BPE training" actually provide?

**What to look for:** BPE vocabulary reflects the frequency distribution of the corpus it was built from. If the corpus is general internet text, specialised domain vocabulary (legal Latin phrases, medical terminology, Indian legal citations, SEBI regulations) will be underrepresented in the BPE vocabulary — those terms get broken into smaller, less meaningful subwords. What a domain-trained model provides: (1) More domain-specific terms as single tokens → the model sees "caveat emptor" or "myocardial infarction" as a meaningful unit rather than broken pieces; (2) More efficient tokenization of domain text → lower token count → lower cost; (3) Better semantic representations because the model has seen those terms in context more often during training. PM implication: a general model fine-tuned on your domain data helps, but if the BPE vocabulary doesn't include your key terms as tokens, there's a ceiling on how well the model can work with them semantically. Strong answers note: this is an argument for specialised domain models for high-stakes fields like legal or medical.

---

## Question 5 — Type: Application

An engineer says "the new model uses Tiktoken with a 128K vocabulary instead of our current model's 32K SentencePiece." From a PM perspective, what are the practical implications of this change — specifically for cost, quality, and any risks to watch for?

**What to look for:** From the session's table: LLaMA 3 uses Tiktoken with 128K vocab vs. LLaMA 2 / Claude using SentencePiece with 32K. Practical implications: (1) Cost: likely lower token count per API call — larger vocabulary means more text can be represented with fewer tokens (especially multilingual content). Monitor cost per interaction before and after switching. (2) Quality: likely improved for any content that was previously split into many small subword pieces — technical terms, multilingual content, code identifiers. (3) Risks: (a) Token count changes mean any existing cost calculations are invalid — need to recalibrate. (b) Context window utilization changes — the same content may now consume fewer tokens, giving effectively more context capacity. (c) System prompts and examples written for the old tokenizer may behave differently. (d) Any hardcoded token counts in the codebase (e.g. "if message > 500 tokens, truncate") need to be re-validated. Strong answers lead with: always benchmark on your actual use case before and after a tokenizer change.
