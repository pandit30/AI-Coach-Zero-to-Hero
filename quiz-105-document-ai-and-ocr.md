# Quiz — Session 105: Document AI & OCR

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/7) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

What is the difference between traditional OCR and modern Document AI? Use the session's "photocopier vs. secretary" analogy to anchor your answer.

**What to look for:** Traditional OCR reads characters and converts to a flat string of text — it has no idea what any of it means (the photocopier). Modern Document AI understands layout: it identifies tables, headers, form labels, key-value pairs, and signature fields, AND reads the text within them. It outputs structured data, not just raw text. Like an experienced secretary, it can tell you that "Net 30" means payment in 30 days, that a clause is non-standard, and that a signature block is missing. Students should contrast flat text output vs. structured data output with meaning.

---

## Question 2 — Type: Scenario

Your team is building a payslip extraction feature. You need to handle payslips from 500+ different Indian companies, each with a slightly different format. Which Document AI platform would you recommend and why?

**What to look for:** Should recommend either Google Document AI (which has a pre-built payslip processor) or Nanonets/Docsumo (India-focused platforms with pre-built templates for Indian payslip formats including GST invoices, Form 16). Azure Form Recognizer (now Azure AI Document Intelligence) could also be valid for its custom model training (upload 5 labeled examples and it trains for your specific format). Should avoid recommending Tesseract (open-source OCR without layout understanding) for this use case. Strong answers will distinguish between pre-trained processors for standard formats vs. custom model training for proprietary formats.

---

## Question 3 — Type: Application

Your product team proposes a feature where the system automatically processes all incoming invoices without any human review. Your head of engineering says "the model is 97% accurate on clean digital invoices." What's missing from this analysis?

**What to look for:** Should raise three issues: (1) The 97% accuracy applies to clean digital invoices — photographed paper invoices are only 88–93%. What's the mix of input quality in production? (2) The confidence score design is missing — not all fields in a single invoice are equally reliable. The session's framework: confidence > 0.95 = auto-accept; 0.80–0.95 = human review; < 0.80 = reject. (3) 97% accuracy at scale still means 3 errors per 100 invoices. At 5,000 invoices/month, that's 150 potential errors. What's the cost of an error? Should recommend a confidence-based routing design rather than fully automated processing.

---

## Question 4 — Type: Concept Check

What is the "straight-through processing rate" and why is it the key KPI for a Document AI product?

**What to look for:** Straight-through processing rate = the percentage of documents handled without any human review (zero human touch). The session's example: 85% of documents auto-processed, 12% with 3–4 flagged fields reviewed, 3% fully manually reviewed. It's the key KPI because it directly measures the ROI of the automation — each document fully processed without human touch saves cost and time. A well-designed Document AI product maximises this rate while keeping error rates acceptable. Students should connect this to the confidence score thresholds.

---

## Question 5 — Type: Scenario

A client tells you their Document AI vendor handles printed English invoices with 97% accuracy but their team manually re-enters about 30% of forms because they contain handwritten Indian regional script fields (Marathi, Tamil). What should you tell them?

**What to look for:** Should reference the session's accuracy table: Indian scripts (handwritten) = 65–80% accuracy. This is why ~30% manual re-entry makes sense — the model's accuracy on handwritten Indian scripts is low enough to require human review. Should recommend: (1) For printed Indian regional scripts, dedicated platforms like Nanonets handle regional language text and accuracy is 90–96%; (2) For handwritten sections, the practical rule from the session: "For any handwritten intake with >5% error rate, design a human review step for the flagged fields." The solution is confidence-score routing for regional script fields, not eliminating human review entirely.

---

## Question 6 — Type: Application

HDFC Bank reduced document processing time from 4 days to 6 hours using Document AI. What architectural choice made that improvement possible — specifically, what does the session describe about how Document AI handles complex loan application documents?

**What to look for:** The session's HDFC example: ~15,000 home loan applications monthly, each with income proof, property documents, bank statements, and KYC. The architectural win is: Document AI extracts structured data from all document types in one pipeline, rather than routing each document type to a different specialist team. The steps from the session: layout analysis → OCR within identified regions → structural reconstruction → optional LLM semantic layer for anomaly detection. The key is that the LLM layer on top can then do three-way matching and flag anomalies — the session gives the Mahindra supply chain example of this.

---

## Question 7 — Type: Scenario

Your VP of Engineering suggests building the payslip extraction feature using just Tesseract (the open-source OCR engine) to save licensing costs. Make the case for why this is the wrong tool and what the right alternative is.

**What to look for:** Tesseract is described in the session as excellent for "straightforward text extraction when you don't need structure" but explicitly cannot understand layout like modern Document AI. For payslips, structure is everything: you need to extract labelled key-value pairs (basic salary, HRA, deductions, net pay) from a multi-column, tabular document. Tesseract would give a flat wall of text with no labels. The right tool is Google Document AI (payslip processor), Nanonets, or Azure Form Recognizer. However, should acknowledge Tesseract's advantage: it runs on your own servers (important for data privacy and cost at scale), making it valid for simple document types but not structured extraction.

---
