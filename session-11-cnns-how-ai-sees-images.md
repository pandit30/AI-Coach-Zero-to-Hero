# Session 11 — CNNs: How AI Sees Images
**Act:** 1 — The Foundation | **Date Completed:** 2026-05-24
**Previous:** Session 10 — Training Data, Epochs, Batch Size, Learning Rate | **Next:** Session 12 — RNNs & LSTMs

---

## The Key Idea

A regular neural network sees an image as a flat list of numbers — it has no idea that nearby pixels are related. A CNN (Convolutional Neural Network) is designed specifically to understand images by scanning for visual patterns — edges, shapes, textures — in the same way your eye does.

---

## The Problem With Regular Networks and Images

Imagine a 100×100 pixel photo. That's 10,000 pixels. Each pixel has 3 colour values (R, G, B). So the network gets 30,000 numbers as input.

A regular network treats all 30,000 as independent inputs with no relationship to each other. It doesn't know that the pixel in position (50,50) is right next to position (51,50). It doesn't know that nearby pixels tend to form patterns — edges, curves, textures.

CNNs fix this by processing the image the way your eye does — looking at small regions at a time, building up from simple features to complex ones.

---

## How a CNN Works — The Three Steps

### Step 1: Scanning for Patterns (Convolution)

A small filter — think of it as a magnifying glass about 3×3 pixels — slides across the entire image. This filter is looking for one specific pattern: maybe a vertical edge, maybe a diagonal line.

When the filter finds what it's looking for, it produces a high number. When there's no match, it produces a low number. The result is a new, smaller image called a **feature map** — a map of where that pattern appeared.

One CNN layer has many filters — each one trained to detect a different pattern.

```
┌──────────────────────────────────────────────────────────────────────┐
│              HOW A FILTER SCANS AN IMAGE                             │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Original image (simplified):                                        │
│  ┌───┬───┬───┬───┬───┐                                              │
│  │ 0 │ 0 │ 1 │ 1 │ 0 │                                              │
│  │ 0 │ 1 │ 1 │ 0 │ 0 │   ← part of a food photo pixel grid         │
│  │ 1 │ 1 │ 0 │ 0 │ 0 │                                              │
│  └───┴───┴───┴───┴───┘                                              │
│                                                                      │
│  3×3 filter slides across →  produces feature map showing           │
│  WHERE in the image this pattern was found                          │
│                                                                      │
│  Each CNN layer has 32, 64, or 128 different filters                │
│  Each filter learns a different visual feature                      │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

### Step 2: Shrinking the Map (Pooling)

After convolution, the feature map is large. Pooling shrinks it by taking only the strongest signal from each small region — usually the maximum value in a 2×2 area.

This does two things: reduces the size of the data, and makes the detection less sensitive to exact position ("the edge was near here" vs "the edge was at exactly pixel 47,83").

### Step 3: Repeat, Then Classify

Multiple convolution + pooling layers are stacked. Early layers detect simple features (edges, colours, corners). Later layers combine those into complex features (eyes, wheels, letters). The final layers feed into a regular neural network that classifies: "this is a dosa."

---

## The Magic: What Each Layer Sees

This is the real insight. CNNs build up understanding in layers — from simple to complex, just like your visual cortex.

```
┌──────────────────────────────────────────────────────────────────────┐
│          WHAT EACH CNN LAYER DETECTS — FOOD CLASSIFIER              │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Layer 1  →  Edges, lines, colour boundaries                        │
│              (the most basic visual building blocks)                 │
│                                                                      │
│  Layer 2  →  Corners, curves, simple shapes                         │
│              (combining edges into patterns)                         │
│                                                                      │
│  Layer 3  →  Textures — grainy rice, smooth sauce, crispy edges     │
│              (combining shapes into surface patterns)                │
│                                                                      │
│  Layer 4  →  Parts — the circular shape of a roti, the golden-brown │
│              colour of a dosa, the glossy surface of biryani rice   │
│              (combining textures into recognisable components)       │
│                                                                      │
│  Layer 5  →  "This is biryani"                                      │
│              (combining parts into the final classification)         │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

Nobody programmed Layer 3 to detect "crispy edges." The CNN learned it — because recognising crispy edges turned out to be useful for telling dosas apart from rotis.

---

## The Full CNN Flow

```
┌──────────────────────────────────────────────────────────────────────┐
│                  CNN END-TO-END — ZOMATO FOOD CLASSIFIER             │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Photo of biryani                                                    │
│       │                                                              │
│       ▼                                                              │
│  [Conv Layer 1: 32 filters] → detects edges and colours             │
│       │                                                              │
│       ▼                                                              │
│  [Pooling] → shrinks feature maps 2×                                │
│       │                                                              │
│       ▼                                                              │
│  [Conv Layer 2: 64 filters] → detects textures and shapes           │
│       │                                                              │
│       ▼                                                              │
│  [Pooling] → shrinks again                                          │
│       │                                                              │
│       ▼                                                              │
│  [Conv Layer 3: 128 filters] → detects food parts and surfaces      │
│       │                                                              │
│       ▼                                                              │
│  [Flatten → Dense layers] → "biryani: 94%  dosa: 3%  pizza: 3%"    │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Where CNNs Are Used Today

| Use Case | What the CNN is detecting |
| --- | --- |
| Zomato / Swiggy food photo verification | Is this actually food? What dish is this? |
| CCTV security | People, vehicles, suspicious behaviour |
| Medical imaging | Tumours, fractures, anomalies in X-rays and MRIs |
| Self-driving cars | Pedestrians, lane markings, traffic signs |
| Face ID on your phone | Your face — 30,000 infrared dots mapped |
| OCR (reading text in images) | Letters, digits, characters |

---

## Key Takeaway

CNNs see images the way you do — building understanding from simple to complex:

1. **Filters scan** the image for patterns (edges, textures, shapes)
2. **Pooling shrinks** the result while keeping the important signals
3. **Layers stack** — early layers see simple patterns, deep layers see complex objects
4. **Final layers classify** — "this is a dosa, 91% confident"

The breakthrough: CNNs learn what to look for. Nobody hardcodes "look for circular shapes with golden-brown colour." The network figures that out by training on thousands of labelled food photos.

This architecture dominated computer vision from 2012 (AlexNet) until vision transformers started taking over around 2020.
