# Simple Flask App with Redis Counter

A minimal Python Flask web application that uses Redis to track the number of visits to the homepage. This project demonstrates a multi-container setup with Docker Compose: one container for the Flask app and another for Redis.

## Features
- Flask web server serving a simple "Hello World" page with a visit counter.
- Redis as an in-memory key-value store for persisting the counter.
- Dockerized for easy deployment and development.

## Prerequisites
- Docker installed (with Docker Compose).
- Basic familiarity with running commands in a terminal.

## Project Structure

.
├── app.py                 # Flask application code
├── Dockerfile             # Instructions to build the Flask container
├── docker-compose.yml     # Orchestrates the Flask and Redis services
└── requirements.txt       # Python dependencies


## Setup
1. Clone or download this repository (or create the files as shown in the screenshots).
2. Navigate to the project directory:
cd simple-flask-app-that-increments-a-counter
text## Building and Running
1. Build the Docker images:
docker compose build
textThis builds the custom Flask image from the `Dockerfile`.

2. Start the services in the background:
docker compose up -d
text- The `web` service (Flask app) will start after `redis`.
- Access the app at [http://localhost:5000](http://localhost:5000).

3. View logs to monitor:
docker compose logs -f
textPress `Ctrl+C` to stop tailing.

## Usage
- Open [http://localhost:5000](http://localhost:5000) in your browser.
- Each refresh increments the counter stored in Redis.
- The counter persists across restarts unless you remove volumes (see below).

### Development Mode
- The `docker-compose.yml` mounts the current directory into `/app` for live code reloads. Edit `app.py` and refresh the browser to see changes—no rebuild needed.

### Interacting with Redis
- Exec into the Redis container:
docker compose exec redis redis-cli
text- Check the counter:
GET hits
text## Stopping and Cleanup
1. Stop the services:
docker compose down
textThis removes containers and networks but keeps volumes (counter persists).

2. Reset the counter (remove volumes):
docker compose down -v
text3. Remove images (optional):
docker compose down --rmi all
text## Troubleshooting
- **Port conflict on 5000**: Edit `docker-compose.yml` to change the host port (e.g., `"8000:5000"`), then access `http://localhost:8000`.
- **Build errors**: Ensure `Dockerfile` and `requirements.txt` are correct; check Docker logs.
- **Redis connection issues**: Verify `depends_on` in `docker-compose.yml`; services communicate via the default network.
- **Windows/WSL users**: Use Docker Desktop; ensure file paths are correct for volume mounts.

## Extending the Project
- Add more routes in `app.py`.
- Scale the web service: Add `deploy: replicas: 3` under the `web` service in `docker-compose.yml`.
- Use environment variables: Create a `.env` file and reference it (e.g., for Redis password).
- For production: Add health checks, use multi-stage Docker builds, and deploy to a cloud orchestrator like Kubernetes.

## License
This project is open-source and available under the MIT License. Feel free to fork and modify!

---

*Built with ❤️ using Flask, Redis, and Docker. Last updated: November 07, 2025.*