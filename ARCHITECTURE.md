# ARCHITECTURE.md — quote-app

## Overview

`quote-app` is a minimal full-stack application that displays a random motivational quote. It follows a two-tier architecture: a React SPA on the frontend and a Node.js/Express API on the backend. There is no database — quotes are hardcoded in the backend.

---

## System Components

```
┌─────────────────────────────────────────────────────────┐
│                        Browser                          │
│                                                         │
│   ┌─────────────────────────────────────────────────┐   │
│   │              React SPA (port 5173)              │   │
│   │                                                 │   │
│   │  ┌────────────────────────────────────────┐    │   │
│   │  │              QuotePage                 │    │   │
│   │  │  - Displays current quote              │    │   │
│   │  │  - "New Quote" button                  │    │   │
│   │  └──────────────┬─────────────────────────┘    │   │
│   │           API Service Layer (fetch)             │   │
│   └───────────────────┬─────────────────────────────┘   │
└───────────────────────┼─────────────────────────────────┘
                        │ HTTP/REST (JSON)
┌───────────────────────▼─────────────────────────────────┐
│           Node.js / Express API (port 3001)             │
│                                                         │
│  ┌──────────────────────────────────────────────────┐   │
│  │  GET /api/quote → picks random from quotes[]     │   │
│  └──────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
```

---

## Frontend (React SPA)

| Concern       | Detail                                      |
|--------------|---------------------------------------------|
| Framework     | React 18 (Vite)                             |
| State         | React `useState` / `useEffect`              |
| HTTP client   | `fetch` (native browser API)               |
| Styling       | Tailwind CSS                                |

### Components

- **`App.jsx`** — root; holds `quote` state, triggers fetch on load and on button click
- **`QuoteCard.jsx`** — displays quote text and author
- **`services/api.js`** — thin fetch wrapper, centralises base URL

---

## Backend (Node.js / Express)

| Concern       | Detail                                       |
|--------------|----------------------------------------------|
| Runtime       | Node.js ≥ 20 LTS                            |
| Framework     | Express 4                                    |
| Data source   | Hardcoded array of 10 quotes in `quotes.js` |
| CORS          | `cors` middleware, allow `http://localhost:5173` |

### Layer Responsibilities

| Layer     | File                          | Responsibility                             |
|----------|-------------------------------|--------------------------------------------|
| Router    | `src/routes/quote.js`         | Maps `GET /api/quote` to controller        |
| Controller| `src/controllers/quote.js`    | Picks random quote, sends JSON response    |
| Data      | `src/data/quotes.js`          | Exports array of 10 hardcoded quotes       |

---

## Project Structure

```
quote-app/
├── client/              # React app (Vite + Tailwind)
│   ├── src/
│   │   ├── App.jsx
│   │   ├── components/
│   │   │   └── QuoteCard.jsx
│   │   └── services/
│   │       └── api.js
│   └── package.json
├── server/              # Express API
│   ├── src/
│   │   ├── app.js
│   │   ├── server.js
│   │   ├── data/
│   │   │   └── quotes.js
│   │   ├── routes/
│   │   │   └── quote.js
│   │   └── controllers/
│   │       └── quote.js
│   ├── tests/
│   │   └── quote.test.js
│   └── package.json
└── README.md
```

---

## Error Handling Pattern

```json
{ "error": { "code": "INTERNAL_ERROR", "message": "..." } }
```

---

## Security Considerations

- No user input accepted — read-only API, zero injection surface
- CORS restricted to frontend origin
- No authentication required (public read-only data)
