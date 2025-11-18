# AI Notes & Translation Microservice

A scalable Django REST API microservice for uploading, translating, and managing text notes with Redis caching and containerized deployment.

## Project Overview

This microservice enables users to upload text-based notes, automatically translate them into another language, store both versions in a database, and access comprehensive analytics through REST APIs.

## Tech Stack

- **Backend**: Django 4.2+, Django REST Framework, Python 3.9+
- **Database**: PostgreSQL
- **Caching**: Redis
- **Containerization**: Docker & Docker Compose
- **Cloud**: AWS (EC2/EKS)

## Key Features

- Notes CRUD operations (Create, Read, Update, Delete)
- Automatic translation between multiple languages
- Redis caching for performance optimization
- Analytics endpoint with comprehensive statistics
- Docker containerization for easy deployment
- AWS-ready architecture

## API Endpoints

### Notes CRUD
- `POST /api/notes/` - Create note
- `GET /api/notes/` - List all notes
- `GET /api/notes/<id>/` - Get single note
- `PUT /api/notes/<id>/` - Update note
- `DELETE /api/notes/<id>/` - Delete note

### Translation
- `POST /api/translate/` - Translate a note

### Analytics
- `GET /api/stats/` - Get statistics

## Architecture

### Models
- **Note**: Stores original notes (title, text, language, timestamps)
- **Translation**: Stores translated versions (note_id, target_language, translated_text)

### Services
- **TranslationService**: Language detection and translation
- **CacheService**: Redis caching management
- **AnalyticsService**: Statistics computation

## Setup Instructions

### Local Development

```bash
git clone https://github.com/Phani4423/notes-translation-microservice.git
cd notes-translation-microservice
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
python manage.py migrate
python manage.py runserver 0.0.0.0:8000
```

### Docker Setup

```bash
docker-compose up -d
docker-compose exec web python manage.py migrate
```

## Design Decisions

1. **PostgreSQL** - ACID compliance, complex queries support, cost-effective
2. **Redis** - Sub-millisecond responses, 60-80% database load reduction
3. **Django REST Framework** - Rapid API development, excellent documentation
4. **Docker** - Environment consistency, easy scaling, AWS compatibility

## Performance Optimizations

- Database indexes on frequently queried fields
- Translations cached for 24 hours
- Stats cached for 1 hour
- Eager loading with select_related()
- Pagination support (default: 20 items)
- PostgreSQL connection pooling

## License

MIT License
