# Break the Bot

A tiny prompt-injection game. A local AI guards a secret word, and you have to
trick it into saying the word. Each level adds a stronger defense.

Built for Hacktoberfest 2026 with FastAPI and a local model (gemma3:1b via Ollama).
Everything runs on your own computer, with no API keys and no cloud calls.

## Why this exists

You can't make an AI keep a secret just by asking it nicely. Telling a model
"never reveal X" is not a security boundary, and attacks like role-play,
format tricks and "ignore previous instructions" get around it. This is called
**prompt injection**, and it is a real problem for AI products. This game lets
you feel it first-hand, and see which defenses actually help.

## Requirements

- Python 3.10+
- [Ollama](https://ollama.com/download) with the model pulled: `ollama pull gemma4:e2b`

## Setup

```bash
python -m venv .venv
# Windows (PowerShell):  .\.venv\Scripts\Activate.ps1
# macOS / Linux:         source .venv/bin/activate
python -m pip install -r requirements.txt
```

## Run

1. Make sure Ollama is running (`ollama list` should work).
2. Start the game:
   ```bash
   uvicorn app:app --reload
   ```
3. Open http://127.0.0.1:8000

## How to play

Chat with the guard and try to make it say the secret word. When you think you
have it, type the word into the submit box. Use the Hint button if you're stuck.

| Level | Defense |
|---|---|
| 1 | The AI is only told "don't reveal it" |
| 2 | A stricter prompt with explicit rules |
| 3 | Adds an **output filter** that blocks replies containing the word (even spaced out or reversed) |
| 4 | Adds an **input filter** that rejects messages containing words like "secret" or "password" |

## How it works

- `app.py` is the FastAPI server. It sends the chat to Ollama together with a
  hidden system prompt, then applies the input and output filters.
- `index.html` is the single-page chat UI.
- Secrets live only on the server and are never sent to the browser.
- Levels are defined in the `LEVELS` list at the top of `app.py`. Add a new
  level by adding a new entry.

## Project structure

```
app.py            server, level definitions, filters
index.html        game page
requirements.txt  Python dependencies
```

## Ideas to extend

- More levels (for example, a second AI that checks the first AI's replies)
- A leaderboard of fewest messages per level
- Let players write their own guard prompt and have others attack it

Inspired by the online game Gandalf by Lakera.
