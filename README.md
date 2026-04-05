# 🚀 60-Day DevOps Learning Journey

> **Goal:** Linux → Docker → Kubernetes → CI/CD → Cloud  
> **Start Date:** Day 0  
> **Current Status:** Day 22 complete  
> **Phase:** Phase 1 — Linux/Docker Mastery (Days 1–60)

---

## 📊 Progress

```
Phase 1: Linux/Docker Mastery
█████████░░░░░░░░░░░░░░░░░░░░░░ Day 35/180
```

| Phase | Days | Topics | Status |
|-------|------|---------|--------|
| Phase 1 | 1–14 | Linux Debugging Deep Dive | ✅ Complete |
| Phase 1 | 15–23 | Docker + Git Production | 🔄 In Progress |
| Phase 1 | 24–42 | Advanced Docker + Projects | ⏳ Upcoming |
| Phase 2 | 43–60 | Kubernetes + CI/CD | ⏳ Upcoming |

---

## 📅 Daily Logs

### 🗓️ Day 0 — Setup & Orientation
**Topics:** Environment setup, roadmap planning  
- Set up Ubuntu VM (VirtualBox/WSL2)
- Installed essential tools: git, curl, vim, htop
- Planned 60-day roadmap across Linux → Docker → K8s → CI/CD
- Created this GitHub repo for daily documentation

---

### 🗓️ Day 1 — Ubuntu VM + Core Linux Commands
**Topics:** Linux fundamentals, filesystem navigation  
- Set up Ubuntu VM and got comfortable with the terminal
- Practiced 20 core commands: `ls`, `cd`, `pwd`, `mkdir`, `rm`, `cp`, `mv`, `cat`, `echo`, `grep`, `ps`, `kill`, `chmod`, `chown`, `man`, `which`, `touch`, `head`, `tail`, `wc`
- Learned about absolute vs relative paths
- Explored the Linux filesystem hierarchy (`/etc`, `/var`, `/usr`, `/home`, `/tmp`)

---

### 🗓️ Day 2 — File Mastery
**Topics:** Advanced file operations, disk usage  
- Deep dive into `find` — searched by name, type, size, mtime
- Used `xargs` to pipe find output into other commands
- Disk usage analysis with `du -sh *` and `df -h`
- Learned about inodes — what they are and how inode exhaustion can fill a disk even when space is free
- Practiced: `find / -name "*.log" -mtime +7 | xargs rm -f`

---

### 🗓️ Day 3 — Processes & Signals
**Topics:** Process management, inter-process communication  
- Process inspection: `ps aux`, `top`, `htop`, `pstree`
- Understood process states: Running, Sleeping, Zombie, Stopped
- Signals deep dive: `SIGTERM` (graceful shutdown) vs `SIGKILL` (force kill)
- Practiced: `kill -9 <pid>`, `kill -15 <pid>`, `pkill`, `killall`
- Learned about parent/child process relationships and orphan processes

---

### 🗓️ Day 4 — Bash Variables & Debugging
**Topics:** Bash scripting fundamentals, debugging techniques  
- Variables, arrays, and environment variables (`export`, `env`)
- Writing and calling functions in bash
- Debugging with `set -x` (trace mode) and `set -e` (exit on error)
- Positional parameters (`$1`, `$2`, `$@`, `$#`, `$?`)
- Wrote first real bash script: automated log cleanup

---

### 🗓️ Day 5 — Bash Error Handling & Logrotate
**Topics:** Robust scripting, log management  
- `trap` command — catching signals and EXIT in scripts
- `set -euo pipefail` — the safe bash header
- Writing idempotent scripts
- `logrotate` — configuration, rotation policies, compression
- Configured custom logrotate for `/var/log/myapp/*.log`

---

### 🗓️ Day 6 — Linux Permissions Deep Dive
**Topics:** ACLs, sudoers, file security  
- Standard permissions: `chmod`, `chown`, `chgrp`
- SUID, SGID, and sticky bit
- ACLs with `setfacl` and `getfacl` — fine-grained per-user permissions
- `/etc/sudoers` — safe editing with `visudo`, user privilege escalation
- Practiced creating a restricted user with specific sudo rules

---

### 🗓️ Day 7 — Debug Lab 1: "Permission Denied"
**Topics:** Real-world Linux debugging  
- Simulated a "Permission denied" error in a running service
- Used `strace` to trace system calls and find the exact failing `open()` call
- Traced the issue: wrong ownership on `/var/run/myapp/`
- Fix chain: `strace -e trace=file ./app` → identify path → `chown` → verify
- Key learning: `strace` is the first tool to reach for when logs aren't enough

---

### 🗓️ Day 8 — Memory Management & OOM Killer
**Topics:** Linux memory internals, resource limits  
- Memory analysis: `free -h`, `vmstat`, `slabtop`, `/proc/meminfo`
- Understanding buffers vs cache vs available memory
- OOM (Out of Memory) killer — how Linux decides which process to kill
- OOM score: `/proc/<pid>/oom_score` and `oom_score_adj`
- Simulated OOM scenario and read kernel logs with `dmesg | grep -i oom`

---

### 🗓️ Day 9 — Cgroups & Performance Profiling
**Topics:** Resource isolation, CPU profiling  
- Cgroups v1 vs v2 — hierarchy, controllers, and the unified hierarchy
- Limiting CPU/memory with `systemd-run`: `systemd-run --scope -p MemoryMax=200M ./app`
- `perf top` — real-time CPU profiling by function
- `perf stat` — instruction-level performance counters
- Created a cgroup-constrained service and observed memory throttling

---

### 🗓️ Day 10 — Networking Basics
**Topics:** Linux networking stack, socket inspection  
- IP configuration: `ip addr`, `ip route`, `ip link`
- Socket inspection: `ss -tulnp`, `netstat -tulnp`
- Network interfaces, loopback, and virtual interfaces
- DNS resolution: `dig`, `nslookup`, `/etc/resolv.conf`, `/etc/hosts`
- Firewall basics: `iptables -L`, `ufw status`

---

### 🗓️ Day 11 — tcpdump & Wireshark
**Topics:** Packet capture and network analysis  
- Capturing traffic with `tcpdump`: filter by port, host, protocol
- `tcpdump -i eth0 port 80 -w capture.pcap`
- Opening `.pcap` files in Wireshark, following TCP streams
- Captured a real HTTP API call and read the request/response headers
- Learned about TLS handshake and why HTTPS traffic is encrypted at packet level

---

### 🗓️ Day 12 — Journalctl & Log Parsing
**Topics:** Systemd logging, log analysis  
- `journalctl` — filtering by unit, time, priority, follow mode
- `journalctl -u nginx --since "1 hour ago" -f`
- `/var/log/syslog` and `/var/log/auth.log` structure
- `grep`, `awk`, `sed` pipelines for log parsing
- Wrote a script to extract failed SSH attempts from auth.log and summarize by IP

---

### 🗓️ Day 13 — Debug Lab 2: "Port 80 Bind Fail"
**Topics:** Network debugging, port conflict resolution  
- Simulated a service failing to bind to port 80
- Diagnosed with `ss -tulnp | grep :80` and `netstat -tulnp`
- Found conflicting process, traced with `strace -e trace=network ./app`
- Fix options: kill conflicting process, change port, use `CAP_NET_BIND_SERVICE`
- Key learning: always check what's already listening before debugging the app

---

### 🗓️ Day 14 — Linux Checklist & Cheat Sheet
**Topics:** Review and consolidation  
- Built personal Linux cheat sheet covering Days 1–13
- Created a "Linux Debug Checklist" — ordered steps for common issues
- Review of all major concepts: processes, memory, networking, permissions, logging
- Completed Phase 1 Linux section ✅

---

### 🗓️ Day 15 — Docker Internals & Layer Caching
**Topics:** How Docker works under the hood  
- Docker architecture: daemon, client, containerd, runc
- Image layers — how each `RUN`/`COPY`/`ADD` creates a new layer
- `docker images --digests` — content-addressable storage
- Layer caching: why instruction order matters in Dockerfiles
- Explored image layers with `docker history <image>` and `dive` tool

---

### 🗓️ Day 16 — Multi-Stage Dockerfile Optimization
**Topics:** Production-grade Dockerfile writing  
- Multi-stage builds — builder stage vs final stage
- Reduced a Python app image from ~900MB → under 100MB
- Used `python:3.11-slim` as base instead of full Python image
- `.dockerignore` — what to exclude and why
- Best practices: combine `RUN` commands, clean apt cache, no secrets in layers

---

### 🗓️ Day 17 — Docker Security
**Topics:** Container security hardening  
- Running containers as non-root with `USER` directive
- `seccomp` profiles — restricting syscalls available to containers
- Read-only filesystems: `--read-only` flag
- Dockerfile best practices: no `COPY . .` as root, pin image versions
- `docker scan` and Trivy for vulnerability scanning
- Understanding what containers do and don't isolate (shared kernel)

---

### 🗓️ Day 18 — Docker Volumes & Backup Strategy
**Topics:** Persistent data in Docker  
- Volumes vs bind mounts vs tmpfs — when to use each
- Named volumes: `docker volume create`, `docker volume inspect`
- Bind mounts for development: live code reload
- Backup strategy: `docker run --rm -v mydata:/data -v $(pwd):/backup alpine tar czf /backup/backup.tar.gz /data`
- Volume permissions and ownership issues with bind mounts

---

### 🗓️ Day 19 — Docker Networks & Healthchecks
**Topics:** Container networking, service reliability  
- Network drivers: bridge (default), host, overlay, none
- Custom bridge networks — container DNS by service name
- `docker network create`, `inspect`, `connect`
- Healthchecks in Dockerfile: `HEALTHCHECK CMD curl -f http://localhost/health`
- `docker inspect` to read health status and logs

---

### 🗓️ Day 20 — Docker Compose v2 & Environment Files
**Topics:** Multi-container orchestration  
- `docker compose` (v2) vs `docker-compose` (v1) differences
- `compose.yml` structure: services, networks, volumes
- Environment files: `.env`, `env_file:` directive, variable substitution
- `depends_on` with `condition: service_healthy`
- Compose profiles for dev vs prod configurations

---

### 🗓️ Day 21 — Movie API: FastAPI Skeleton
**Topics:** Building a real project  
- Started the **Movie Sentiment API** project
- FastAPI skeleton: routes, Pydantic models, async handlers
- Project structure: `app/`, `tests/`, `Dockerfile`, `compose.yml`
- Basic endpoints: `GET /health`, `POST /analyze`
- Wrote first pytest tests for the API

---

### 🗓️ Day 22 — Dockerize FastAPI + pytest in Docker
**Topics:** Containerizing a Python app  
- Wrote production Dockerfile for FastAPI app (multi-stage, non-root user)
- Docker build + tag workflow: `docker build -t movie-api:latest .`
- Running pytest inside Docker: `docker run --rm movie-api pytest`
- `docker build --target test` for test-only stage
- Confirmed: build passes, all tests green inside container ✅

---

### 🗓️ Day 23 — Docker Compose + Redis Integration
**Topics:** Multi-container architecture, service networking, caching with Redis  

- Built a multi-container setup using Docker Compose (FastAPI API + Redis)
- Connected API to Redis using service name as hostname (`host="redis"`)
- Implemented Redis caching (cache hit vs cache miss logic)
- Added named volume for Redis to persist data across container restarts
- Used `depends_on` to ensure proper service startup order
- Exposed services using port mapping (`8000`, `6379`)
- Verified setup using `curl` and container logs
- demo work for practical

---

### 🗓️ Day 24 — Docker Compose + Redis Integration
-Learned Git branching strategy (GitFlow) including feature, develop, and main branches for structured development. -Practiced rebase and squash to maintain a clean and linear commit history.
-Built a monitoring setup using FastAPI, Prometheus, and Grafana. Exposed /metrics endpoint and created a custom -counter (analyze_requests_total) to track API usage.
-Configured Prometheus to scrape metrics and connected Grafana for visualization.
-Used queries like rate(...) to monitor real-time API traffic.
-Understood how monitoring helps analyze system performance and behavior in production.

---

### 🗓️ Day 25 — GitHub Repo Setup + .gitignore & Security
**Topics:** Repository hygiene, secret management, security best practices

- Set up GitHub repository with proper structure
- Created `.gitignore` to exclude sensitive files — `.env`, `venv/`, `__pycache__`, `.pyc`
- Understood why secrets should never be committed to public repos (API keys, DB passwords)
- Learned about secret leak risks and how to detect them early
- Best practice: always add `.gitignore` before first commit

---

### 🗓️ Day 26 — GitHub Actions CI Pipeline (lint + test)
**Topics:** CI/CD automation, YAML syntax, automated testing

- Set up GitHub Actions workflow — triggers on every `push` and `pull_request`
- Wrote CI pipeline in YAML: install dependencies → lint → test
- Used `flake8` for linting and `pytest` for running tests automatically
- Integrated Redis as a service inside the CI pipeline using `services:` block
- Handled environment-based config using GitHub Actions `env:` variables
- Debugged real CI errors from the Actions dashboard — red → green pipeline
- Key learning: CI catches bugs before they reach main branch

---

### 🗓️ Day 27 — GitHub Actions: Cache + Matrix Strategy
**Topics:** Dependency caching, matrix builds, parallel testing

- Added `actions/cache@v3` to cache pip dependencies using `requirements.txt` hash as cache key
- Understood cache hit vs miss — cache hit skips `pip install`, saving 30–60s per run
- Set up matrix strategy to test across Python 3.9 and 3.11 in parallel automatically
- Each matrix job runs independently with its own cache — no conflicts between versions
- Fixed a real bug: extra space in `~/ .cache/pip` path was breaking cache lookup
- Debugged pipeline from red → green — both Python versions passing simultaneously
- Key learning: Cache + Matrix = faster, more reliable CI without duplicate job definitions

---

### 🗓️ Day 28 — Secrets Management + Artifacts
Topics: GitHub Secrets, secure CI variables, artifact storage

Learned difference between .env (local) vs GitHub Secrets (CI secure storage)
Added repository secret (TEST_SECRET) and used it safely in workflow via ${{ secrets.* }}
Understood masking (***) in logs — secrets are never exposed in CI output
Fixed real issue: secret not loading due to incorrect setup → debugged using shell check
Learned why .gitignore is not enough (secrets can leak via commit history)
Implemented artifact upload using actions/upload-artifact@v4
Faced real bug: artifact conflict in matrix → fixed using dynamic naming with ${{ matrix.python-version }}
Debugged hidden file issue (.pytest_cache) and learned about include-hidden-files
Final solution: stored test output (test-report.txt) as artifact for reliable debugging
Key learning: Secure pipelines require proper secret handling + artifact tracking for debugging and reuse

---

### 🗓️ Day 29 — Docker Debug Lab: Layers + Optimization
Topics: Docker layer caching, multi-stage builds, optimization, debugging

Deeply understood Docker layers — each instruction creates a cached layer
Learned how layer caching speeds up builds by avoiding re-installing dependencies
Identified bad practice: COPY . . before installing dependencies breaks cache
Optimized Dockerfile by copying requirements.txt first → install → then copy code
Built advanced multi-stage Dockerfile (builder → tester → runner)
Added test stage inside Docker (pytest) to ensure image correctness before runtime
Implemented production best practices: slim image, non-root user, healthcheck
Debugged critical issue: wrong dependency path (/root/.local vs /usr/local)
Understood difference between build-time vs runtime errors in Docker
Learned to use docker history for analyzing layers and image size
Key learning: Proper layer ordering + multi-stage builds = faster, smaller, production-ready images + Caching

---

### 🗓️ Day 30 — CI/CD Pipeline + Docker Hub Integration
Topics: GitHub Actions, CI/CD, Docker build & push, pipeline debugging

Set up GitHub Actions workflow to automate CI/CD pipeline
Understood CI (build + test) vs CD (delivery vs deployment) clearly
Implemented pipeline trigger on push to main branch
Built Docker image inside CI environment using docker build
Learned authentication with Docker Hub using GitHub Secrets
Used secure credentials (DOCKER_USERNAME, DOCKER_PASSWORD) in pipeline
Tagged Docker image properly (username/myapp:latest)
Pushed image to Docker Hub using docker push
Debugged real CI/CD issues (auth errors, token scope, path issues)
Understood difference between local Docker vs CI runner environment
Learned that current pipeline is partial CD (image delivery, not deployment)
Ran pulled image locally using docker run and verified app working
Debugged real runtime issues (port conflicts, Docker engine issues)

Key learning: CI/CD automates build + delivery, but real CD requires server deployment

---

### 🗓️ Day 31 — Image Security: Trivy Scanning + Vulnerability Fix
Topics: Container security, vulnerability scanning, base image optimization

Learned importance of securing Docker images before deployment
Used Trivy to scan Docker images
Identified OS-level and Python package vulnerabilities (HIGH, MEDIUM)
Understood CVE (Common Vulnerabilities & Exposures) concept
Reduced vulnerabilities drastically by switching base image (Debian → Alpine)
Learned impact of base image on size, performance, and security
Handled OS differences (apt vs apk, useradd vs adduser)
Updated system packages to reduce vulnerabilities
Tried upgrading Python dependencies and faced real dependency conflicts
Understood dependency compatibility (FastAPI ↔ Starlette issue)
Learned not to blindly upgrade packages — balance security vs stability
Re-scanned image after fixes and compared results (before vs after)
Accepted that 0 vulnerabilities is not realistic — focus on reducing HIGH/CRITICAL

Key learning: Secure images require scanning, smart base image choice, and careful dependency management

---

### 🗓️ Day 32 — Docker Swarm (Single Node Setup & Basics)
Topics: Container orchestration, Swarm mode, services, replicas, self-healing

Learned what container orchestration means and why it is needed in production (managing multiple containers)
Understood difference between docker run (single container) vs docker service (managed service)
Initialized Docker Swarm on local machine using docker swarm init
Created first Swarm service and deployed container using docker service create
Learned concept of replicas and scaled service using docker service scale
Observed how Swarm maintains desired state vs actual state
Tested self-healing by manually killing containers and seeing automatic replacement
Explored service lifecycle using docker service ps and docker service logs
Understood internal load balancing across multiple replicas
Learned that in Swarm we control services, not individual containers

Key learning:
Docker Swarm ensures high availability by maintaining desired state, automatically replacing failed containers, and distributing load across replicas

---

### 🗓️ Day 33–35 — Production-Ready Movie API Dockerization
Topics: Production Docker image, CI/CD, healthchecks, debugging, deployment

Converted FastAPI Movie API into a production-ready Docker image
Replaced Uvicorn with Gunicorn + Uvicorn workers for production stability
Implemented proper Dockerfile structure with optimized layers and non-root user
Added healthcheck endpoint (/health) and configured Docker HEALTHCHECK
Faced real issue: containers restarting continuously in Swarm
Debugged using logs and identified healthcheck timing issue (too aggressive)
Learned importance of start-period, interval, and retries in healthchecks
Fixed restart loop by tuning healthcheck configuration
Faced Docker Hub push error (denied: access) and fixed authentication issue
Understood difference between local image vs registry image (Docker Hub)
Built CI/CD pipeline using GitHub Actions to automatically:
→ build Docker image
→ login to Docker Hub
→ push image

Tested full pipeline end-to-end (code → build → push → pull → run)
Learned difference between:

docker build vs docker push vs docker pull vs docker run
Understood that push does NOT deploy container — pull + run does

Handled port conflict issue (address already in use) and learned proper port management
Used Swarm for deployment and scaling (multi-replica setup)
Tested self-healing behavior in production scenario
Implemented rolling updates (start-first strategy) for zero downtime

Understood difference between:

manual deployment vs CI/CD automation
single container vs orchestrated system
💣 Key learnings:
Production apps should use Gunicorn, not Uvicorn directly
Healthchecks must be tuned — not too aggressive
Always verify image push before deployment
Swarm maintains system stability using desired state
CI/CD automates build & push, but deployment can still be manual
Never run multiple containers on same port
Docker Hub acts as single source of truth for images

---

### 🗓️ Day 36 — GitHub Pages for API Documentation (Swagger Auto)
Topics: API documentation, OpenAPI schema, Swagger UI, static hosting

Learned how to generate OpenAPI schema automatically from FastAPI application
Understood how Swagger UI works as an interactive API documentation interface
Created static Swagger UI using openapi.json and custom index.html
Configured project structure to serve API docs via static files
Hosted API documentation using GitHub Pages from repository
Understood difference between dynamic docs (/docs in FastAPI) and static hosted docs
Learned importance of API documentation for developer experience and integration
Exposed public documentation URL for external usage without running backend

Key learning: API documentation is critical in production systems, and hosting Swagger UI via GitHub Pages allows easy access and sharing without requiring the backend to be live

---

### 🗓️ Day 37–42 — Milestone Review: Dockerized Movie API + CI Pipeline
Topics: Project consolidation, production readiness, CI/CD improvement, debugging, architecture, interview preparation

Reviewed and deeply understood complete project flow from request handling to deployment
Strengthened understanding of FastAPI application structure, request validation, and response handling
Analyzed Redis caching mechanism for performance optimization and reduced recomputation
Explored Prometheus metrics integration for monitoring API usage and observability
Revisited Docker concepts including image optimization, multi-stage builds, and non-root execution
Understood production setup using Gunicorn with Uvicorn workers for better performance
Improved CI pipeline using GitHub Actions with linting, testing, caching, and artifact storage
Implemented Docker image versioning using Git commit SHA for traceability and rollback
Learned importance of avoiding only “latest” tag and using versioned builds in production
Practiced real-world debugging scenarios including:

Healthcheck failures causing container restart loops
Port conflicts during container startup
Docker image mismatch and stale deployments
Docker Hub authentication issues
CI pipeline failures and log-based debugging

Built strong understanding of Docker Swarm behavior including replicas, scaling, and self-healing
Validated service reliability by simulating failures and observing automatic recovery
Improved project structure, added .dockerignore, and cleaned unnecessary files
Created professional README and documented architecture clearly for interview readiness
Practiced explaining project end-to-end, including backend, DevOps flow, and deployment strategy

Key learning: A production-ready system is not just about building features but ensuring reliability, observability, scalability, and proper CI/CD practices with strong debugging skills

---

**Commands Used:**
```bash
docker-compose up -d --build
docker ps
docker-compose logs -f

curl -X POST "http://localhost:8000/analyze" \
-H "Content-Type: application/json" \
-d '{"text":"Avengers was amazing","movie_name":"Avengers"}'

## 🔜 Coming Up

| Day | Topic |
|-----|-------|
| Day 23 | Compose: API + Redis → `docker compose up` |
| Day 24 | Git branching strategy + conventional commits |
| Day 25 | GitHub Actions: first CI pipeline |

---

## 🛠️ Tools & Stack Used So Far

`Ubuntu` `Bash` `strace` `tcpdump` `Wireshark` `Docker` `Docker Compose` `FastAPI` `pytest` `Python` `Redis` `Git`

---

## 📁 Repo Structure

```
devops-60days/
├── README.md          ← this file (daily logs)
├── logs/              ← individual day markdown files (from Day 23+)
│   └── day23.md
└── projects/
    └── movie-api/     ← FastAPI sentiment project
```

---

*Updated daily. Each day = one commit = one green square. 🟩*