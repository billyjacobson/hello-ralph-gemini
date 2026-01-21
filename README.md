# Hello Ralph: The Ralph Loop with the Gemini CLI

  
This repo shows you how to get started with a minimal implementation of the **Ralph** technique using the Google Gemini CLI. With the specs and prompt, you'll learn the basic technique and spin up a simple Python game.


## 🧠 The Concept
The core idea behind [Ralph](https://ghuntley.com/ralph/) is to replace a single, long-running AI session with a continuous loop of fresh agents.

1. **Amnesia by Design**: Each iteration starts a brand new agent with no memory of the previous turn.
2. **State is on Disk**: The "memory" is stored in the spec files and `fix_plan.md`.
3. **Atomic Progress**: The agent reads the plan, executes exactly **one** task, updates the plan, and terminates.
4. **Relentless Iteration**: The loop keeps grinding until the plan is empty.

## 🛠️ Prerequisites
You need the Google Gemini CLI installed and authenticated:

```bash
npm install -g @google/gemini-cli
gemini auth login
```

## 🚀 How to Run
Clone the repo and then to start the autonomous development loop, run this command in your terminal:

```bash
while :; do
  echo "--------------------------------"
  echo "♻️  RESTARTING AGENT..."
  cat PROMPT.md | gemini -m gemini-2.5-flash --yolo
  sleep 0.5
done
```

Play the game by running: 
```bash
python src/main.py
```

## 📂 Project Structure
- **`PROMPT.md`**: The system instructions that tell the agent how to behave (the "Sniper" protocol).
- **`fix_plan.md`**: The source of truth for the project's state. It tracks what's done and what's next.
- **`specs/game.md`**: The requirements for the application being built.
- **`src/`**: Where the agent will build the source code.