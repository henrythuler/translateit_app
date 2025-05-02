# TranslateIt App

- [About the Project](#about-the-project)
- [Technologies Used](#technologies-used)
- [Project Setup](#project-setup)
  - [Prerequisites](#prerequisites)
  - [Installation and Setup](#installation-and-setup)
- [Testing the Application](#testing-the-application)
- [Troubleshooting](#troubleshooting)

## About the Project

TranslateIt is an application designed for managing translators and documents, with automatic language locale detection for uploaded documents using OpenAI’s API. Users can upload CSV files containing document details, and the application infers the locale (en-US, es-ES) for documents with missing locale information. The app has a backend API for CRUD operations, a frontend for user interaction, and a PostgreSQL database for persisting data.

## Technologies Used

- **Backend:** Spring Boot 3.4.4 (Java 21), Maven, OpenCSV for CSV parsing, Spring Data JPA, Springdoc OpenAPI (Swagger UI)
- **Frontend:** Vue 3, Node.js 20
- **Database:** PostgreSQL 15
- **AI Integration:** OpenAI API (gpt-3.5-turbo) for locale generation
- **Containerization:** Docker, Docker Compose
- **Repository Structure:** Unified Git repository with submodules (`translate_api` for backend, `translate_frontend` for frontend)

## Project Setup

Follow these steps to set up and run the TranslateIt App locally using Docker Compose.

### Prerequisites

- **Docker:** Install Docker and Docker Compose (Docker Desktop).
- **Git:** Install Git for cloning the repository and submodules.

### Installation and Setup

#### Clone the Repository:

```bash
git clone git@github.com:henrythuler/translateit_app.git
cd translateit_app
```

#### Initialize Submodules

The backend (`translate_api`) and frontend (`translate_frontend`) are Git submodules. Clone them:

```bash
git submodule update --init --recursive
```

#### Verify:

```bash
ls translate_api       # Should show Dockerfile, pom.xml, src/
ls translate_frontend  # Should show Dockerfile, package.json, src/
```

#### Set OpenAI API Key

Export your OpenAI API key in the terminal:

```bash
export OPENAI_API_KEY=sk-your-openai-api-key-here
```

If you don't have an OpenAI API Key, you can use this one for testing:

```
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
```

Verify:

```bash
echo $OPENAI_API_KEY  # Should display your key
```

#### Run Docker Compose

Start the PostgreSQL database, backend, and frontend:

```bash
docker-compose up -d --build
```

Check logs:

```bash
docker-compose logs
```

Expect messages like:

- `translateit_db: “database system is ready to accept connections”`
- `translateit_backend: “Started Application in X seconds”`
- `translateit_frontend: “Serving dist/ at http://0.0.0.0:3000”`

#### Stop the Application (when done):

```bash
docker-compose down
```

## Testing the Application

- **Backend:** Access Swagger UI at [http://localhost:8080/docs/swagger-ui/index.html](http://localhost:8080/docs/swagger-ui/index.html)
- **Frontend:** Open [http://localhost:3000](http://localhost:3000) in a browser.

## Troubleshooting

### Submodule Issues

If `translate_api` or `translate_frontend` is empty:

```bash
git submodule update --init --recursive
```

### OpenAI Errors

If `401 Unauthorized` occurs, verify the API key:

```bash
curl -X POST https://api.openai.com/v1/chat/completions \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "gpt-3.5-turbo", "messages": [{"role": "user", "content": "Hello"}], "max_tokens": 10}'
```
