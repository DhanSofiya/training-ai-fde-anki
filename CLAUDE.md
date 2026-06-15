# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

Install:
```
python -m pip install -e .
```

Run the app:
```
uvicorn app.main:app --reload
```

Run all tests:
```
pytest
```

Run a single test:
```
pytest tests/test_routes.py::test_create_and_get_deck
```

## Architecture

A FastAPI flashcard app with spaced-repetition scheduling and one Claude-powered endpoint.

- `app/routes.py` — all HTTP endpoints; entry and exit point for every request
- `app/services.py` — scheduling logic between the HTTP layer and the database
- `app/db.py` — all SQLite reads and writes; the only file that touches the database
- `app/ai.py` — the Claude integration; raises `AINotConfigured` if no API key is set
- `app/models.py` — Pydantic models used for request validation and response serialisation

Data lives in `anki.db` in the repo root (created on first run). On startup, `main.py` initialises the schema and seeds sample data if the database is empty.

## Conventions

**Test database isolation:** the test suite points at a throwaway SQLite file via the `ANKI_DB_PATH` environment variable. This is set in `tests/conftest.py`. Do not hardcode database paths in tests.

**AI endpoint is optional:** `POST /decks/{deck_id}/generate` requires `ANTHROPIC_API_KEY`. Without it the endpoint returns 503 cleanly. All other endpoints and all tests work without a key.

**No API key in tests:** the test suite mocks `ai.generate_cards` directly — it never calls the real Claude API. Keep it that way.

## Behaviour

Always ask for clarification before making any changes to the code.
