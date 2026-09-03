# Kubernetes YAML Validator
> Paste a manifest. Get errors, warnings, and a production-ready fixed YAML — instantly.

**Live:** [kubernetes-yaml-validator.vercel.app](https://kubernetes-yaml-validator.vercel.app)
![Preview](./assets/preview.png)

---

## What it does

Paste any Kubernetes manifest and get a full AI-powered validation — errors that will break your deployment, warnings for security and operational gaps, and a corrected YAML with fixes applied automatically.

No more silent failures after `kubectl apply`. No more hunting label mismatches by hand. No more discovering missing resource limits in production.

---

## Why I built this

Kubernetes YAML is the most silent killer in DevOps. A single label mismatch between `selector.matchLabels` and `template.metadata.labels` means zero pods ever start — and `kubectl apply` won't tell you why. Missing `resources.limits` causes node starvation. Hardcoded secrets in env vars is a security incident waiting to happen.

Most engineers only find these issues in production. This tool catches them before `kubectl apply`.

---

## Resources supported

| Resource | Key checks |
|----------|-----------|
| **Deployment** | selector/label match, resource limits, probes, security context |
| **Service** | port/targetPort alignment, selector match, type validity |
| **Ingress** | ingressClassName, TLS secretName, backend config |
| **HPA** | min < max replicas, metrics defined, scaleTargetRef |
| **ConfigMap** | non-empty data, no secrets in plain text |
| **Secret** | valid type, base64 encoding |
| **PodDisruptionBudget** | minAvailable XOR maxUnavailable |
| **Namespace** | reserved name check |
| **ServiceAccount** | automountServiceAccountToken |

---

## Severity classification

Every issue is tagged — not dumped as a flat list:

- **CRITICAL** — Will break deployment. Fix before applying.
- **HIGH** — Security risk or likely runtime failure.
- **MEDIUM** — Operational gap that will hurt under load.
- **LOW** — Best practice deviation, low immediate risk.

---

## How it works

1. Paste your Kubernetes manifest into the editor
2. Enter your free [Groq API key](https://console.groq.com/keys) — stored in your browser only
3. Hit Validate — get errors, warnings, and a corrected YAML
4. Copy the fixed manifest directly into your repo

---

## Stack

- **Frontend** — React + Vite, deployed on Vercel
- **Backend** — FastAPI + Python, deployed on Render (Docker)
- **AI** — Groq `llama-3.3-70b-versatile` via BYOK
- **Storage** — localStorage only (key, validation history)

---

## Key engineering decisions

- **BYOK** — Your Groq key is stored in localStorage only. Never sent to any server other than Groq directly.
- **Severity-tagged output** — Issues are classified strictly, not inferred loosely. CRITICAL only fires when deployment will actually fail.
- **Fixed YAML output** — Not just a list of problems. The tool returns a corrected manifest with security context, resource limits, and probes added where missing.
- **Cold start handling** — Render free tier wakes on first request. UI surfaces a clear message — no silent failures.

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