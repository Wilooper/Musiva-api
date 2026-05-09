# Musiva API

Musiva API is a ready-to-deploy **FastAPI** backend built on top of `ytmusicapi` and extended with Apple Music, Deezer, and Spotify trending sources.

It is designed for developers who want a deployable music backend without rebuilding everything from scratch.

**Demo in action:** https://musicanaz.vercel.app/

## Overview

- **Language/stack:** Python, FastAPI, Uvicorn
- **Main app entrypoint:** `app.py`
- **Interactive API docs:** `/docs` (Swagger UI), `/redoc`, `/openapi.json`
- **Health endpoint:** `/health`

> Important: This API is **public/stateless**. It does **not** implement user authentication or per-user private data management.

## Quick Start (Local)

### 1) Clone and install dependencies

```bash
git clone https://github.com/Wilooper/Musiva-api.git
cd Musiva-api
python -m venv .venv
source .venv/bin/activate  # Windows: .venv\Scripts\activate
pip install --upgrade pip
pip install -r requirements.txt
```

### 2) Configure environment

```bash
cp .env.example .env
```

Edit `.env` only if you need optional integrations (proxy, Spotify, docs password, cache location).

### 3) Run the API

```bash
uvicorn app:app --host 0.0.0.0 --port 7860 --reload
```

Open:
- API root: `http://localhost:7860/`
- Health: `http://localhost:7860/health`
- Swagger: `http://localhost:7860/docs`

## Quick Start (Docker)

```bash
docker build -t musiva-api .
docker run --rm -p 7860:7860 --env-file .env musiva-api
```

Then access `http://localhost:7860/`.

## Environment Configuration

Use `.env.example` as the source of truth for supported variables.

Common variables:
- `PROXY_USERNAME`, `PROXY_PASSWORD` (optional proxy support)
- `SPOTIFY_CLIENT_ID`, `SPOTIFY_CLIENT_SECRET` (optional Spotify trending source)
- `DOCS_PASSWORD` (password for `/docs`, `/redoc`, `/openapi.json`)
- `CACHE_DIR` (disk cache location)

## Main API Endpoints

Key endpoints available in `app.py`:

- Meta: `/`, `/health`, `/cache_stats`, `DELETE /cache`
- Search: `/search`, `/search_suggestions`
- Discovery: `/home`, `/charts`, `/trending`, `/genres`, `/mood_categories`, `/mood_playlists/{params}`, `/explore`
- Song/media: `/song/{video_id}`, `/video_info/{video_id}`, `/stream/{video_id}`, `/now_playing/{video_id}`, `/related_songs/{video_id}`, `/upnext/{video_id}`, `/watch/{video_id}`, `/lyrics/{browse_id}`
- Artist/album/playlist/podcast: `/artist/{artist_id}`, `/artist/{artist_id}/songs`, `/artist/{artist_id}/albums`, `/album/{album_id}`, `/playlist/{playlist_id}`, `/podcast/{podcast_id}`
- External sources: `/apple_music/top_songs`, `/apple_music/top_albums`, `/deezer/charts`, `/spotify/trending`
- Public channel data: `/user/{channel_id}`, `/user_playlists/{channel_id}`

## Authentication and Data Model (Important)

This project intentionally has **no user auth system**:

- No signup/login/logout
- No JWT/session auth for API business endpoints
- No user account/profile management
- No per-user private playlists/favorites persistence in this repository

All API endpoints are intended for **public/stateless** usage.

## Additional Documentation

- [FEATURES.md](./FEATURES.md) — what the API provides vs does not provide
- [DIRECTORY_STRUCTURE.md](./DIRECTORY_STRUCTURE.md) — repository/file organization guide
- [.env.example](./.env.example) — environment variable template

## Contributing

1. Fork the repo
2. Create a feature/fix branch
3. Keep changes focused and minimal
4. Open a PR with clear context
