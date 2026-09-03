# AI DevOps Incident Copilot
> Paste a failed log. Get the root cause and fix — in seconds.

**Live:** [ai-dev-ops-incident-copilot.vercel.app](https://ai-dev-ops-incident-copilot.vercel.app)
![Preview](./assets/preview.png)

---

## What it does

Paste any CI/CD or infrastructure error log. Get an instant AI-powered diagnosis — root cause, severity classification, and actionable fix steps.

No more guessing what `exit code 1` means. No more scrolling through 500 lines to find the actual error. No more Stack Overflow rabbit holes.

---

## Why I built this

Debugging pipeline failures is one of the biggest time sinks in DevOps. I've watched engineers spend 45 minutes on an error that had a one-line fix buried at line 312 of a log. The context switch alone kills momentum.

This tool reads the log, finds the signal, explains the cause, and tells you exactly what to change — so you can get back to shipping.

---

## Log types supported

| Source | What it handles |
|--------|----------------|
| **Azure DevOps** | Pipeline task failures, agent errors, artifact issues |
| **Kubernetes** | CrashLoopBackOff, OOMKilled, ImagePullBackOff, probe failures |
| **Terraform** | State lock errors, provider auth, plan/apply failures |
| **Docker** | Build errors, registry push/pull failures, layer issues |
| **GitHub Actions** | Step failures, permission errors, runner issues |
| **Helm** | Release failures, chart rendering errors, hook timeouts |

---

## Severity classification

Every analysis returns a severity level with strict rules — not vibes:

- **LOW** — Warning or config drift, no immediate action needed
- **MEDIUM** — Functional failure, fix before next deploy
- **HIGH** — Production impact likely, fix now
- **CRITICAL** — Active outage or data risk, drop everything

---

## How it works

1. Paste your error log into the log viewer
2. Enter your free [Groq API key](https://console.groq.com/keys) or paid [Claude API key](https://console.anthropic.com/) — stored in your browser only
3. Select your provider (Groq = free, Claude = higher quality)
4. Hit Analyze — get root cause, severity, and fix steps
5. Copy the output directly into your incident ticket or PR

---

## Stack

- **Frontend** — React + Vite, deployed on Vercel
- **Backend** — FastAPI + Python, deployed on Render (Docker)
- **AI** — Groq `llama-3.3-70b-versatile` (free) or Anthropic `claude-haiku-4-5` (paid) via BYOK
- **Storage** — localStorage only (keys, history — max 50 entries)

---

## Key engineering decisions

- **Dual BYOK** — Groq or Claude, user chooses per request. API key never touches the server.
- **Strict severity prompt rules** — Severity is enforced via prompt constraints, not inferred loosely. CRITICAL only fires on genuine outage signals.
- **Cold start handling** — Render free tier wakes on first request. UI surfaces a clear "Backend is waking up. Wait 30 seconds and retry." — no silent failures.
- **ADO-style UI** — Breadcrumb nav, pipeline job sidebar, syntax-colored log viewer. Feels native to the DevOps workflow.
- **Animated pipeline flow** — Ingest → Analyze → Classify → Fix nodes light up during analysis. Visual feedback without spinner noise.

---

## Running locally

**Backend**
```bash
cd backend
pip install -r requirements.txt
uvicorn main:app --reload --port 8000
```

**Frontend**
```bash
cd frontend
npm install
npm run dev
```

Frontend → `http://localhost:5173` | Backend → `http://localhost:8000`

---

## Part of a DevOps tools portfolio

| Project | Description | Live |
|---------|-------------|------|
| AI DevOps Incident Copilot | Paste failed log → root cause + fix | [Live](https://ai-dev-ops-incident-copilot.vercel.app) |
| Azure Pipeline YAML Generator | Plain English → azure-pipelines.yml | [Live](https://azure-pipeline-yaml-generator.vercel.app) |
| Kubernetes YAML Validator | Paste K8s YAML → errors, warnings + fixed version | [Live](https://kubernetes-yaml-validator.vercel.app) |
| DevOps Interview Copilot | Practice DevOps interview Q&A with AI | [Live](https://devops-interview-copilot.vercel.app) |

---

Built by [Manoj Kumar](https://linkedin.com/in/manoj2606) — Senior DevOps Engineer