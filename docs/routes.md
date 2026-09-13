# Routes

Base path: `/api` (health and metrics are unprefixed).

| method | path | auth | db | notes |
|---|---|---|---|---|
| GET | `/health` | no | no | returns 200 / 503 |
| GET | `/metrics` | no | no | Prometheus metrics |
| GET | `/api/webhooks` | no | no | webhook verification (`hub.challenge`) |
| POST | `/api/webhooks` | HMAC | no | ingest messages + comments |
| GET | `/api/tickets` | yes | no | list via Laravel API |
| GET | `/api/tickets/:id` | yes | no | detail via Laravel API |
| POST | `/api/tickets/:id/messages` | yes | yes | **SSE**: routes + responds |
| GET | `/api/tickets/:id/messages` | yes | yes | transcript (MongoDB) |
| POST | `/api/tickets/:id/escalate` | yes | yes | force handoff to an agent |

## SSE protocol

`POST /api/tickets/:id/messages` with `Accept: text/event-stream` emits:

```
event: status   data: {"status":"understanding"}
event: chunk    data: {"chunk":"..."}      (repeated)
event: complete data: {"messageId":"...","threadId":"..."}
event: error    data: {"error":"..."}
```

A keep-alive comment (`: keepalive`) is sent every 15 seconds.

## Conventions

- Errors: `{ success: false, error: { code, message } }`.
- Mutating routes require auth (Laravel token validation).
- Webhooks must verify the signature and dedupe by event id.
