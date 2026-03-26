# Cyb3rVolt3x AI Sentinel v2.2.0

**Enterprise-grade multi-tenant AI SaaS platform**  
Built by **Syed Abrar aka Cyb3rVolt3x** — [sentinelreign.com](https://sentinelreign.com)

---

## What Is This?

You host the Python backend ("Mainframe") and sell license keys to clients who install the WordPress plugin ("Bridge"). The plugin autonomously researches, writes, fact-checks, SEO-optimises, and publishes articles — fully hands-free.

```
WordPress Site (Plugin)  ──→  Your Ubuntu Server (Backend)  ──→  NVIDIA NIM / AI
       ↑                              ↑
  Client installs plugin         You deploy this
  enters license key             Python FastAPI app
```

---

## What's in This Package

```
sentinel-saas-deploy.zip
├── backend/                   ← Python FastAPI backend (deploy on your Ubuntu server)
│   ├── main.py                  FastAPI entry point
│   ├── requirements.txt         All Python dependencies (pinned versions)
│   ├── .env.example             Config template — copy to .env and fill in
│   ├── DEPLOY.md                Full Ubuntu 22.04 step-by-step deployment guide
│   ├── core/                    Config, database, tenant auth, license manager
│   ├── api/                     Route handlers (license, agent, memory, oauth)
│   ├── models/                  SQLAlchemy database models
│   ├── services/                AI router, rate limiter, OAuth, agents
│   └── alembic/                 Database migration scripts
│
├── plugin/
│   └── sentinel-ai-swarm-v2.2.0.zip  ← Upload this to WordPress
│
└── README.md                  ← This file
```

---

## Deploy in 5 Steps

### 1. Read the full guide first
Open `backend/DEPLOY.md` — it has every command you need for Ubuntu 22.04, PostgreSQL, Nginx, and systemd.

### 2. Install backend

```bash
cd backend/
python3.11 -m venv venv && source venv/bin/activate
pip install -r requirements.txt
cp .env.example .env
nano .env          # fill in your API keys (see guide below)
```

### 3. Set minimum required keys in `.env`

| Variable | How to get it |
|---|---|
| `NVIDIA_NIM_API_KEY` | [build.nvidia.com](https://build.nvidia.com/explore/discover) — free tier available |
| `JWT_SECRET_KEY` | Run: `openssl rand -hex 32` |
| `FOUNDER_SECRET` | Run: `python3 -c "import secrets; print(secrets.token_hex(32))"` |
| `DATABASE_URL` | `sqlite:///./sentinel_reign.db` for SQLite, or your PostgreSQL URL |

### 4. Start the server

```bash
python -m uvicorn main:app --host 0.0.0.0 --port 8000
```

Test it: `curl http://your-server-ip:8000/health`

### 5. Install the WordPress Plugin

1. WordPress Admin → Plugins → Add New → Upload Plugin
2. Upload `plugin/sentinel-ai-swarm-v2.2.0.zip`
3. Activate → **Sentinel AI Management** → Settings
4. Set **Backend URL** to `https://api.your-domain.com`
5. Enter your license key

---

## AI Provider Chain (automatic fallback)

1. **NVIDIA NIM** — `moonshotai/kimi-k2.5` (primary, rate-limited to 38 RPM)
2. **OpenAI GPT-4o** — set `OPENAI_API_KEY`
3. **Google Gemini** — set `GEMINI_API_KEY` or use OAuth browser login
4. **Alibaba Qwen** — set `QWEN_API_KEY`

---

## Pricing Tiers

| Tier | Price | `plan_type` |
|------|-------|-------------|
| Starter | $99/mo | `starter` |
| Pro | $249/mo | `pro` |
| Agency | $500/mo | `agency` |

---

## Key API Endpoints

| Path | Description |
|------|-------------|
| `GET /health` | Health check — confirm backend is live |
| `GET /docs` | Interactive API docs (Swagger) |
| `POST /api/v1/license/verify` | License verification |
| `POST /api/v1/agent/trigger` | Trigger AI content pipeline |
| `GET /api/v1/agent/status` | AI telemetry (model, CPU, RAM) |
| `POST /api/v1/oauth/initiate` | Google OAuth for Gemini login |

---

## Support

- Website: [sentinelreign.com](https://sentinelreign.com)
- Author: Syed Abrar aka Cyb3rVolt3x

*Cyb3rVolt3x AI Sentinel v2.2.0 — Enterprise AI SaaS*
# SentinelWorking
# SentinelWorking
