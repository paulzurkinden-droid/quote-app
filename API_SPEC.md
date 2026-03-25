# API_SPEC.md — quote-app

## Base URL

```
http://localhost:3001/api
```

All responses use `Content-Type: application/json`.

---

## Endpoints

### `GET /quote`

Return a single random motivational quote from the hardcoded collection.

**Request**

No parameters, no request body.

**Response `200 OK`**

```json
{
  "id": 4,
  "text": "The only way to do great work is to love what you do.",
  "author": "Steve Jobs"
}
```

| Field    | Type      | Description                          |
|---------|-----------|--------------------------------------|
| `id`     | `integer` | 1-based index within the quotes array |
| `text`   | `string`  | The quote text                        |
| `author` | `string`  | Attribution                           |

**Response `500 Internal Server Error`**

```json
{
  "error": {
    "code": "INTERNAL_ERROR",
    "message": "Failed to retrieve quote"
  }
}
```

---

## Notes

- Each call independently picks a random quote — the same quote may be returned on consecutive calls.
- There is no endpoint to list all quotes; the collection is intentionally opaque to the client.
