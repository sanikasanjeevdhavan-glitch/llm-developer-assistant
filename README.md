# LLM Developer Assistant

> Agentic backend tool for code review, bug detection, optimization, and contextual chat — built with **FastAPI · LangChain · OpenAI**.

[![CI](https://github.com/YOUR_USERNAME/llm-developer-assistant/actions/workflows/ci.yml/badge.svg)](https://github.com/YOUR_USERNAME/llm-developer-assistant/actions)

---

## Live Demo

**Frontend:** [https://YOUR_USERNAME.github.io/llm-developer-assistant](https://YOUR_USERNAME.github.io/llm-developer-assistant)  
**API Docs:** [https://your-backend.up.railway.app/docs](https://your-backend.up.railway.app/docs)

---

## Project Structure

```
llm-developer-assistant/
├── backend/
│   ├── main.py              ← FastAPI app (single-file MVP)
│   ├── requirements.txt
│   ├── Dockerfile
│   └── .env.example
├── frontend/
│   └── index.html           ← Standalone UI — works on any static host
└── .github/
    └── workflows/
        └── ci.yml           ← GitHub Actions CI
```

---

## Quickstart (local)

```bash
# 1. Clone
git clone https://github.com/YOUR_USERNAME/llm-developer-assistant
cd llm-developer-assistant/backend

# 2. Install deps
pip install -r requirements.txt

# 3. Add your key
cp .env.example .env
# then edit .env and set OPENAI_API_KEY=sk-...

# 4. Run
python main.py
# → http://localhost:8000
# → http://localhost:8000/docs  (Swagger UI)
```

Then open `frontend/index.html` directly in your browser — it connects to `localhost:8000` automatically.

---

## Deploy the backend — Railway (recommended, free tier)

1. Push this repo to GitHub
2. Go to [railway.app](https://railway.app) → **New Project → Deploy from GitHub repo**
3. Select your repo, set **Root Directory** to `backend`
4. Add environment variable: `OPENAI_API_KEY` = your key
5. Railway auto-detects the `Dockerfile` and deploys
6. Copy the generated URL (e.g. `https://llm-dev-assistant.up.railway.app`)

### After deploying — update the frontend

Open `frontend/index.html` and find this line near the top of the `<script>`:

```js
const API_BASE = window.BACKEND_URL || 'http://localhost:8000';
```

Change `'http://localhost:8000'` to your Railway URL:

```js
const API_BASE = window.BACKEND_URL || 'https://llm-dev-assistant.up.railway.app';
```

---

## Host the frontend — GitHub Pages (free)

1. Go to your repo → **Settings → Pages**
2. Set source: **Deploy from a branch → main → /frontend**
3. Your site is live at `https://YOUR_USERNAME.github.io/llm-developer-assistant`

Or drag `frontend/index.html` to [Netlify Drop](https://app.netlify.com/drop) for instant hosting.

---

## Embed on your portfolio website

Add this anywhere in your portfolio's HTML:

```html
<!-- Option A: iframe embed -->
<iframe
  src="https://YOUR_USERNAME.github.io/llm-developer-assistant"
  style="width:100%; height:700px; border:none; border-radius:12px;"
  title="LLM Developer Assistant Demo"
></iframe>
```

Or link to it as a project:

```html
<a href="https://YOUR_USERNAME.github.io/llm-developer-assistant" target="_blank">
  Live Demo →
</a>
```

To pass your backend URL dynamically into the iframe (if you don't want to hardcode it in `index.html`), add this to your portfolio page:

```html
<script>
  // Tell the iframe which backend to use
  const iframe = document.querySelector('iframe');
  iframe.onload = () => {
    iframe.contentWindow.BACKEND_URL = 'https://llm-dev-assistant.up.railway.app';
  };
</script>
```

---

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/analyze-code` | Explain, detect bugs, suggest fixes |
| POST | `/optimize-code` | Return optimized code + improvements |
| POST | `/chat` | Contextual chat with memory |
| POST | `/analyze-multi` | Analyze multiple files concurrently |
| GET  | `/health` | Liveness probe |
| DELETE | `/chat/memory/{id}` | Clear session memory |

Full interactive docs at `/docs` (Swagger UI).

---

## Tech Stack

- **FastAPI** — async Python web framework
- **LangChain** — LLM orchestration & prompt management
- **OpenAI GPT-4o-mini** — underlying model
- **Pydantic v2** — request/response validation
- **Docker** — containerised deployment
- **GitHub Actions** — CI on every push

---

## Architecture

```
FastAPI Routes → Service Layer → LLM Wrapper (LangChain/OpenAI)
                      ↓               ↓
               Response Cache    Conversation Memory
               (in-memory SHA)   (sliding window, 5 turns)
```

Key design decisions and tradeoffs are documented inline in `backend/main.py`.
