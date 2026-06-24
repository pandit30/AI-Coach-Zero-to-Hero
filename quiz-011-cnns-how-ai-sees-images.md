# Quiz — Session 011: CNNs — How AI Sees Images

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/5) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

What problem do CNNs solve that a regular neural network has with images — and why does a 100×100 pixel photo create a specific structural problem for a regular network?

**What to look for:** A 100×100 pixel photo = 10,000 pixels × 3 colour values (RGB) = 30,000 numbers. A regular network treats all 30,000 as independent inputs with no relationship to each other — it has no concept that pixel (50,50) is adjacent to pixel (51,50) or that nearby pixels form patterns (edges, curves, textures). It can't leverage spatial structure. CNNs fix this by processing the image the way your eye does — scanning small regions at a time, building from simple features to complex ones. The session's key phrase: "looking at small regions at a time, building up from simple features to complex ones." Strong answers note the core insight: regular networks lose spatial relationships by treating pixels as a flat list; CNNs preserve and exploit those relationships through convolution.

---

## Question 2 — Type: Application

The session describes the three-step CNN process using a food classifier. Walk through all three steps and explain what the model learns at each stage — be specific to the food classification example.

**What to look for:** Step 1 (Convolution): a small filter (3×3 pixels) slides across the entire image looking for one specific pattern — a vertical edge, a diagonal line. Where it finds its pattern, it produces a high number. One CNN layer has many filters (32, 64, 128) each trained to detect a different visual pattern. Step 2 (Pooling): shrinks the feature map by taking the maximum value from small 2×2 regions — reduces data size and makes detection less sensitive to exact pixel position ("edge near here" not "at pixel 47,83"). Step 3 (Repeat, then Classify): multiple conv + pooling layers stack. From the session's food classifier diagram: Layer 1 → edges/lines/colour boundaries; Layer 2 → corners/curves/shapes; Layer 3 → textures (grainy rice, smooth sauce, crispy dosa edges); Layer 4 → parts (circular roti, golden-brown dosa); Layer 5 → "this is biryani." Final dense layers output: "biryani: 94%, dosa: 3%, pizza: 3%." Strong answers note: nobody programmed Layer 3 to look for "crispy edges" — the CNN learned it because it proved useful for distinguishing dosas from rotis.

---

## Question 3 — Type: Scenario

Zomato's team wants to add a feature that verifies food photos uploaded by restaurant partners — confirming they show actual food and identifying the dish category. What type of model architecture do they need, and what would the training data requirement be?

**What to look for:** They need a CNN — the session's use case table explicitly lists "Zomato/Swiggy food photo verification: Is this actually food? What dish is this?" Training data requirements: labelled photos — millions of food images labelled with dish names (biryani, dosa, pizza, etc.) and possibly a binary label (food / not food). The CNN would learn to detect the visual features that distinguish food categories: texture patterns (crispy, grainy, glossy), colour patterns (golden-brown, white, red), shapes (circular, elongated). Strong answers note the key insight: the CNN figures out what to look for from the labelled examples — nobody needs to write rules like "biryani has long rice grains and colour X." Bonus: mention that CNNs dominated computer vision from 2012 (AlexNet) until ~2020 when vision transformers started taking over.

---

## Question 4 — Type: Concept Check

The session says "nobody programmed Layer 3 to detect crispy edges." Why does this matter — and how does it connect to the core distinction between deep learning and traditional ML from Session 02?

**What to look for:** In traditional ML (Session 02), engineers had to define "features" — a checklist of what to look for (check security thread position, verify colour-shifting ink, examine watermark sharpness). This was the bottleneck: humans are not good at knowing what visual features to specify for complex tasks like facial recognition or food identification. Deep learning removed this bottleneck: the CNN discovers its own features automatically from labelled examples. Layer 3 learning "crispy edges" is the CNN discovering on its own that this feature is useful for telling dosas from rotis — because training showed that dosas with crispy edges consistently got labelled differently from rotis. This generalises to anything: face ID, medical imaging, self-driving — the model invents the relevant visual vocabulary from the data. Strong answers state: this is why deep learning outperforms feature-engineered ML for image tasks.

---

## Question 5 — Type: Application

A product team at your company wants to use AI for medical imaging — detecting anomalies in chest X-rays. Using the CNN framework, what are the three most important questions you'd ask the engineering team before approving the roadmap?

**What to look for:** Strong answers draw from the session's use case table and the CNN mechanics. Key questions: (1) Training data quality and size — how many labelled X-rays do we have, who labelled them (radiologists), and are the labels reliable? CNNs learn from what's in the training data — mislabelled images teach wrong patterns. (2) How will the model handle edge cases / rare conditions not well-represented in training data? CNNs excel at patterns they've seen, struggle with distributions they haven't. (3) What does the output layer look like — is this binary (anomaly / no anomaly) → Sigmoid, or multi-class (tumour type A / B / C / no finding) → Softmax? Additional strong questions: how do we measure false negatives specifically (missing a real anomaly is worse than a false alarm in medical contexts), and how do we monitor for degradation in production when the patient population shifts?
