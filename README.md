# Nexus Social Discourse Experiment

This repo is an experiment that adapts the Nexus (formerly MindMeld) deliberation tooling to study how social media discussions evolve. Instead of running the production Nexus service, we are prototyping workflows that ingest public conversations (e.g., Bluesky posts, AI-assisted debates) and visualize how statements cluster, how participants align, and where new associations emerge.

The codebase still contains the full-stack Nexus app (React frontend + Node/Express backend), but it is being repurposed as an analysis playground. Expect rough edges, hard-coded datasets, and plenty of exploratory scripts under `datafetching/`.

## Goals

- Collect and normalize social media discussions into the Nexus CSV format (participants, statements, votes).
- Use Nexus' graphing UI to explore alignments, statement clusters, and attention patterns.
- Prototype new adapters that link statements back to their source posts for richer qualitative analysis.
- Document what works (and what breaks) when using Nexus for rapid discourse research.

## Experiment Layout

```
frontend/                 Nexus React app with custom visualizations for discourse data
frontend/public/data/     Generated CSV + TXT snapshots of each conversation
datafetching/             Notebooks and scripts for scraping, cleaning, and associating posts
backend/                  Existing Nexus API (used sparingly in this experiment)
bot_app/                  Early bot prototypes for preference gathering and post replay
```

Key scripts/notebooks:
- `datafetching/url_constellation_search.ipynb` – pulls conversation threads for inspection.
- `datafetching/process_*` – transforms raw exports into Nexus-ready CSVs.
- `frontend/src/services/csvAdapter.ts` – experimental adapter that links votes to original posts.

## Getting Started

> These steps mirror the upstream Nexus instructions, but you really only need them if you plan to tweak the UI or run local analyses.

1. **Install dependencies**
   ```bash
   npm run install:all
   ```

2. **Environment variables**
   - `frontend/.env`
     - `REACT_APP_OAUTH_CLIENT_ID` – optional for auth flows
     - `REACT_APP_BACKEND_URL` – usually `http://localhost:3001` during local development
   - `backend/.env`
     - `DB_USER`, `DB_HOST`, `DB_NAME`, `DB_PASSWORD`
     - `OPENAI_API_KEY`
     - `GOOGLE_CLIENT_ID`

3. **Run both apps**
   ```bash
   npm run dev
   ```
   The frontend boots on port 3000 and the backend on port 3001. Most experiment work happens locally against CSV files, so you can also serve `frontend` alone for static data exploration.

## Working With Data

1. Place raw discussion exports (TXT/CSV) under `datafetching/`.
2. Run the relevant `process_*.py` script to generate participants/statements/votes CSVs.
3. Copy the outputs into `frontend/public/data/` to make them available to the UI.
4. Open the Nexus graph/analysis views to inspect clusters, alignments, and associations.

Because this is exploratory research, commits may include large CSVs or temporary files. Clean up before sharing changes broadly.

## Current Status & Next Steps

- Several Bluesky and AI-generated discussions are already ingested (`ai-scientist-discussion`, `recursive-reasoning-discussion`, etc.).
- Post-to-statement association work is in progress inside `frontend/src/services/csvAdapter.ts`.
- Future work: smoother ingestion tooling, automated linking back to source URLs, and clearer storytelling around insights.

Feedback, ideas, or interesting datasets are welcome—open an issue or drop files into `datafetching/` and document your process. This is research-in-progress, not an official Nexus release.
