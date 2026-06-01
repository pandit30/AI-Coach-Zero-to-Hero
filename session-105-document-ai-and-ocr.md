# Session 105 — Document AI & OCR: Reading Invoices, Contracts, and Forms
**Act:** 8 — Seeing, Hearing & More: Multimodal AI | **Date Completed:**
**Previous:** Session 104 — How Diffusion Models Work | **Next:** Session 106 — Speech-to-Text: Whisper & Real-Time Transcription

---

## The Key Idea

Every organization drowns in paper — scanned forms, photographed receipts, PDF invoices, printed contracts, handwritten application forms. Traditional OCR could read text off a page. Modern Document AI understands the document: what's a table, what's a header, what's a signature field, and what does this clause actually mean. The jump from "reading text" to "understanding documents" is as big as the jump from typing to thinking — and it's already transforming finance, legal, HR, and logistics operations across India and the world.

---

## The Analogy: From Photocopier to Secretary

Imagine the difference between a photocopier and an experienced secretary. A photocopier reproduces text faithfully — it has no idea what any of it means. An experienced secretary reads a contract, understands its structure, flags the unusual clauses, summarizes the key terms, and tells you what needs your attention.

Traditional OCR is the photocopier. Modern Document AI is the secretary. The secretary doesn't just see the letters — she understands that "Net 30" means payment in 30 days, that the clause on page 4 is non-standard, and that the signature block is missing an authorized signatory.

---

## Traditional OCR vs Modern Document AI

```
┌──────────────────────────────────────────────────────────────────────┐
│              OCR vs DOCUMENT AI — WHAT'S THE DIFFERENCE?            │
├──────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  TRADITIONAL OCR (e.g. Tesseract, ABBYY classic)                   │
│  ──────────────────────────────────────────────                     │
│  → Reads characters and converts to text                            │
│  → Output: a flat string of text                                    │
│  → No understanding of layout, structure, or meaning               │
│  → Terrible with tables, forms, multi-column text                  │
│  → Fails on handwriting, unusual fonts                              │
│  → Accuracy degrades significantly with skew, lighting, quality    │
│                                                                       │
│  MODERN DOCUMENT AI (e.g. AWS Textract, Google Document AI)        │
│  ──────────────────────────────────────────────────────────         │
│  → Reads text AND understands layout (tables, forms, headers)      │
│  → Output: structured data (key-value pairs, table rows, sections) │
│  → Preserves document structure — knows a table cell is a table    │
│  → Handles handwriting, stamps, signatures                         │
│  → Extracts: "Invoice Total: ₹47,500" as a labeled field          │
│  → Can be combined with LLMs for full semantic understanding       │
│                                                                       │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Key Document AI Platforms

**AWS Textract**
Amazon's cloud Document AI service. Excels at extracting structured data from forms and tables — it outputs key-value pairs ("Employee Name: Deepak Sharma") and tables with row/column structure preserved. Integrates naturally with AWS Lambda and S3 for automated pipelines. Pricing: per page processed.

**Google Document AI**
Google's equivalent, with purpose-built "processors" for specific document types: invoice processor, identity document processor, contract processor, payslip processor. You pick the processor for your use case and it comes pre-trained for that document layout. Also offers the Document AI Workbench for fine-tuning on your own document formats.

**Azure Form Recognizer (now Azure AI Document Intelligence)**
Microsoft's platform. Strong on custom model training — upload 5 labeled example documents and it trains a model for your specific form layout. Excellent for proprietary forms with unusual structures. Deep integration with Power Automate for no-code pipelines.

**Tesseract 5.x (open source)**
The free, open-source OCR engine maintained by Google. Cannot understand layout like modern Document AI, but excellent for straightforward text extraction when you don't need structure. Runs on your own servers — important for data privacy. Used extensively in Indian government digital initiatives.

**Nanonets / Docsumo (India-focused startups)**
Rapidly growing Document AI platforms focused on Indian document formats — GST invoices, Aadhaar cards, PAN cards, Form 16, payslips. Offer pre-built templates for Indian compliance documents.

---

## How Document AI Works — The Architecture

```
┌──────────────────────────────────────────────────────────────────────┐
│              HOW MODERN DOCUMENT AI WORKS                            │
├──────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  INPUT: Scanned invoice image (JPEG, PDF)                           │
│                                                                       │
│  STEP 1: LAYOUT ANALYSIS                                             │
│  Model identifies: text blocks, tables, checkboxes, signatures     │
│  Uses a Document Layout Analysis model (trained on millions of     │
│  documents with labeled regions)                                    │
│                                                                       │
│  STEP 2: OCR EXTRACTION                                              │
│  Reads the actual characters within each identified region          │
│  Much more accurate because it knows what TYPE of content to expect │
│                                                                       │
│  STEP 3: STRUCTURAL RECONSTRUCTION                                   │
│  Assembles: table headers + rows, form label + value pairs,        │
│  section headings + paragraphs                                       │
│                                                                       │
│  STEP 4 (Optional): SEMANTIC UNDERSTANDING                          │
│  Send the extracted structured data to an LLM:                     │
│  "Here is the extracted invoice. What is the total, the vendor,    │
│   the PO number, and are there any discrepancies?"                 │
│  → Full document intelligence: structure + meaning                  │
│                                                                       │
│  OUTPUT: JSON with labeled fields, tables, confidence scores        │
│  {"vendor_name": "Infosys Ltd", "total": "₹2,40,000",             │
│   "due_date": "2026-06-30", "confidence": 0.97}                    │
│                                                                       │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Key Document Types and What AI Can Extract

**Invoices:**
Vendor name, date, line items (description, quantity, unit price), subtotal, tax (GST breakdown), total, bank details, PO reference. Accuracy: 95%+ on clean digital invoices, 88–93% on photographed paper invoices.

**Contracts:**
Party names, effective date, termination conditions, payment terms, renewal clauses, governing jurisdiction, signature blocks. LLM layer then flags unusual clauses, summarizes obligations.

**Identity documents (KYC):**
Aadhaar — name, DOB, address, UID number. PAN — name, father's name, DOB, PAN number. Passport — all biographic page fields. Indian-specific platforms like Nanonets handle regional language text (Tamil, Telugu, Marathi) on these documents.

**HR documents — payslips:**
Employer name, employee name/ID, pay period, CTC components (basic, HRA, DA, special allowance), deductions (PF, TDS, professional tax), net pay. ECHO India: this is directly relevant.

**Forms (paper-based):**
Application forms, government forms (Form 16, GST returns, IT forms), insurance claim forms. Key-value extraction: checkbox states (ticked/unticked), handwritten entries, signature presence/absence.

**Bank statements:**
Transaction date, description, debit/credit, running balance. Used in lending platforms (Cashfree, Razorpay, NBFC players) for automated income assessment.

---

## Handling Handwriting — The Hard Problem

Printed text OCR is largely a solved problem. Handwriting is harder.

```
┌──────────────────────────────────────────────────────────────────────┐
│              HANDWRITING OCR — ACCURACY RANGES                       │
├──────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  PRINTED (typed) text, clean scan:     97–99% character accuracy   │
│  PRINTED text, poor quality scan:      88–95%                       │
│  NEAT handwriting, consistent style:   85–92%                       │
│  Average handwriting:                  70–85%                       │
│  Hurried / cursive / poor handwriting: 50–70%                       │
│  Mixed handwriting + printed:          85–92% (varies per region)   │
│  Indian regional scripts (printed):    90–96% (improving rapidly)  │
│  Indian scripts (handwritten):         65–80%                       │
│                                                                       │
│  Practical rule: For any handwritten intake with >5% error rate,   │
│  design a human review step for the flagged fields.                 │
│                                                                       │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Real-World Deployment Examples

**HDFC Bank — Loan processing:**
HDFC processes ~15,000 home loan applications monthly. Each application includes income proof, property documents, bank statements, and KYC. Document AI reduced document processing time from 4 days to 6 hours, and manual data entry headcount was redeployed to exception handling.

**Indian government — DigiLocker + UIDAI:**
DigiLocker uses Document AI to verify and parse documents uploaded by citizens — degrees, marksheets, vehicle registrations, and insurance certificates. Millions of documents processed daily.

**Legal — Khaitan & Co (law firm):**
Contract review automation using Document AI + LLM layer. Associates no longer manually read 200-page contracts looking for non-standard clauses. The system flags deviations from standard templates, letting lawyers focus on the flagged 10% rather than the full document.

**Manufacturing — Mahindra supply chain:**
Goods received notes, delivery challans, and vendor invoices are processed automatically via Textract. Three-way matching (PO + delivery note + invoice) happens in seconds instead of hours.

**Healthcare — Apollo Hospitals:**
Discharge summaries, lab reports, and prescriptions are parsed and entered into electronic health records automatically. Reduces transcription errors and speeds up insurance claims submission.

---

## The Confidence Score: Your Quality Guardrail

Every Document AI extraction comes with a confidence score (0.0 to 1.0) for each field.

```
┌──────────────────────────────────────────────────────────────────────┐
│              USING CONFIDENCE SCORES IN PRODUCT DESIGN              │
├──────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  Score > 0.95: Auto-accept — process without human review           │
│  Score 0.80–0.95: Low-confidence flag — show to reviewer            │
│  Score < 0.80: Reject — ask user to re-upload or enter manually     │
│                                                                       │
│  A well-designed Document AI product uses these thresholds to       │
│  route documents intelligently:                                      │
│  - 85% of documents → fully automated (zero human touch)           │
│  - 12% of documents → human reviews 3–4 flagged fields             │
│  - 3% of documents → full manual review                             │
│                                                                       │
│  This is the "straight-through processing rate" metric — a key     │
│  KPI for any document automation product.                           │
│                                                                       │
└──────────────────────────────────────────────────────────────────────┘
```

---

## What This Means for ECHO India

```
┌──────────────────────────────────────────────────────────────────────┐
│              DOCUMENT AI AT ECHO INDIA                               │
├──────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  PAYSLIP PROCESSING: Candidates submit payslips from previous       │
│  employers (printed, photographed, PDF). Document AI extracts       │
│  CTC, employer, designation, and pay period automatically.          │
│  ROI: ~10 mins manual entry per candidate → 5 seconds + review.    │
│                                                                       │
│  OFFER LETTER PARSING: Clients send offer letters for new hires.   │
│  AI extracts designation, CTC, joining date, grade level —         │
│  populates HRIS without manual entry.                               │
│                                                                       │
│  PF/ESI FORM PROCESSING: Monthly PF returns involve form data      │
│  from employer challan documents. Document AI automates intake.     │
│                                                                       │
│  RECOMMENDED STACK: Google Document AI (payslip + ID processor)    │
│  for standard documents. Azure custom model for proprietary         │
│  client-specific form formats. LLM layer (GPT-4o/Claude) on top    │
│  for contract/offer letter semantic understanding.                  │
│                                                                       │
│  FIRST BUILD: Payslip parser — high frequency, clear ROI,          │
│  measurable accuracy improvement, manageable legal risk.            │
│                                                                       │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Key Takeaway

Modern Document AI is not OCR. It understands document structure — tables, forms, key-value pairs, sections — and extracts structured data, not just raw text. Combined with an LLM layer, it can semantically understand documents: summarize contracts, flag anomalies, and interpret what a clause means.

The key operational metric for any Document AI product is the straight-through processing rate — the percentage of documents handled without human review. Design confidence-score thresholds to maximize that rate while keeping error rates acceptable.

For ECHO India, payslip parsing and identity document extraction are the highest-ROI starting points — both are high-volume, well-understood document types, and both have pre-trained models available today.
