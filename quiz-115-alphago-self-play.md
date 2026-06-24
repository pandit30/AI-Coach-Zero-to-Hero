# Quiz — Session 115: AlphaGo & Self-Play

> **Instructions for Claude:** Ask questions one at a time. Wait for the student's answer before moving to the next question. Use the "What to look for" guide to evaluate each answer. After all questions, calculate the score (e.g. 5/5) and update PROGRESS.md.

---

## Question 1 — Type: Concept Check

Why was Go considered unbeatable for computers even after chess computers beat Kasparov in 1997? Use the specific numbers the session provides to make the comparison concrete.

**What to look for:** Chess: 8×8 board, ~10^120 possible games — brute force with pruning works (IBM Deep Blue). Go: 19×19 board, ~10^170 possible games (more than atoms in the universe), with highly intuitive positional strategy that "can't be calculated through — you have to feel the board." Go masters said a computer would never beat them. The key difference: chess can be solved with deep tactical calculation; Go requires something like intuition and aesthetic judgment. There is no brute-force approach that works for Go's search space, which is why the neural network approach was necessary.

---

## Question 2 — Type: Concept Check

AlphaGo combined three components. Name them and explain what each one contributes to the decision-making process.

**What to look for:** (1) Policy Network: a CNN trained on 160,000 games from strong amateur players to predict "what move would a strong human player make?" — outputs a probability distribution over possible moves, replacing brute-force search with learned intuition about promising moves; (2) Value Network: a separate neural network answering "given this board position, who is likely to win and by how much?" — evaluates positions without searching to the end of the game; (3) Monte Carlo Tree Search (MCTS): the orchestrator — uses the policy network to select which moves are worth exploring, and the value network to evaluate positions without full search, then combines signals to choose the actual move.

---

## Question 3 — Type: Application

The session draws a direct connection between AlphaGo's self-play and RLHF (Reinforcement Learning from Human Feedback) used in GPT-4, Claude, and all major LLMs. Explain the parallel.

**What to look for:** AlphaGo self-play: the system plays against itself, gets a win/lose reward signal, and updates based on which moves led to wins. RLHF: instead of playing against yourself, the LLM generates two outputs and humans choose the better one. Same principle: a reward signal (human preference) teaches the model which outputs are better, improving the model iteratively. The session also draws the MCTS → test-time reasoning parallel (o1/o3 use tree-search-like exploration in their thinking phase), and the Value Network → Process Reward Model parallel (PRMs evaluate reasoning paths mid-way, like the value network evaluates board positions mid-game). Students should demonstrate understanding that the AlphaGo principles now power modern AI training.

---

## Question 4 — Type: Concept Check

AlphaGo Zero started with only the rules of Go — no human game data — and became the strongest player in history within 40 days. What profound principle does this demonstrate about AI and human knowledge?

**What to look for:** AlphaGo Zero proved: "Human knowledge is not required for superhuman performance." Given a clear reward signal (win/lose) and the ability to generate infinite training data through self-play, a system can discover everything a human expert knows — and then discover strategies beyond human knowledge. It rediscovered classic human Go theory from scratch and then moved beyond it, inventing forms of play that experts found "alien and beautiful." AlphaZero then extended this to chess and shogi without game-specific engineering — same algorithm, different reward signal, superhuman in each game within hours. The principle: reward signal + self-generated training data + scale = superhuman performance without human expert data.

---

## Question 5 — Type: Application

The session says ECHO India should invest in "instrumentation" as the direct application of the AlphaGo lesson. What does that mean concretely, and why is it the most important investment before building AI systems that improve over time?

**What to look for:** Instrumentation = measuring outcomes to create a reward signal. From the session: Track which candidates who were hired went on to perform well → that is the "win/lose" signal for a screening model. Collect feedback on which chatbot responses employees found helpful → that is the preference signal for RLHF of the HR chatbot. Track which recommended policies led to lower attrition, which interview questions predicted performance, which onboarding actions drove retention → those are reward signals for continuous improvement. Without instrumentation, there is no reward signal. Without a reward signal, self-improvement is impossible. The AlphaGo lesson: you need to know whether your actions led to good outcomes before you can learn to take better ones.

---
