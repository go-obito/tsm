# Football News API

FastAPI backend using SQLModel and an async PostgreSQL connection.

## Requirements

- Python 3.12
- Docker Desktop (for the local PostgreSQL and Redis services)

## Local setup

From the repository root, start the development services:

```powershell
docker compose up -d
```

In PowerShell, create a virtual environment, install dependencies, and copy the
example environment file:

```powershell
cd backend
py -3.12 -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
Copy-Item .env.example .env
```

On macOS or Linux, use:

```sh
cd backend
python3.12 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
cp .env.example .env
```

The example configuration connects to the services exposed on localhost by
Docker Compose. Apply database migrations and start the API:

```sh
alembic upgrade head
uvicorn app.main:app --reload
```

The API is available at <http://localhost:8000>, with interactive docs at
<http://localhost:8000/docs>. `GET /health` executes a query against PostgreSQL
and returns `{"status":"ok"}` when the database is reachable. A database
connection failure returns an HTTP error rather than a successful health
response.

## Database migrations

Create a revision after defining or changing SQLModel tables:

```sh
alembic revision --autogenerate -m "describe the change"
alembic upgrade head
```

Alembic loads `backend/.env` and uses the same async PostgreSQL URL as the API.
