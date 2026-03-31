# 🚀 60-Day DevOps Learning Journey

> **Goal:** Linux → Docker → Kubernetes → CI/CD → Cloud  
> **Start Date:** Day 0  
> **Current Status:** Day 22 complete  
> **Phase:** Phase 1 — Linux/Docker Mastery (Days 1–60)

---

## 📊 Progress

```
Phase 1: Linux/Docker Mastery
████████████░░░░░░░░░░░░░░░░░░░░░░ Day 27/60
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