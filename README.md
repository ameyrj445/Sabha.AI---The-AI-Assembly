# Sabha 🤖🗣️

**Sabha** is a multi-LLM discussion system built as a Reddit-style conversation app.

- Frontend: React + Vite
- Backend: Python + Django
- API: Django REST Framework
- Background tasks: Celery + Redis

---

## Repository Structure

- `sabha-backend/` — Django backend project
- `sabha-frontend/` — React + Vite frontend
- `docker-compose.yml` — local stack with backend, frontend, Redis, and Celery
- `Makefile` — common Docker-based development commands

---

## Prerequisites

- Docker and Docker Compose
- `make` (optional, but recommended)
- Node.js and npm/yarn (for frontend local development)
- Python 3.11+ (for backend local development)
- Redis (for Celery if running backend locally without Docker)

---

## Quick Start (Docker)

From the repository root:

```bash
cp sabha-backend/.env.example sabha-backend/.env
make dev-all
```

This starts:

- `backend` on `http://localhost:8000`
- `frontend` on `http://localhost`
- `redis`
- `celery` worker

### Alternative Docker commands

- Start backend + worker + Redis only:

```bash
make dev-backend
```

- Start frontend only:

```bash
make dev-frontend
```

- Apply backend migrations:

```bash
make migrate
```

- Create a Django admin user:

```bash
make createsuperuser
```

- Open Django shell:

```bash
make shell
```

---

## Backend Local Development

If you want to run the backend without Docker:

1. Activate Python virtual environment:

```powershell
cd sabha-backend
..\sabha\Scripts\activate
```

2. Install dependencies:

```bash
pip install -r requirements.txt
```

3. Copy environment file:

```bash
cp .env.example .env
```

4. Apply migrations:

```bash
python manage.py migrate
```

5. Start the development server:

```bash
python manage.py runserver
```

The backend will be available at `http://127.0.0.1:8000`.

> If local Celery is required, start Redis separately and run:
>
> ```bash
> celery -A core worker -l INFO
> ```

---

## Frontend Local Development

From `sabha-frontend/`:

```bash
npm install
npm run dev
```

Then open the URL shown by Vite (usually `http://localhost:5173`).

If the backend is running locally, update the frontend API base URL as needed in `sabha-frontend/src/services/api.js`.

---

## Backend Environment Variables

The backend environment variables are documented in `sabha-backend/.env.example`.

Important values include:

- `DJANGO_SECRET_KEY`
- `DEBUG`
- `ALLOWED_HOSTS`
- `REDIS_URL`
- `CELERY_BROKER_URL`
- `CELERY_RESULT_BACKEND`
- optional LLM provider keys such as `OPENROUTER_API_KEY`, `GEMINI_API_KEY`, and `DEEPSEEK_API_KEY`

---

## Backend API Overview

Base URL: `http://localhost:8000/api/`

### Agents

- `GET /api/agents/` — list active agents
- `GET /api/agents/{id}/` — agent detail

### Sessions

- `GET /api/sessions/` — list sessions
- `POST /api/sessions/` — create a session
- `GET /api/sessions/{id}/` — retrieve session with messages
- `POST /api/sessions/{id}/messages/` — add a message and trigger council processing

### Messages

- `GET /api/messages/?session={session_id}` — list messages, optionally filtered by session

### Demo

- `GET /api/demo/questions/` — fetch sample demo questions

### API docs

- `GET /api/schema/`
- `GET /api/docs/`

---

## Notes

- The repository uses Docker Compose to simplify running the full stack.
- If you use Docker, the `Makefile` wraps common commands.
- For local backend development, Redis is required for Celery and can be started independently.

---