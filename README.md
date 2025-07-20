# AI Travel Itinerary Generator

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Docker Compose](https://img.shields.io/badge/Docker%20Compose-Ready-green.svg)](docker-compose.yml)

A full-stack application that leverages Google's Gemini AI to create personalized travel itineraries. This repository contains both frontend and backend components as Git submodules, with Docker Compose orchestration for seamless development.

## 🌍 Live Deployed App

The full app is deployed and accessible here:

🔗 **Web App URL**:  
[https://ai-travel-itinerary-gen-app-frontend-181459384771.us-central1.run.app](https://ai-travel-itinerary-gen-app-frontend-181459384771.us-central1.run.app)
> This frontend connects to the deployed backend and Gemini AI APIs under the hood — no setup required to test.

📘 Swagger Docs: [Swagger UI](https://ai-travel-itinerary-gen-app-backend-181459384771.us-central1.run.app/docs)
---- 



## 🏗️ Architecture Overview

```
ai-travel-itinerary-app/
├── frontend/          # React SPA (submodule)
├── backend/           # FastAPI REST API (submodule)
├── docker-compose.yml # Local development orchestration
└── .gitmodules        # Submodule configuration
```

## 🚀 Features

- **AI-Powered Itinerary Generation**: Utilizes Google Gemini 2.5 Flash for intelligent travel planning
- **Full Authentication System**: JWT-based auth with secure user management
- **Modern Tech Stack**: React frontend + FastAPI backend
- **Containerized Development**: Docker Compose for one-command setup
- **Production Ready**: Deployed on Google Cloud Platform

## 📋 Prerequisites

- Docker and Docker Compose
- Git (with submodule support)
- Google Cloud Platform account with Vertex AI enabled
- Service account key for Gemini API access

## 🛠️ Quick Start

### 1. Clone with Submodules

```bash
# Clone the repository with all submodules
git clone --recurse-submodules https://github.com/kal997/ai-travel-itinerary-app.git
cd ai-travel-itinerary-app

# If you already cloned without submodules
git submodule update --init --recursive
```

### 2. Configure Environment

```bash
# Backend configuration
cp backend/.env.example backend/.env
# Edit backend/.env with your settings

# Docker environment configuration
cp .env.docker.example .env.docker
# Edit .env.docker and set your GCP credentials path:
# GCP_KEY_PATH=/path/to/your/credentials.json
```

### 3. Start the Application

```bash
# Start all services with environment file
docker-compose --env-file .env.docker up -d

# Or export the variable and run
export GCP_KEY_PATH=/path/to/your/credentials.json
docker-compose up -d

# View logs
docker-compose logs -f

# Stop services
docker-compose down
```

The application will be available at:
- Frontend: http://localhost:3000
- Backend API: http://localhost:8000
- API Documentation: http://localhost:8000/docs

## 🔧 Development Workflow

### Working with Submodules

```bash
# Pull latest changes for all submodules
git submodule update --remote --merge

# Work on a specific submodule
cd backend
git checkout main
git pull origin main
# Make changes, commit, push
cd ..
git add backend
git commit -m "Update backend submodule"
```

### Running Individual Services

```bash
# Run only the database
docker-compose up -d db

# Run backend with database
docker-compose up -d db backend

# Rebuild a specific service
docker-compose build backend
docker-compose up -d backend
```

### Database Management

```bash
# Access PostgreSQL
docker-compose exec db psql -U travel_user -d travel_itinerary

# Run migrations (backend container must be running)
docker-compose exec backend alembic upgrade head

# Create a new migration
docker-compose exec backend alembic revision --autogenerate -m "Description"
```

## 📦 Services Configuration

### PostgreSQL Database
- **Image**: postgres:15
- **Port**: 5432
- **Database**: travel_itinerary
- **User**: travel_user
- **Data Persistence**: Named volume `postgres_data`

### Backend Service
- **Framework**: FastAPI
- **Port**: 8000
- **Hot Reload**: Enabled via volume mount
- **Dependencies**: Waits for healthy database

### Frontend Service
- **Framework**: React (Create React App)
- **Port**: 3000
- **Hot Reload**: Enabled with CHOKIDAR_USEPOLLING
- **API Proxy**: Configured for backend communication

## 🚢 Production Deployment

Each component can be deployed independently:

### Backend Deployment
```bash
cd backend
# See backend/README.md for detailed deployment instructions
gcloud run deploy travel-backend --source .
```

### Frontend Deployment
```bash
cd frontend
# See frontend/README.md for detailed deployment instructions
gcloud run deploy travel-frontend --source .
```

## 📝 Environment Variables

### Docker Environment (.env.docker)
```bash
# Path to your GCP service account key
GCP_KEY_PATH=/home/username/.gcp/vertex-sa.json
```

### Backend (.env)
```env
ENV_STATE=dev
DEV_DATABASE_URL=postgresql://travel_user:travel_password@db:5432/travel_itinerary
DEV_SECRET_KEY=your-secret-key-here
DEV_ALGORITHM=HS256
DEV_GCP_PROJECT_ID=your-gcp-project
DEV_GCP_REGION=us-central1
```

### Frontend
Frontend configuration is handled via build-time variables:
```bash
REACT_APP_API_URL=http://localhost:8000
```

## 🔍 Troubleshooting

### Common Issues

1. **Submodules not initialized**
   ```bash
   git submodule update --init --recursive
   ```

2. **Database connection errors**
   ```bash
   # Ensure database is healthy
   docker-compose ps
   docker-compose logs db
   ```

3. **GCP credentials not found**
   - Verify the service account key path in docker-compose.yml
   - Ensure the file exists and has correct permissions

4. **Port conflicts**
   - Check if ports 3000, 8000, or 5432 are already in use
   - Modify ports in docker-compose.yml if needed

### Useful Commands

```bash
# Clean restart (removes volumes)
docker-compose down -v
docker-compose up -d --build

# View container resource usage
docker stats

# Access backend shell
docker-compose exec backend /bin/bash

# Access frontend shell
docker-compose exec frontend /bin/sh
```

## 🤝 Contributing

1. Fork the repository
2. Clone with submodules
3. Create your feature branch
4. Make changes in the appropriate submodule
5. Test with docker-compose
6. Submit a pull request

### Development Guidelines

- Follow the contributing guidelines in each submodule
- Test integration between services before submitting PRs
- Update documentation for any configuration changes

## 📚 Documentation

- [Backend Documentation](https://github.com/kal997/ai-travel-itinerary-generator-backend)
- [Frontend Documentation](https://github.com/kal997/ai-travel-itinerary-generator-frontend)
- [API Documentation](http://localhost:8000/docs) (when running)

## 🏛️ Project Structure

```
.
├── backend/                 # FastAPI backend (submodule)
│   ├── travelitinerarybackend/
│   ├── tests/
│   ├── alembic/
│   └── requirements.txt
├── frontend/                # React frontend (submodule)
│   ├── src/
│   ├── public/
│   └── package.json
├── docker-compose.yml       # Development orchestration
├── .gitmodules             # Submodule configuration
└── README.md               # This file
```

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- Powered by Google's Gemini AI via Vertex AI
- Built with FastAPI and React
- Containerized with Docker

---

## Technology Stack

- **Frontend**: React 18, Axios, CSS Grid/Flexbox
- **Backend**: FastAPI, SQLAlchemy, Alembic, JWT Auth
- **Database**: PostgreSQL 15
- **AI/ML**: Google Vertex AI (Gemini 2.5 Flash)
- **Infrastructure**: Docker, Docker Compose, Google Cloud Platform
- **Development**: Git Submodules, Hot Reload, Volume Mounts
