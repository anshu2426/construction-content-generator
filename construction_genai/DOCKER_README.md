# Docker Setup for Construction Content Generator

## Prerequisites
- Docker installed on your system
- Google API Key for the LLM engine

## Build Time Note
The first Docker build may take 3-5 minutes due to ML dependencies:
- scikit-learn (~100MB)
- google-genai (~80MB)
- fpdf (~2MB)

Subsequent builds will be much faster (~30 seconds) due to Docker layer caching.

## Quick Start with Docker Compose (Recommended)

1. **Ensure your .env file contains GOOGLE_API_KEY**
   ```bash
   # .env file should have:
   GOOGLE_API_KEY=your_api_key_here
   ```

2. **Build and run the container:**
   ```bash
   docker-compose up --build
   ```

3. **Access the application:**
   Open your browser and go to: http://localhost:8501

## Manual Docker Build

1. **Build the Docker image:**
   ```bash
   docker build -t construction-genai .
   ```

2. **Run the container:**
   ```bash
   docker run -d -p 8501:8501 -e GOOGLE_API_KEY=your_api_key_here construction-genai
   ```

3. **Access the application:**
   Open your browser and go to: http://localhost:8501

## Stopping the Container

With Docker Compose:
```bash
docker-compose down
```

With Docker:
```bash
docker stop <container_id>
```

## Useful Commands

- **View logs:** `docker-compose logs -f` or `docker logs -f <container_id>`
- **Enter container shell:** `docker exec -it <container_id> /bin/bash`
- **Rebuild without cache:** `docker-compose build --no-cache`
