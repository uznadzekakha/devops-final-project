DevOps Final Project: Multi‐Service Application Pipeline
Kakha Uznadze

Project Overview
This project demonstrates a complete DevOps pipeline for a multi‐service application:

1. Containerization of Backend (Python Flask) and Frontend (Static HTML served by Nginx).
2. Orchestration using Docker Compose for seamless local development.
3. Monitoring & Visualization with Prometheus, cAdvisor, and Grafana showing CPU & memory metrics.
4. Security Scanning via Trivy to identify high‐severity vulnerabilities in Docker images.
5. Secrets Management using .env files and Docker Compose.
6. Incident Simulation by intentionally killing the backend container and conducting a structured post‐mortem.
   Prerequisites
   • Docker (Desktop or CLI)
   • Docker Compose (if not bundled with Docker)
   • Git for version control

Setup Instructions

1. Clone the Repository
   git clone https://github.com/<your-username>/devops-final-project.git
   cd devops-final-project
2. Environment Setup
   • Create a .env file for the backend (ignored by Git):
   cat << 'EOF' > backend/.env
   SECRET_KEY=super-secret-value
   DATABASE_URL=postgres://user:pass@db:5432/app
   EOF
3. Build & Run with Docker Compose
   docker-compose up --build -d
   • backend: http://localhost:5000/
   • frontend: http://localhost:8080/
   • Prometheus: http://localhost:9090/
   • cAdvisor: http://localhost:8081/
   • Grafana: http://localhost:3000/ (admin/admin)

Service Details
Backend (Flask)
• Language: Python 3.12
• Framework: Flask with a /metrics endpoint via prometheus_client
• Dockerfile: Builds a slim Python image, installs dependencies, and exposes port 5000.
Frontend (Nginx)
• Content: Static HTML (index.html) served by Nginx
• Dockerfile: Copies HTML to Nginx default directory and exposes port 80.
Metrics & Monitoring
• Prometheus: Scrapes cAdvisor every 5s
• cAdvisor: Exposes container CPU & memory metrics (http://localhost:8081)
• Grafana: Visualizes panels for CPU Usage (%) and Memory Usage (bytes)
Security Scanning
• Tool: Trivy (via GHCR image)
• Scans: backend-demo:dev and frontend-demo:dev images
• Reports: Stored in security/trivy-backend-report.txt and security/trivy-frontend-report.txt

Incident Simulation & Post‐Mortem

1. Simulate Outage:

docker-compose kill backend 2. **Observe:** `curl http://localhost:5000/` fails, Prometheus target DOWN. 3. **Recover:**

```bash
docker-compose up -d backend
curl http://localhost:5000/  # OK
4.	Post‐Mortem: See incident-postmortem.md for timeline, impact, root cause, and action items.

Version Control
•	Branch: main holds production-ready state.
•	Commits: Structured with conventional commit messages (feat, chore, docs).


```
