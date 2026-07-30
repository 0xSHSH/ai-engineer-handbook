# 8.2 API Documentation Template (`api_doc_template.md`)

```markdown
# API Reference

All endpoints are versioned under `/api/v1/` and require a JWT token for authentication.

## Authentication
### `POST /auth/login`
```http
POST /api/v1/auth/login HTTP/1.1
Content-Type: application/json

{ "username": "your_user", "password": "your_pass" }
```
**Response**
```json
{ "access_token": "<jwt>", "refresh_token": "<jwt>" }
```

### `POST /auth/refresh`
Refreshes an expired access token.
```http
POST /api/v1/auth/refresh HTTP/1.1
Authorization: Bearer <refresh_token>
```
**Response**
```json
{ "access_token": "<new_jwt>" }
```

## Endpoints
### `POST /chat`
**Description**: Sends a user message to the LLM and returns a response with optional citations.
**Request**
```json
{ "message": "What is the capital of France?" }
```
**Response**
```json
{ "answer": "Paris", "sources": ["wiki_page_123"] }
```

### `GET /history`
**Description**: Retrieves the last N conversation turns for the authenticated user.
**Query Parameters**
- `limit` (int, default=20)
**Response**
```json
[ { "role": "user", "content": "..." }, { "role": "assistant", "content": "..." } ]
```

*Add additional endpoints as needed, following the same pattern.*
```
