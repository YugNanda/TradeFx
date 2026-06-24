# TradeX — Real-Time Trading Dashboard

> A full-stack financial market dashboard built with the MERN stack, featuring live price feeds, financial news, technical signals, portfolio tracking, and WebSocket-powered real-time updates.


---

## 📸 Preview

> *(Add a screenshot of your dashboard here)*

---

## ✨ Features

- 📈 **Real-Time Prices** — Live market data for stocks, crypto, forex and indices via Alpha Vantage
- 📰 **Financial News Feed** — Symbol-specific news pulled from Newsdata.io with smart 6-hour caching
- 🤖 **Technical Signals** — Rule-based BUY/SELL/HOLD signals using SMA crossover and momentum analysis
- 💼 **Portfolio Tracker** — Track your holdings, P&L, and allocation breakdown
- ⭐ **Watchlist** — Save and monitor your favourite instruments
- 🔔 **Price Alerts** — Get notified via WebSocket when a symbol hits your target price
- 🔐 **JWT Authentication** — Secure register/login with token-based sessions
- ⚡ **WebSocket Live Ticks** — Socket.io pushes price updates to all connected clients in real time
- 💾 **Smart Caching** — MongoDB-backed price and news cache with automatic staleness detection and API budget guards

---

## 🛠️ Tech Stack

### Frontend
| Technology | Purpose |
|---|---|
| React 18 + Vite | UI framework and build tool |
| React Router v6 | Client-side routing |
| Axios | HTTP client |
| Socket.io Client | Real-time WebSocket connection |
| ApexCharts | Interactive price charts |
| Lucide React | Icon library |
| Tailwind CSS | Utility-first styling |

### Backend
| Technology | Purpose |
|---|---|
| Node.js + Express | REST API server |
| Socket.io | WebSocket server for live ticks |
| MongoDB + Mongoose | Database and ODM |
| JWT + bcryptjs | Authentication and password hashing |
| Alpha Vantage API | Real-time market price data |
| Newsdata.io API | Financial news feed |

### Deployment
| Service | Role |
|---|---|
| Vercel | Frontend hosting (React/Vite) |
| Render | Backend hosting (Express/Node) |
| MongoDB Atlas | Cloud database |

---

## 📁 Project Structure

```
tradex/
├── client/                   # React frontend
│   ├── src/
│   │   ├── api/              # Axios API calls
│   │   ├── components/       # Reusable UI components
│   │   ├── context/          # Auth + Market context providers
│   │   ├── pages/            # Route-level page components
│   │   └── styles/           # Global styles
│   ├── .env.example
│   ├── vercel.json
│   └── vite.config.js
│
└── server/                   # Express backend
    ├── config/               # Database connection
    ├── controllers/          # Route handler logic
    ├── jobs/                 # Price scheduler (cron-style)
    ├── middleware/            # Auth middleware
    ├── models/               # Mongoose schemas
    ├── routes/               # Express route definitions
    ├── services/
    │   └── aiProviders/      # Alpha Vantage + Newsdata.io clients
    ├── utils/                # Symbol list, helpers
    └── index.js              # Server entry point
```

---

## ⚙️ Local Development Setup

### Prerequisites
- Node.js v18+
- MongoDB (local or Atlas)
- Alpha Vantage API key — [get free key](https://alphavantage.co/support/#api-key)
- Newsdata.io API key — [get free key](https://newsdata.io)



## 📊 API Budget Management

Both external APIs are free tier — the app manages daily limits automatically.

| Provider | Free Limit | Usage Strategy |
|---|---|---|
| Alpha Vantage | 25 req/day | 3h cache + budget guard, falls back to stale cache |
| Newsdata.io | 200 req/day | 6h cache per symbol, falls back to stale cache |

Monitor live usage anytime:
```
GET /api/health
```

---

## 🌐 API Endpoints

### Auth
```
POST   /api/auth/register
POST   /api/auth/login
GET    /api/auth/me
```

### Market
```
GET    /api/market/quote/:symbol
GET    /api/market/quotes
```

### Portfolio
```
GET    /api/portfolio
POST   /api/portfolio
DELETE /api/portfolio/:id
```

### Watchlist
```
GET    /api/watchlist
POST   /api/watchlist
DELETE /api/watchlist/:symbol
```

### Alerts
```
GET    /api/alerts
POST   /api/alerts
DELETE /api/alerts/:id
```

### News
```
GET    /api/news/:symbol
GET    /api/news
```

### Signals
```
GET    /api/signals/:symbol
```

---

## 🔌 WebSocket Events

| Event | Direction | Description |
|---|---|---|
| `join:market` | Client → Server | Subscribe to symbol price ticks |
| `leave:market` | Client → Server | Unsubscribe from symbol ticks |
| `join:user` | Client → Server | Join personal alert room |
| `tick` | Server → Client | Live price update for a symbol |
| `alert:triggered` | Server → Client | Fires when a price alert is hit |

---

## 🧠 Key Engineering Decisions

**Why Alpha Vantage over AI-generated prices?**
Phase 1 used Gemini/OpenAI to fetch prices via web grounding — creative but unreliable and quota-heavy. Switched to Alpha Vantage for deterministic, real market data with zero hallucination risk.

**Why rule-based signals over AI?**
SMA crossover signals are transparent and explainable — users can see exactly why a BUY fired. AI black-box signals added latency and API cost with no accuracy benefit for this use case.

**Why split Vercel + Render?**
Socket.io requires persistent WebSocket connections, which Vercel Serverless Functions don't support. Render provides a persistent Node.js process; Vercel handles the static React build.

---

## 📄 License

MIT License — feel free to fork, modify and build on this.

---

## 👤 Author

**Yug Nanda**
B.Tech Information Technology — SKIT Rajasthan (2024–2028)

---

> *Built as a personal project to bridge institutional trading concepts (ICT methodology, smart money) with full-stack engineering.*
