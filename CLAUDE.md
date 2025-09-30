# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## About DocsGPT

DocsGPT is an open-source documentation assistant that uses GPT models to help developers find information in project documentation. The application consists of a Flask backend API, React frontend, Chrome extension, and various widgets.

## Architecture Overview

### Core Components
- **Application**: Flask backend API (`/application/`) - Main REST API with document processing, LLM integration, and user management
- **Frontend**: React + Vite application (`/frontend/`) - User interface built with TypeScript, Redux Toolkit, and Tailwind CSS
- **Extensions**: Browser extensions and embeddable widgets (`/extensions/`)
  - Chrome extension (`/extensions/chrome/`)
  - React widget (`/extensions/react-widget/`)
  - Web widget (`/extensions/web-widget/`)
- **Scripts**: Utility scripts for creating similarity search indexes
- **Tests**: Unit tests for backend functionality (`/tests/`)

### Key Technologies
- **Backend**: Flask, Celery (task queue), Redis (cache/broker), MongoDB (database)
- **Frontend**: React 18, TypeScript, Vite, Redux Toolkit, Tailwind CSS
- **AI/ML**: LangChain, sentence-transformers, FAISS, multiple LLM providers (OpenAI, Anthropic, local models)
- **Vector Stores**: FAISS, Elasticsearch, Qdrant, Milvus, LanceDB support
- **Document Processing**: Support for PDF, DOCX, TXT, and other formats

### Configuration
- Backend configuration is centralized in `application/core/settings.py` using Pydantic settings
- Environment variables can be set in `.env` files (see `.env-template`)
- Frontend environment variables prefixed with `VITE_`

## Development Commands

### Environment Setup
```bash
# Quick setup (Mac/Linux)
./setup.sh

# Development environment (MongoDB + Redis only)
docker compose -f docker-compose-dev.yaml up -d

# Full application with Docker
docker compose up
```

### Backend Development
```bash
# Install dependencies
pip install -r application/requirements.txt

# Run Flask application
flask --app application/app.py run --host=0.0.0.0 --port=7091

# Start Celery worker
celery -A application.app.celery worker -l INFO

# Run tests
python -m unittest discover -v
```

### Frontend Development
```bash
# Navigate to frontend directory
cd frontend

# Install dependencies
npm install --include=dev

# Development server
npm run dev

# Build for production
npm run build

# Linting and formatting
npm run lint
npm run lint-fix
npm run format
```

### Global Linting
```bash
# Root level linting (frontend files)
npm run lint  # ESLint
npm run format  # Prettier
```

## Testing

- Backend tests are located in `/tests/` directory
- Run backend tests with: `python -m unittest discover -v`
- Frontend testing setup uses standard React testing patterns
- Tests cover LLM integrations, vector stores, API endpoints, and core functionality

## LLM Integration

The application supports multiple LLM providers configured via `LLM_NAME` environment variable:
- `docsgpt`: Local models (default)
- `openai`: OpenAI GPT models
- `anthropic`: Anthropic Claude models
- Azure OpenAI, SageMaker, and other providers

Vector embeddings use sentence-transformers by default, with support for OpenAI embeddings and other providers.

## Document Processing Pipeline

1. Document upload and parsing (PDF, DOCX, TXT, etc.)
2. Text chunking and preprocessing
3. Vector embedding generation
4. Storage in chosen vector database
5. Retrieval-augmented generation (RAG) for answering queries

## Deployment Options

- **Local Development**: Use `docker-compose-dev.yaml` for dependencies only
- **Full Docker**: Use `docker-compose.yaml` for complete application
- **Production**: Supports various cloud deployments with Azure, AWS configurations