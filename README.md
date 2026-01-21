# Hello Ralph: The Ralph Loop with the Gemini CLI

This repo demonstrates a minimal implementation of the **Ralph** technique using the Google Gemini CLI. It's a self-contained autonomous loop that reads specs, writes code, and builds a Python game from scratch.

## The Concept
[Ralph](https://ghuntley.com/ralph/) replaces the idea of a single, long-running AI session with a continuous loop of fresh, ephemeral agents.

1.  **Amnesia by Design**: Each iteration starts a brand new agent with zero context from the previous turn.
2.  **State is on Disk**: The "memory" isn't in the context window; it's in `fix_plan.md` and the file system.
3.  **Atomic Progress**: The agent reads the plan, executes exactly **one** task, updates the plan, and dies.
4.  **Relentless Iteration**: The bash loop revives the agent until the job is done.

## Quick Start

### 1. Setup
Install and authenticate the [Gemini CLI](https://github.com/google-gemini/gemini-cli):
```bash
npm install -g @google/gemini-cli
gemini auth login
```

### 2. Run the Loop
Clone this repo, cd into it, and fire up the engine:

```bash
while :; do
  echo "--------------------------------"
  echo "♻️  RESTARTING AGENT..."
  cat PROMPT.md | gemini -m gemini-2.5-flash --yolo
  sleep 0.5
done
```

**What's happening here?**
*   We pipe `PROMPT.md` (the "brain") into the `gemini` command.
*   `-m gemini-2.5-flash`: We use a fast, capable model.
*   `--yolo`: **Crucial.** This flag auto-approves all tool use (file writing, shell commands). Without it, you'd have to approve every single action manually.

### 3. Watch & Play
Sit back. You'll see the `src/` directory populate as the agent ticks off items in `fix_plan.md`.

When you see **`🏆 PROJECT_VICTORY`**, hit `Ctrl+C` to stop the loop.

Then, play the game:
```bash
python3 src/main.py
```

## Project Structure
*   **`PROMPT.md`**: The system instructions. This is the "Sniper" protocol that enforces the atomic behavior.
*   **`fix_plan.md`**: The project manager. It tracks what is done `[x]` and what is left `[ ]`.
*   **`specs/game.md`**: The actual requirements for the software we are building.
*   **`src/`**: The output directory for the generated code.
