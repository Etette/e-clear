# eClear — LLM User Modeling & Recommendation Agent

> DSN × BCT Challenge Submission
> An agentic system that understands users deeply enough to simulate their
> reviews and deliver personalized recommendations with a Nigerian lens.

**Live Demo:** 
- [UI](https://e-clear.up.railway.app/ui)
- [Swagger API](https://e-clear.up.railway.app/docs)

---

## What It Does

eClear has two core capabilities:

**Task A — Review Simulation**
Give it a user ID and any item (a restaurant, a product, a place) and it
simulates what that specific user would write and rate. Capturing their
tone, rating habits and Nigerian voice.

**Task B — Personalized Recommendation**
Give it a user ID and it builds a behavioral persona from their review
history, then recommends items they would genuinely enjoy. Works in three
modes: standard (with history), cold start (no history needed) and
cross-domain (transfer preferences across categories).

---

## Quick Start

### Option 1 — Use the Live Demo

Visit the live URL above. No setup needed. Use the dropdown to pick a
real user from the dataset and start exploring.

### Option 2 — Run Locally

**Prerequisites**
- Python 3.11+
- A Groq API key or any LiteLLM-compatible provider

**1. Clone the repo**
```bash
git clone 
```

**2. Create a virtual environment**
```bash
python -m venv venv

# Windows
venv\Scripts\activate

# Mac/Linux
source venv/bin/activate
```

**3. Install dependencies**
```bash
pip install -r requirements.txt
```

**4. Set up environment variables**

Copy the example and fill in your keys:
```bash
cp .env.example .env
```

Open `.env` and set:
GROQ_API_KEY=your_groq_api_key_here
LLM_MODEL=groq/llama-3.3-70b-versatile
DATA_DIR=./data/dataset

**5. Add the dataset**

The filtered dataset is included in `data/dataset/`.
If you want to run on the full Yelp dataset, download it from kaggle, place the three JSON
files in `data/`, then update `DATA_DIR=./data` in your `.env`.

**6. Start the server**
```bash
uvicorn app.main:app --reload
```

Visit `http://localhost:8000/ui` or `http://localhost:8000/docs` in your browser.

## API Endpoints


| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/ui` | Web interface |
| GET | `/health` | Health check |
| GET | `/sample-users` | Get real users from dataset for the UI dropdown |
| GET | `/users/{user_id}/history` | Inspect a user's review history |
| GET | `/businesses/sample` | Browse available businesses |
| POST | `/task-a/simulate-review` | Simulate a review for a user |
| POST | `/task-a/simulate-review/cold-start` | Simulate without user history |
| POST | `/task-b/recommend` | Get personalized recommendations |
| POST | `/task-b/recommend/cold-start` | Recommend without user history |
| POST | `/task-b/recommend/cross-domain` | Cross-domain recommendations |

**Example — Simulate a review:**
```bash
curl -X POST http://localhost:8000/task-a/simulate-review \
  -H "Content-Type: application/json" \
  -d '{
    "user_id": "your_user_id_here",
    "item": {
      "item_name": "Chicken Republic",
      "item_category": "Fast Food, Nigerian",
      "item_description": "Popular Nigerian fast food chain"
    }
  }'
```

**Example — Cold start recommendation:**
```bash
curl -X POST http://localhost:8000/task-b/recommend/cold-start \
  -H "Content-Type: application/json" \
  -d '{
    "persona": {
      "description": "A young professional in Lagos, loves trying new food spots",
      "preferences": "Values good ambience, budget conscious, loves spicy food",
      "context": "Looking for a dinner spot for a first date"
    },
    "top_n": 5
  }'
```

---

## Datasets Used

| Dataset | Source | Size | Purpose |
|---------|--------|------|---------|
| Yelp Reviews | kaggle | 50k reviews (filtered) | Primary behavioral data |
| Goodreads Books | Kaggle | 8,205 books | Cross-domain item pool |
| Nigerian Foods KB | Kaggle | 109 foods | Nigerian item recommendations |
| Jumia Nigeria | Kaggle | 7 products | Nigerian e-commerce context |

---

## Environment Variables

| Variable | Required | Description |
|----------|----------|-------------|
| `GROQ_API_KEY` | Yes | Your Groq API key |
| `LLM_MODEL` | Yes | Model string e.g. `groq/llama-3.3-70b-versatile` |
| `DATA_DIR` | No | Path to dataset folder (default: `./data/dataset`) |

---

## Generating the Filtered Dataset Locally

If you have the full Yelp dataset and want to regenerate the filtered
version:

```bash
python scripts/explore.py
```

This reads from `data/` and writes filtered copies to `data/dataset/`.
Original files are never modified.

---

## Deployment

To deploy your own instance:

1. Fork this repo
2. Sign up at [railway.app](https://railway.app)
3. Create a new project → Deploy from GitHub repo
4. Add environment variables in Railway dashboard
5. Railway detects the Python app automatically and deploys

---

## Author

**Etette Etok**
DSN × BCT LLM Agent Challenge | May 2026