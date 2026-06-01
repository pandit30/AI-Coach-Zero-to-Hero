# Session 115 — AlphaGo & Self-Play: Reinforcement Learning That Beat World Champions
**Act:** 9 — Reasoning Models & The Intelligence Frontier | **Date Completed:**
**Previous:** Session 114 — AlphaFold & Scientific AI | **Next:** Session 116 — World Models

---

## The Key Idea

In March 2016, a computer program beat the world's best player of the ancient board game Go — a feat experts had said was a decade away. What made AlphaGo remarkable wasn't just the win. It was the method: a combination of deep learning, reinforcement learning, and a technique called self-play, where the system learned by playing millions of games against itself. This combination — give the system a clear reward signal, let it practice against itself, and scale the compute — produced superhuman performance. That same core insight now underlies modern reasoning models, RLHF, and the systems being built toward AGI.

---

## Why Go Was Considered Unbeatable for Computers

Chess computers beat Kasparov in 1997. Why did Go take another 19 years?

```
┌──────────────────────────────────────────────────────────────────────┐
│           CHESS vs GO — WHY GO IS HARDER                            │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  CHESS:                                                              │
│  Board: 8×8 = 64 squares                                            │
│  Pieces: 32 total, fixed moves                                       │
│  Game tree: ~10^120 possible games                                   │
│  Strategy: Deep calculation, tactical patterns                      │
│  Brute force (with pruning) works: IBM Deep Blue beat Kasparov     │
│                                                                      │
│  GO:                                                                 │
│  Board: 19×19 = 361 intersections                                   │
│  Pieces: just black and white stones                                 │
│  Game tree: ~10^170 possible games (more than atoms in universe)   │
│  Strategy: Highly intuitive, positional, aesthetic                 │
│  Brute force is impossible — even with all computing power on Earth │
│                                                                      │
│  Go requires something like intuition. You can't calculate          │
│  your way through it. You have to "feel" the board.                │
│  This is why Go masters said a computer would never beat them.     │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## AlphaGo: The Architecture

AlphaGo combined three components that had never been combined this way before:

### Component 1: The Policy Network

A convolutional neural network trained to predict "what move would a strong human player make here?" It was trained on 160,000 games played by strong amateur players.

Output: a probability distribution over all possible moves. "Given this board position, move X is 35% likely, move Y is 22% likely..."

This replaced brute-force search with learned intuition about promising moves.

### Component 2: The Value Network

A separate neural network trained to answer: "Given this board position, who is likely to win, and by how much?"

This is the game's equivalent of a "position evaluation" — instead of searching to the end of the game (impossible), estimate the outcome from here.

### Component 3: Monte Carlo Tree Search (MCTS)

The orchestrator. Uses the policy network to select which moves are worth exploring, and the value network to evaluate positions without full search, then combines these signals to choose the actual move to play.

```
┌──────────────────────────────────────────────────────────────────────┐
│           ALPHAGO'S DECISION PROCESS                                 │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  CURRENT BOARD POSITION                                              │
│         │                                                            │
│         ▼                                                            │
│  POLICY NETWORK: "These 10 moves look promising"                    │
│         │                                                            │
│         ▼                                                            │
│  MCTS: Simulate each promising move, see what happens               │
│  For each simulated continuation:                                    │
│         │                                                            │
│         ▼                                                            │
│  VALUE NETWORK: "This branch leads to ~60% win probability"         │
│         │                                                            │
│         ▼                                                            │
│  MCTS combines: How promising was the move? + How good does        │
│  the resulting position look? → BEST MOVE                          │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Self-Play: The Core Innovation

AlphaGo learned from human games — but the truly revolutionary step was self-play.

After the initial supervised learning phase (learning from human games), AlphaGo played millions of games against previous versions of itself. When it won, the moves that led to the win were reinforced. When it lost, the moves were penalised.

The intuition: you learn Go by playing Go. If you can play against an opponent that is exactly as good as you (yourself from yesterday), every game is maximally informative. You're always at your limit, always being challenged, always learning.

```
┌──────────────────────────────────────────────────────────────────────┐
│           SELF-PLAY TRAINING LOOP                                    │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  VERSION 1 (learned from human games)                               │
│         │                                                            │
│         ▼                                                            │
│  Play 1000 games: V1 vs V1                                          │
│  Update weights based on who wins                                   │
│         │                                                            │
│         ▼                                                            │
│  VERSION 2 (slightly better than V1)                                │
│         │                                                            │
│         ▼                                                            │
│  Play 1000 games: V2 vs V2                                          │
│  ... (repeat millions of times) ...                                 │
│         │                                                            │
│         ▼                                                            │
│  FINAL VERSION — superhuman                                          │
│                                                                      │
│  Scale: AlphaGo trained on ~30 million self-play games             │
│  No human ever had or could have that many games of practice       │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

Key insight: **self-play removes the ceiling on performance.** Human-level data is the starting point, but the system can improve far beyond any human because it can practice more than any human ever could.

---

## AlphaGo Zero: Removing the Human Data

In October 2017, DeepMind published AlphaGo Zero — a version that started with only the rules of Go and no human game data whatsoever.

Within 3 days of self-play: beat the version that defeated Lee Sedol (world champion).
Within 40 days: became the strongest Go player in history.

This proved something profound: **human knowledge is not required for superhuman performance.** Given a clear reward signal (win/lose), a system can discover everything a human expert knows — and then discover strategies beyond human expert knowledge.

AlphaGo Zero also rediscovered classic human opening sequences and Go theory from scratch — and then moved beyond them. It invented new forms of play that Go experts found "alien" and "beautiful."

---

## AlphaZero: Any Game

One month after AlphaGo Zero, DeepMind released AlphaZero — the same algorithm, applied without modification to chess, shogi (Japanese chess), and Go.

- Chess: became the strongest chess engine in history within 4 hours of training, beating the previous champion Stockfish (a 30-year optimised chess engine) 28-0 with 72 draws.
- Go: matched AlphaGo Zero.
- Shogi: best in the world.

Same algorithm. Different reward signal. No game-specific engineering. This generalisation was shocking to the AI field.

---

## The Connection to Modern AI

The ideas from AlphaGo now appear everywhere in modern AI:

```
┌──────────────────────────────────────────────────────────────────────┐
│           ALPHAGO IDEAS IN MODERN AI                                 │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  SELF-PLAY → RLHF (Reinforcement Learning from Human Feedback):    │
│  Instead of playing against yourself, generate two outputs and     │
│  have humans choose the better one. Same principle: the reward     │
│  signal teaches the model which outputs are better.                │
│  Used in: GPT-4, Claude, all major LLMs                            │
│                                                                      │
│  MCTS → TEST-TIME REASONING:                                        │
│  o1/o3 reasoning models use tree-search-like exploration during    │
│  the thinking phase — generating multiple reasoning paths and      │
│  evaluating which are worth pursuing further                       │
│                                                                      │
│  VALUE NETWORK → PROCESS REWARD MODELS:                            │
│  PRMs (from Session 113) are the LLM equivalent of the value      │
│  network — evaluating how good a reasoning path is mid-way         │
│                                                                      │
│  SELF-PLAY → DEBATE + CONSTITUTIONAL AI:                           │
│  Claude's Constitutional AI uses a form of self-critique —        │
│  the model generates a response, then critiques it against         │
│  principles, then revises. A form of self-play.                    │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

---

## The Key Lesson: Reward Signal + Self-Play + Scale

The AlphaGo story encodes a single lesson that is now the foundation of AI capabilities research:

**Give a system:**
1. A clear, verifiable reward signal (win/lose; correct/incorrect answer; human prefers A over B)
2. The ability to practice against itself or generate its own training data
3. Enough compute to do this at scale

**Result:** Superhuman performance — even without human expert data as a starting point.

This is why the AI capabilities labs are so focused on finding domains with clear reward signals. In games: win/lose. In maths: provably correct/incorrect. In coding: does the code pass the tests? In science: does the prediction match experiments?

The domains where AI is hardest to improve (creative writing, strategy, ethics) are precisely the domains where the reward signal is unclear or requires human judgment.

---

## The Match That Changed Everything

On March 9, 2016, AlphaGo beat Lee Sedol, the 18-time world Go champion, 4-1. The match was watched by 200 million people in Asia. In game 2, AlphaGo played "Move 37" — a move so strange that commentators thought it was a computer error. No human would ever play there. Lee Sedol left the room for 15 minutes.

Move 37 was correct. AlphaGo had discovered a strategy humans had never found in 2,500 years of playing Go.

The moment crystallised something: AI wasn't just doing human tasks more efficiently. It was discovering things beyond human knowledge.

---

## What This Means for ECHO India

Self-play and reinforcement learning are directly relevant to how ECHO India could improve AI systems over time:

- A candidate screening model can be retrained on outcomes: which candidates who were hired went on to perform well? That outcome data is the "win/lose" signal — feed it back into the model to improve screening quality.

- An HR chatbot can use RLHF: collect feedback on which responses employees found helpful, and use that signal to fine-tune the model. Same principle as AlphaGo's self-play — the reward signal (human preference) teaches the model which outputs are better.

- The crucial investment: instrumentation. You need to measure outcomes to have a reward signal. If ECHO India tracks which recommended policies led to lower attrition, which interview questions predicted performance, and which onboarding actions drove retention — those are your reward signals, and they can power continuous improvement.

---

## Key Takeaway

AlphaGo beat the world Go champion by combining deep learning (policy and value networks) with Monte Carlo Tree Search and — crucially — self-play: generating millions of training games by playing against itself. AlphaGo Zero removed even the need for human data, starting from scratch and reaching superhuman performance through pure self-play. AlphaZero proved the same algorithm works across any game.

The core insight that now powers modern AI: a clear reward signal plus self-generated training data plus scale equals superhuman performance. This same principle now drives RLHF in LLMs, reasoning model training, and the entire trajectory toward AGI.
