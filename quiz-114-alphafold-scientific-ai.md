# Quiz — Session 114: AlphaFold & Scientific AI

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/5) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

What is the protein folding problem, and why was it considered unsolvable for 50 years? Use the "Levinthal's Paradox" concept to anchor your answer.

**What to look for:** A protein is a chain of amino acids (there are 20 types) that folds into a precise 3D shape — and the shape determines what the protein does. Misfolded proteins cause diseases like Alzheimer's and Parkinson's. The problem: predicting the 3D shape from the amino acid sequence. Levinthal's Paradox: a 100-amino-acid protein has ~10^47 possible conformations. If sampled one per picosecond, it would take longer than the age of the universe to find the right one — yet real proteins fold in microseconds. The paradox: how does nature solve this instantly? We didn't understand for 50 years. AlphaFold 2 (2020) answered it at 92.4/100 accuracy, matching experimental methods that cost $100K–$1M and months of lab work.

---

## Question 2 — Type: Concept Check

How did AlphaFold 2 use evolutionary information to predict 3D structure? Explain the co-evolution analysis step.

**What to look for:** AlphaFold 2's key insight: proteins evolved from common ancestors, so similar proteins across different organisms share structural constraints. Step 1: Find all known proteins with similar sequences across evolution (Multiple Sequence Alignment). Step 2 — Co-evolution analysis: when two positions in the sequence always change together across evolution (correlated mutations), they are probably physically close in 3D space. If position 47 and position 203 always mutate together, they likely touch each other in the folded structure — this gives spatial constraints. Step 3: The Evoformer (transformer architecture) processes both the sequence and the evolutionary information simultaneously. Students should explain why co-evolution = physical proximity.

---

## Question 3 — Type: Application

The session describes the impact on drug discovery: "AlphaFold + AI-assisted virtual screening → vastly narrowed candidate list." Explain what this changes in the traditional drug discovery timeline.

**What to look for:** Traditional timeline: Identify disease target → determine protein structure (years, $100K–$1M) → screen millions of compounds → find candidates → clinical trials. Total: 10–15 years, $1–2 billion per drug. With AlphaFold: protein structure in minutes for free → AI-assisted virtual screening of billions of compounds → narrowed candidate list → better-targeted trials. The session's concrete examples: Isomorphic Labs using AlphaFold 3 for cancer/cardiovascular targets with first clinical trials in 2024; malaria vaccine development; antibiotic resistance work. The key change: removing the bottleneck of experimental structure determination collapses years into minutes and opens screening to the full chemical space.

---

## Question 4 — Type: Scenario

Your CEO comes back from a conference excited about "scientific AI" and asks whether ECHO India should be investing in AlphaFold-like capabilities. How do you frame the strategic lesson from AlphaFold for an HR tech company?

**What to look for:** AlphaFold is not directly applicable to HR tech, but the session gives three strategic lessons: (1) AI as a creative tool, not just efficiency — just as AlphaFold solves problems humans couldn't, AI can potentially solve talent matching or workforce planning problems that are genuinely hard with traditional tools; (2) Verifiable-answer domains are ripe for AI — AlphaFold works because outcomes are verifiable (you can check the structure experimentally). In HR, performance outcomes are increasingly measurable. The more you instrument and measure outcomes, the more conditions for AI to improve; (3) Data moats matter — DeepMind's advantage was the Protein Data Bank (structured, labelled dataset). ECHO India's equivalent: a large proprietary dataset of hiring decisions correlated with employee performance outcomes. This is a genuine competitive moat.

---

## Question 5 — Type: Concept Check

What is the significance of DeepMind winning the 2024 Nobel Prize in Chemistry for AlphaFold? What does it signal about the scientific community's view of AI?

**What to look for:** Demis Hassabis and John Jumper won the Nobel Prize in Chemistry in 2024 for AlphaFold — the first Nobel Prize awarded for an AI system. This signals that the scientific community officially recognises AI as a tool for genuine discovery, not just automation. The session's framing: "Scientific AI generates value by enabling discoveries that are completely new to humanity" as opposed to most AI products that "generate economic value by making existing human tasks faster or cheaper." The Nobel recognition is significant because Nobel Prizes go to discoveries that fundamentally advance human knowledge — acknowledging AlphaFold places AI in that category.

---
