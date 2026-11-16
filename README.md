# why-this-company

Interview prep from a company's own filings. Pick a company and a role. The service reads
the company's DART business report, shows what stands out with the numbers and where they
come from, and asks 3–7 interview questions that end on "why this company?"

It never writes the answer for you. You pick and you write.

Upstage MABC 2026 hackathon, final round. [Live service](https://why-this-company.onrender.com)
(free tier: the first load can take a minute).

## How it works

Rules decide, the LLM only writes, and a verifier checks every number.

| Step | Who | What |
|---|---|---|
| Judge | `judge.py` (rules) | Reads DART figures and picks a finding type: concentration, sudden change, dependence, stagnation, falling margins, heavy investment |
| Map | Mapping table | Uses only the cell where the finding type meets the applicant's role (6 types × 6 role families) |
| Write | Solar Pro 4 | Fills that cell with the company's own numbers and names |
| Verify | `render.py`, `verify.py`, `ir.py` | Checks the cell, every figure against the DART source, and every quote against the IR PDF |

If the LLM output still fails the format check after two retries, the report falls back to the rule-based findings alone.
Uploaded IR PDFs are turned into text in the browser and never stored on the server.

## Results

200 listed companies, 3 requests at a time (2026-09-16 build):

- **97.3%** of 2,666 figures in the reports match the DART source
- median **21 s** per report

## Run locally

Python 3.12. Set `OPEN_DART_API_KEY` and `UPSTAGE_API_KEY` in `.env`.

```bash
python -m venv .venv
.venv/bin/python -m pip install -r backend/requirements.txt
.venv/bin/python -m uvicorn backend.app:app --port 9527
```

Then open http://127.0.0.1:9527.

## Repository

| Path | Contents |
|---|---|
| `backend/` | FastAPI app: filing analysis (`/api/jobs`), IR questions (`/api/ir-questions`), single-file frontend |
| `why-this-company/` | The analysis engine, first built as a Claude skill for the preliminary round |
| `Docs/` | Product spec and change notes (Korean) |

The service UI and reports are in Korean.
