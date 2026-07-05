# GitHub Repo Health Check: LLM/AI Tooling Analysis

Comparing development activity, responsiveness, and sustainability across 5 popular open-source LLM/AI tools — using live data pulled directly from the GitHub API.

**[[Live Dashboard on Tableau Public](https://public.tableau.com/app/profile/chiru.ratnala/viz/Github_Repo_Analysis/Dashboard) →]**

---

## Problem Statement

Popularity alone doesn't tell you if an open-source project is worth depending on. Star counts measure hype, not health. This project looks past surface-level popularity to answer:

> **How healthy and sustainable is the development activity behind popular open-source LLM/AI tooling projects?**

Specifically:
- Are these projects driven by active human contributors, or mostly automated (bot) updates?
- How fast do maintainers respond to and resolve issues?
- Is the issue backlog growing or shrinking?
- Which project is the most reliable to build on long-term?

## Repos Analyzed

| Repo | Description |
|---|---|
| `langchain-ai/langchain` | LLM application framework |
| `ollama/ollama` | Local LLM runner |
| `ggerganov/llama.cpp` | LLM inference in C++ |
| `huggingface/transformers` | ML model library |
| `run-llama/llama_index` | LLM data framework |

## Tech Stack

| Tool | Purpose |
|---|---|
| **Python** (`requests`, `pandas`) | Pulled live data from the GitHub REST API, cleaned and structured it |
| **SQLite** | Stored cleaned data in relational tables; used SQL for aggregation and analysis |
| **Tableau Public** | Built the interactive dashboard and story-based presentation |

## Data Pipeline

```
GitHub API  →  Python (extract + clean)  →  SQLite (store + query)  →  Tableau (visualize)
```

1. **Extract** — Pulled repo metadata, commit history, and issues for all 5 repos using GitHub's REST API (authenticated with a personal access token)
2. **Clean** — Structured raw JSON into pandas DataFrames; parsed dates, split multi-value labels, separated bot vs. human commit authors
3. **Store** — Loaded cleaned tables into a local SQLite database (`github_analytics.db`)
4. **Query** — Wrote SQL to calculate resolution times, backlog ratios, contributor counts, and label frequency
5. **Visualize** — Built an interactive Tableau dashboard and a guided Story to walk through the findings

## Key Metrics Analyzed

- Stars, forks, and watchers per repo
- Bot vs. human commit ratio
- Top contributors per repo
- Average issue resolution time (days to close)
- Open vs. closed issue ratio (backlog health)
- Most common issue labels (bug, enhancement, question, etc.)
- Days since last push (activity recency)

## Key Findings

- **llama.cpp** carries the heaviest backlog risk — 82 open issues vs. only 20 closed, a ratio far worse than any other repo in the set
- **ollama** resolves issues fastest (avg. 0.34 days) but also shows the highest bot-commit share (an even 150/150 split with human commits)
- Development effort is concentrated — a small handful of top contributors account for a disproportionate share of total commits across all 5 repos
- "Bug" and "bug-unconfirmed" dominate issue labels for ollama and llama.cpp, suggesting active triage bottlenecks rather than a shortage of real problems

## Dashboard Features

- KPI summary cards: total stars, total commits, total contributors, total forks, repo with most open issues
- Stars & forks comparison across repos
- Bot vs. human commit breakdown
- Average issue resolution time by repo
- Open vs. closed issues (backlog health)
- Top 10 issue labels (heat map / treemap)
- Top 10 contributors leaderboard
- Interactive filters (repo selector, recency filter)
- A guided **Story** walking through each finding with a question-driven narrative


## How to Reproduce

1. Generate a [GitHub Personal Access Token](https://github.com/settings/tokens) (classic, `public_repo` scope)
2. Install dependencies:
   ```bash
   pip install requests pandas
   ```
3. Run `github_pull.ipynb`, adding your token where indicated
4. This will pull fresh data, clean it, and write it into `github_analytics.db` plus the CSV exports
5. Open the CSVs or `.db` file in Tableau Public to rebuild or refresh the dashboard

## Limitations

- Commit and issue data is capped at ~300 records per repo per pull (rate-limit/scope tradeoff) — not the full historical record for larger repos
- "Health" is measured using activity signals (resolution speed, commit source, backlog ratio) — it does not account for code quality, test coverage, or documentation quality
- Data reflects a single point-in-time snapshot; re-running the pipeline is needed to track trends over time

### Author 
## Chiru Ratnala
**[LinkedIn](https://www.linkedin.com/in/chiru-ratnala)**  **[Portfolio](https://chiruratnala.netlify.app/)**
