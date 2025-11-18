# API Documentation - Notes & Translation Microservice

## Base URL
```
http://localhost:8000/api/
```

## API Endpoints

### 1. Notes Management

#### Create a Note
**Endpoint:** `POST /api/notes/`

**Request:**
```json
{
  "title": "My First Note",
  "text": "This is the content of my note",
  "language": "en"
}
```

**Response (201 Created):**
```json
{
  "id": 1,
  "title": "My First Note",
  "text": "This is the content of my note",
  "language": "en",
  "created_at": "2025-11-18T20:30:00Z",
  "updated_at": "2025-11-18T20:30:00Z"
}
```

#### List All Notes
**Endpoint:** `GET /api/notes/`

**Query Parameters:**
- `page` (int): Page number (default: 1)
- `page_size` (int): Items per page (default: 20)
- `language` (str): Filter by language
- `search` (str): Search in title/text

**Response (200 OK):**
```json
{
  "count": 42,
  "next": "http://localhost:8000/api/notes/?page=2",
  "previous": null,
  "results": [
    {
      "id": 1,
      "title": "My First Note",
      "text": "This is the content of my note",
      "language": "en",
      "created_at": "2025-11-18T20:30:00Z",
      "updated_at": "2025-11-18T20:30:00Z"
    }
  ]
}
```

#### Retrieve a Single Note
**Endpoint:** `GET /api/notes/{id}/`

**Response (200 OK):**
```json
{
  "id": 1,
  "title": "My First Note",
  "text": "This is the content of my note",
  "language": "en",
  "created_at": "2025-11-18T20:30:00Z",
  "updated_at": "2025-11-18T20:30:00Z"
}
```

#### Update a Note
**Endpoint:** `PUT /api/notes/{id}/`

**Request:**
```json
{
  "title": "Updated Title",
  "text": "Updated content"
}
```

**Response (200 OK):**
```json
{
  "id": 1,
  "title": "Updated Title",
  "text": "Updated content",
  "language": "en",
  "created_at": "2025-11-18T20:30:00Z",
  "updated_at": "2025-11-18T21:00:00Z"
}
```

#### Delete a Note
**Endpoint:** `DELETE /api/notes/{id}/`

**Response (204 No Content)**

### 2. Translation Service

#### Translate a Note
**Endpoint:** `POST /api/translate/`

**Request:**
```json
{
  "note_id": 1,
  "target_language": "hi"
}
```

**Response (200 OK):**
```json
{
  "id": 1,
  "note_id": 1,
  "original_text": "This is the content of my note",
  "translated_text": "यह मेरे नोट की सामग्री है",
  "source_language": "en",
  "target_language": "hi",
  "cached": false,
  "created_at": "2025-11-18T20:35:00Z"
}
```

**Status Codes:**
- `200 OK`: Translation successful
- `404 Not Found`: Note not found
- `400 Bad Request`: Invalid language code
- `500 Internal Server Error`: Translation service error

### 3. Analytics & Statistics

#### Get Statistics
**Endpoint:** `GET /api/stats/`

**Response (200 OK):**
```json
{
  "total_notes": 42,
  "total_translations": 87,
  "avg_note_length": 156,
  "language_breakdown": {
    "en": 30,
    "hi": 12
  },
  "popular_languages": ["en", "hi", "es"],
  "translations_per_language": {
    "en-hi": 25,
    "en-es": 15,
    "en-fr": 10
  }
}
```

## Error Responses

### 400 Bad Request
```json
{
  "error": "Invalid request data",
  "details": {
    "field_name": ["Error message"]
  }
}
```

### 404 Not Found
```json
{
  "error": "Resource not found",
  "message": "Note with id 999 not found"
}
```

### 500 Internal Server Error
```json
{
  "error": "Internal server error",
  "message": "An unexpected error occurred"
}
```

## Supported Languages

- `en`: English
- `hi`: Hindi
- `es`: Spanish
- `fr`: French
- `de`: German
- `pt`: Portuguese
- `ru`: Russian
- `ja`: Japanese
- `zh`: Chinese
- `ar`: Arabic

## Rate Limiting

Not implemented in MVP but recommended for production:
- 1000 requests per hour per IP
- 100 concurrent requests maximum

## Authentication

Currently API is public (no authentication required in MVP).
Production deployment should implement JWT authentication.

## Caching

- Translations cached for 24 hours
- Statistics cached for 1 hour
- Cache is cleared on note updates

## Performance Tips

1. Use pagination for large result sets
2. Cache translations for frequently used language pairs
3. Use filters to reduce result set
4. Consider implementing connection pooling
