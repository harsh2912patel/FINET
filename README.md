# FINET — AI-Powered Personal Finance Platform

> Built for the Indian financial ecosystem · 

FINET is a full-stack personal finance web app that combines AI coaching, real-time market data, budget flow visualization, investment tracking, and a community hub — all in one place. Built specifically around Indian markets (NSE, BSE, SIPs, INR).

---

## Features

**FinCoach — AI Financial Advisor**
Chat interface powered by Google Gemini 2.5 Flash. Context-aware coaching based on your financial profile (age, tier, goal, capital). Adapts between beginner and advanced tone. Trained focus on Indian finance: SEBI, RBI, mutual funds, SIPs, tax laws.

**AI-Annotated News Feed**
Aggregates financial news from NewsAPI, Alpha Vantage, and Moneycontrol (scraped). Each article gets a Gemini-generated summary, cause analysis, portfolio impact, and key learning — personalized to your financial tier.

**Budget Flow Visualizer**
Interactive node-based diagram built with D3.js showing how income flows into Needs, Wants, and Investments. Add, edit, and delete categories in real time.

**Investment Portfolio Tracker**
Track mutual funds, stocks, and index funds. Monitor invested amount vs. current value with live ROI calculation.

**Loan Manager**
Track all active loans — home, car, education, personal. Key details: principal, remaining balance, interest rate, tenure, and EMI per loan.

**Live Market Pulse**
Real-time Sensex (^BSESN) and Nifty 50 (^NSEI) data via Alpha Vantage. 5-minute server-side caching for performance.

**Community Hub**
Real-time community feed backed by Supabase PostgreSQL. Post types: Query, Tactical Success, Intelligence Tip, Open Discussion. Toggle upvotes with one-vote-per-user enforcement.

**Gamification System**
XP points, levels, daily streaks, tier progression (Beginner → Advanced), and a financial milestone roadmap to keep users engaged.

---

## Tech Stack

| Layer | Technologies |
|---|---|
| **Frontend** | React 18, Vite, React Router v6, D3.js, Lightweight Charts, Axios |
| **Backend** | FastAPI, Python, SQLAlchemy 2.0, Uvicorn, BeautifulSoup4 |
| **AI** | Google Gemini 2.5 Flash |
| **Database & Auth** | Supabase (PostgreSQL + JWT Auth) |
| **Deployment** | Vercel (frontend + Python serverless functions) |
| **External APIs** | Alpha Vantage, NewsAPI, Moneycontrol (scraped) |

---

## Getting Started

### Prerequisites
- Node.js v18+
- Python 3.10+
- Supabase project (free tier works)
- API keys: Google Gemini, NewsAPI, Alpha Vantage

### Setup

```bash
# Clone
git clone https://github.com/harsh2912patel/finet.git
cd finet

# Frontend dependencies
npm install

# Python virtual environment
python -m venv venv
source venv/bin/activate      # macOS/Linux
venv\Scripts\activate         # Windows
pip install -r requirements.txt

# Environment variables
cp env.example .env
# Fill in your keys (see below)
```

### Environment Variables

```env
# Supabase
VITE_SUPABASE_URL=https://your-project.supabase.co
VITE_SUPABASE_ANON_KEY=your_anon_key
SUPABASE_URL=https://your-project.supabase.co
SUPABASE_JWT_SECRET=your_jwt_secret
SUPABASE_DB_PASSWORD=your_db_password

# AI
GEMINI_API_KEY=your_gemini_key

# News & Markets
NEWS_API_KEY=your_newsapi_key
ALPHA_VANTAGE_KEY=your_alpha_vantage_key
```

| Key | Where to get it |
|---|---|
| Supabase keys | Supabase Dashboard → Settings → API |
| `GEMINI_API_KEY` | [aistudio.google.com](https://aistudio.google.com/app/apikey) |
| `NEWS_API_KEY` | [newsapi.org](https://newsapi.org) |
| `ALPHA_VANTAGE_KEY` | [alphavantage.co](https://www.alphavantage.co/support/#api-key) |

### Run Locally

```bash
# Terminal 1 — Backend
PYTHONPATH=. python -m uvicorn server.main:app --reload --port 8000

# Terminal 2 — Frontend
npm run dev
```

Open [http://localhost:5173](http://localhost:5173)

---

## API Overview

All endpoints require a valid Supabase JWT in the `Authorization: Bearer <token>` header, except `/api/health`.

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/chat` | FinCoach AI chat (Gemini) |
| `GET` | `/api/news` | AI-annotated financial news |
| `GET` | `/api/market-pulse` | Live Sensex & Nifty 50 |
| `GET` | `/api/user/data` | All user data |
| `GET` | `/api/user/journey` | Gamification state |
| `POST` | `/api/user/transaction` | Add transaction |
| `POST` | `/api/user/portfolio` | Add portfolio item |
| `POST` | `/api/user/loans` | Add loan |
| `GET` | `/api/community/posts` | Community feed |
| `POST` | `/api/community/posts/{id}/upvote` | Toggle upvote |

---

## Security

- All API endpoints protected by Supabase JWT validation (HS256, RS256, ES256 supported)
- Every database query filters by `user_id` extracted from the JWT — users cannot access each other's data
- No hardcoded secrets; all credentials loaded from environment variables

---

## Deployment

Configured for Vercel — frontend served as a React SPA, backend runs as Python serverless functions via `api/index.py`.

```bash
vercel --prod
```

Routes: `/api/*` → FastAPI · `/*` → React SPA

---

## License

For educational and personal use. External API usage subject to respective terms of service (Google Gemini, NewsAPI, Alpha Vantage, Supabase).
