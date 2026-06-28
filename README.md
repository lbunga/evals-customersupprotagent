# Ivy — IT Helpdesk Evaluation Agent

A lean, production-flavored eval project: route an IT request → answer from a knowledge base (RAG) → or escalate, then grade it on **5 metrics** with **LangSmith** as the system of record.

## What's here

```
evals-customersupprotagent/
├── ivy_agent_evals.ipynb     # the notebook you run (build → eval → improve)
├── ivy_golden_dataset.csv      # 30 labeled test cases (the answer key)
├── ivy_kb/                      # 10 IT KB docs — Ivy retrieves answers from these
│   ├── password_reset.md  vpn.md  software_access.md  hardware.md
│   ├── mfa.md  printing.md  conference_rooms.md
│   └── security_incidents.md  acceptable_use.md  onboarding.md
├── .env.example                # copy to .env and add your keys
├── requirements.txt
└── .gitignore
```

## The 5 metrics
routing F1 · faithfulness · answer correctness · escalation correctness · p95 latency & cost

## Setup
1. `python -m venv .venv && .venv\Scripts\activate`   (Windows)
2. `pip install -r requirements.txt`
3. `copy .env.example .env`  then fill in `OPENAI_API_KEY` and `LANGSMITH_API_KEY`
   (LangSmith key: https://smith.langchain.com → Settings → API Keys)
4. Open `ivy_eval_scaffold.ipynb`, select the `.venv` kernel, run top to bottom.

## Run order (inside the notebook)
1. Sections 1–2: install + keys (confirm "Config loaded" before going on)
2. Sections 3–6: build Ivy (intents → FAISS KB → router → pipeline)
3. Section 7: sanity-check one query (and see it appear in LangSmith)
4. Section 8: upload the golden set as a LangSmith Dataset (`ivy-golden-v1`)
5. Section 9: run `evaluate()` → records the `ivy-baseline` experiment
6. Sections 10–11: local metrics + failure clustering for the writeup
7. Section 12: apply 3 improvements → records `ivy-v2` → compare in LangSmith
8. Section 13: LLM-judge alignment (hand-label ~25 rows, rare-class P/R/F1)
9. Section 14: report checklist

## LangSmith
Tracing is on by default (`LANGSMITH_TRACING=true`). Every run shows as one `ivy`
trace with nested router → retriever → answer steps, latency, and token cost.
Compare `ivy-baseline` vs `ivy-v2` in the Experiments → Compare view.

> Note: the repo is named for a customer-support agent; the implemented example is
> the IT-helpdesk variant (Ivy). Same architecture and metrics — rename freely.
