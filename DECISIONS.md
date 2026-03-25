# DECISIONS.md — quote-app

## 1. No Database — Hardcoded Quotes Array

**Decision:** Store quotes as a hardcoded JS array, not in a database.

**Rationale:** The app is a read-only display with a fixed, static dataset of 10 quotes. A database would add operational overhead (setup, migrations, connection management) with zero benefit. The array lives in `src/data/quotes.js` and can be edited like any other source file.

**Trade-off:** Changing quotes requires a code deploy. Acceptable for this use case.

---

## 2. Single API Endpoint

**Decision:** Expose one endpoint: `GET /api/quote`.

**Rationale:** The only requirement is "show a random quote on button click." A single endpoint that returns one random quote satisfies this completely. No list endpoint, no pagination, no filtering — YAGNI.

---

## 3. Server-side Randomisation

**Decision:** Pick the random quote on the server, not the client.

**Rationale:** If the full quotes array were sent to the client, the browser could trivially enumerate all quotes. Server-side selection keeps the collection opaque and makes the "surprise" genuinely random from the user's perspective.

---

## 4. React + Vite (same stack as todo-app)

**Decision:** Use React 18 + Vite + Tailwind, consistent with the todo-app frontend.

**Rationale:** Consistency across projects reduces cognitive switching cost. Vite's HMR is fast. Tailwind avoids writing custom CSS for a simple one-page app.

---

## 5. Port 3001 for Backend (not 4000)

**Decision:** Run the backend on port 3001 instead of 4000.

**Rationale:** Avoids port collision if both `todo-app` and `quote-app` servers are run simultaneously on the same machine during development.

---

## 6. CORS Restricted to Frontend Origin

**Decision:** Allow only `http://localhost:5173` in CORS config.

**Rationale:** Even a read-only API should not be callable from arbitrary origins. This is a minimal security posture consistent with the todo-app architecture decisions.
