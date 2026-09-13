# Environment

## Local services (docker compose)

| Service | Port | Purpose |
|---|---|---|
| MongoDB | 27017 | conversation state (threads, messages, memories, runs) |
| Redis | 6379 | cache + BullMQ queues |
| Typesense | 8108 | search / RAG index |

Start them: `docker compose up -d`

## Variables

| var | purpose |
|---|---|
| `NODE_ENV` | development / test / production |
| `PORT` | HTTP port (default 3001) |
| `FRONTEND_URL` | CORS origin |
| `LARAVEL_API_URL` | Laravel API base URL (domain data: tickets/agents/SLA) |
| `LARAVEL_API_KEY` | service key sent as `X-Api-Key` |
| `LARAVEL_API_TIMEOUT_MS` | request timeout |
| `LLM_PROVIDER` | adapter selector (default `openai`) |
| `OPENAI_BASE_URL` | OpenAI-compatible endpoint base URL |
| `OPENAI_API_KEY` | provider key |
| `OPENAI_CHAT_MODEL` | chat model id |
| `OPENAI_CLASSIFIER_MODEL` | classification model id |
| `OPENAI_EMBEDDING_MODEL` | embedding model id (for RAG) |
| `MONGODB_URI` / `MONGODB_DB` | conversation state |
| `REDIS_URL` | cache + BullMQ |
| `TYPESENSE_HOST` / `TYPESENSE_PORT` / `TYPESENSE_PROTOCOL` / `TYPESENSE_API_KEY` | search/RAG |
| `TYPESENSE_COLLECTION_PREFIX` | prefix for this service's collections |
| `WEBHOOK_VERIFY_TOKEN` | webhook verification token |
| `WEBHOOK_SIGNING_SECRET` | webhook HMAC signing secret |
| `LOG_LEVEL` / `LOG_DIR` | logging |
| `RATE_LIMIT_WINDOW_MS` / `RATE_LIMIT_MAX` | rate limiting |

## Setup

```bash
nvm use                 # Node 22
npm install
cp .env.example .env    # fill in values
docker compose up -d
npm run mongo:init
npm run typesense:init
npm run dev             # http://localhost:3001
```

## Notes

- Config is validated at boot — the process should fail fast on a missing required var.
- Never commit `.env`; only `.env.example` belongs in the repo.
