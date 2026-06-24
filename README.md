# AI Coach — Zero to Hero

From zero AI knowledge to being the most AI-literate PM in the room.

**148 sessions** covering everything from how neural networks work to AI strategy, ethics, agents, fine-tuning, and building AI products — all explained for Product Managers.

---

## How to Use This Repo with Claude

This repo is designed to be used **interactively with Claude Code** (or claude.ai). Claude acts as your personal AI coach — it reads the sessions, quizzes you, evaluates your answers, and tracks your progress.

### Setup (one time)

```bash
git clone https://github.com/pandit30/AI-Coach-Zero-to-Hero.git
cd AI-Coach-Zero-to-Hero
claude  # opens Claude Code in this directory
```

### Start Learning

Once Claude is open, just say:

```
start session 1
```

Claude will:
1. Walk you through the key ideas from the session
2. Quiz you with 5–10 PM-scenario questions
3. Give you feedback on each answer
4. Update your progress in `PROGRESS.md`

### Useful Commands

| What you say | What happens |
|---|---|
| `start session 1` | Begin session 1 |
| `start session 42` | Jump to any session |
| `next` | Move to the next session |
| `hint` | Get a nudge without the answer |
| `skip` | Skip a question (counts as 0) |
| `score` | See your current progress |
| `recap` | Get a 5-bullet summary of the current session |
| `where am I` | Show where you are in the curriculum |

---

## Curriculum Structure

| Act | Sessions | Topic |
|-----|----------|-------|
| Act 1 | 001–014 | The Foundation — Neural networks, weights, training |
| Act 2 | 015–032 | The LLM — Transformers, tokens, scaling laws |
| Act 3 | 033–046 | Talking to AI — Prompt engineering |
| Act 4 | 047–060 | Memory and RAG — Vector search, retrieval |
| Act 5 | 061–075 | AI That Acts — Agents, tools, orchestration |
| Act 6 | 076–099 | Teaching AI — Fine-tuning, RLHF, inference |
| Act 7 | 100–109 | AI at Scale — Cost, multimodal, deployment |
| Act 8 | 110–128 | Advanced AI — Reasoning, science AI, ethics |
| Act 9 | 129–148 | AI in the Real World — Evals, strategy, product |

---

## File Structure

```
session-XXX-<topic>.md   → Reading material for each session
quiz-XXX-<topic>.md      → Quiz questions (Claude uses these — don't peek!)
PROGRESS.md              → Your progress tracker (auto-updated by Claude)
CLAUDE.md                → Instructions for Claude (the coach brain)
```

---

## Progress Tracking

Your progress is saved in `PROGRESS.md` after every completed quiz. It tracks:
- Score per session
- Date completed
- Personal notes from Claude about your strengths and gaps
- Running average and streak

You can check it any time by saying `score` to Claude, or opening `PROGRESS.md` directly.

---

## Who This Is For

Product Managers who want to:
- Understand what their AI engineering team is actually building
- Make better decisions about when to use RAG vs fine-tuning vs prompting
- Speak credibly about AI to leadership and to engineers
- Identify real AI opportunities in their product
- Navigate AI ethics, regulations, and risk

No coding required. No maths. Just clear thinking and PM judgment.
