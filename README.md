# 🚀 60-Day DevOps Learning Journey

> **Goal:** Linux → Docker → Kubernetes → CI/CD → Cloud  
> **Start Date:** Day 0  
> **Current Status:** Day 22 complete  
> **Phase:** Phase 1 — Linux/Docker Mastery (Days 1–60)

---

## 📊 Progress

```
Phase 1: Linux/Docker Mastery
█████████░░░░░░░░░ Day 35/180
```

| Phase | Days | Topics | Status |
|-------|------|---------|--------|
| Phase 1 | 1–14 | Linux Debugging Deep Dive | ✅ Complete |
| Phase 1 | 15–23 | Docker + Git Production | ✅ Complete |
| Phase 1 | 24–42 | Advanced Docker + Projects | ✅ Complete |
| Phase 2 | 43–60 | Kubernetes + CI/CD | ⏳ In progress |

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

### 🗓️ Day 43 — 43: Minikube install + kubectl config
topics: Set up a local Kubernetes cluster using Minikube and learned basic Pod operations with kubectl.

We set up a local Kubernetes cluster using Minikube with the Docker driver.
We understood that Minikube runs a single-node Kubernetes cluster inside a Docker container.
We learned that kubectl is a client tool that communicates with the Kubernetes API Server.
We explored the internal flow: kubectl → API Server → Node → Pod → Container.
We created a Pod using kubectl run and observed its lifecycle states.
We debugged real issues like cluster not running, Docker not started, and API server connection errors.
We analyzed Pod details using kubectl describe and understood restart behavior.


Key Learnings
Kubernetes does not manage containers directly; it manages Pods.
Minikube provides a local environment to simulate a real Kubernetes cluster.
kubectl always interacts with the API Server, not directly with Pods.
A Pod is the smallest deployable unit and can contain one or more containers.
Container restarts can happen due to node issues or crashes, and Kubernetes auto-heals them.
Debugging flow: kubectl get pods → describe → logs.
Infrastructure must be running (Docker, cluster) before using kubectl.

---

### 🗓️ Day 44 — Pods Deep (YAML, Logs, Exec, Events Debug)
Topics: YAML declarative system, kubectl logs, kubectl exec, kubectl describe, events, debugging flow

Understood that YAML defines desired state — Kubernetes ensures that state is always maintained
Learned YAML structure: metadata (Pod identity — name, labels) and spec (desired config — container, image)
Understood that YAML is source of truth, stored in Git, applied via CI/CD
Created a Pod using YAML file and applied it with kubectl apply -f
Learned kubectl logs to check application output and debug startup or runtime issues
Used kubectl logs -f for real-time streaming and --previous for crashed container logs
Used kubectl exec -it pod-name -- /bin/sh to enter running container for deep inspection
Understood exec is for debugging only — not for permanent fixes inside container
Learned kubectl describe pod to see Kubernetes-level events (scheduling, image pull, probe status)
Understood difference between logs (app behavior) and events (system actions)
Practiced full debugging flow: get pods → describe → logs → exec
Understood that these 4 tools solve 70-80% of real production Pod issues

Key learning:
YAML defines the system, Kubernetes maintains it, logs show application behavior, events show system actions, and exec allows deep runtime inspection — together these form the core Pod debugging toolkit in production

---

### Day 45: Deployments + rolling updates
Topics: Deployment management, replicas, self-healing, rolling updates

Created a Deployment using YAML to manage multiple Pods instead of using standalone Pods
Used replicas to maintain desired number of Pods (e.g., always keep 2 Pods running)
Understood that Deployment automatically recreates Pods if they are deleted (self-healing)
Learned how selector and template are used to identify and create Pods
Observed that Pods created by Deployment have dynamic names and are managed automatically
Performed a rolling update by modifying the container image and reapplying the YAML
Verified that Pods were updated one by one without downtime (zero-downtime deployment)
Used kubectl rollout status to track deployment progress
Key learning: Deployment ensures availability and stability, while rolling updates enable safe and seamless updates in production

---

### Day 46: Services (ClusterIP, NodePort, Selectors)

Topics: Service networking, stable IP, load balancing, internal vs external communication

Created a Service using YAML to expose Pods managed by Deployment and enable network access
Understood that Pods have dynamic IPs, so Service provides a stable IP and DNS for communication
Learned that Service uses labels and selectors to identify and connect to the correct Pods
Observed that Service automatically distributes traffic across multiple Pods (basic load balancing)
Explored different types of Services:

ClusterIP → used for internal communication within the cluster
NodePort → used to expose application externally via node IP and port
LoadBalancer → used in cloud environments for external access

Understood the difference between:

port → the port exposed by the Service
targetPort → the port on which the container (application) is running

Learned how Services enable communication between microservices using DNS names (e.g., FastAPI connecting to Redis via service name)
Integrated multiple components (FastAPI + Redis) using Services for internal communication

Key learning:
Service provides a stable networking layer and load balancing mechanism, allowing applications to communicate reliably despite dynamic Pod lifecycle

---

### 🗓️ Day 47 — ConfigMaps & Secrets (Configuration Management)

Topics: Environment variables, ConfigMap, Secret, base64 encoding, runtime config injection

Understood problem of hardcoded configuration (e.g., REDIS_HOST) and why it should be avoided
Learned that configuration should be separated from code using environment variables
Created a ConfigMap to store non-sensitive configuration (e.g., REDIS_HOST=redis)
Injected ConfigMap values into containers using environment variables in Deployment YAML
Understood flow: ConfigMap → Deployment → Pod → Container → Application
Learned that Docker images do not contain environment-specific configuration; config is injected at runtime
Understood that .env files are used only for local development and not in production
Learned about Secrets for storing sensitive data (passwords, API keys)
Understood that Secrets store values in base64 encoded format (not encryption, only encoding)
Injected Secret values into containers similar to ConfigMap using secretKeyRef
Key learning: Applications should be environment-independent, and configuration should be dynamically injected using ConfigMaps and Secrets at runtime

---

### 🗓️ Day 48 — Deploy Movie API to Minikube

Topics: End-to-end deployment, Kubernetes resources, DockerHub integration, service exposure

Deployed Dockerized FastAPI Movie API to Kubernetes cluster using Minikube
Used Docker image from DockerHub in Deployment YAML to run application inside Pods
Created a Deployment for Movie API to manage Pods and ensure availability
Created a NodePort Service to expose the application externally
Verified that Kubernetes automatically pulls image from DockerHub when Pod is created
Created separate Redis Deployment and ClusterIP Service for internal communication
Connected FastAPI with Redis using service name (redis) as hostname inside cluster
Understood complete microservice architecture in Kubernetes (API + Redis)
Started and accessed application using minikube service for external access
Verified application endpoints (/health, /analyze) after deployment
Observed full request flow: User → Service → Pod → FastAPI → Redis → Response
Key learning: Successfully deployed a real-world microservice application on Kubernetes using Deployments, Services, and internal service communication

---


### 🗓️ Day 49 — Persistent Volumes & Claims (Redis Data Persistence)

Topics: Stateful storage, PV, PVC, volume mounts, data persistence

Understood that Pods are ephemeral and data stored inside containers is lost when Pod is deleted
Identified need for persistent storage for stateful services like Redis
Created PersistentVolume (PV) to define storage on the node using hostPath
Created PersistentVolumeClaim (PVC) to request and bind storage from PV
Learned that PVC binds to PV based on storage size, access mode, and storage class
Mounted PVC inside Redis container using volumeMounts (/data)
Connected Redis with persistent storage instead of container memory
Tested persistence by inserting data into Redis, deleting Pod, and verifying data still exists
Observed full flow: Pod → PVC → PV → Node storage
Key learning: PersistentVolumes enable stateful applications in Kubernetes by preserving data beyond Pod lifecycle

---

### 🗓️ Day 50 — Ingress Controller + Host Routing

Topics: Ingress, domain routing, reverse proxy, service exposure

Understood limitations of NodePort (non-user-friendly URLs and manual port usage)
Learned that Ingress provides domain-based routing for services
Enabled Ingress controller in Minikube to handle incoming HTTP traffic
Created Ingress resource to map domain (movie.local) to internal service (movie-service)
Configured host-based routing using rules in Ingress YAML
Updated local hosts file to map domain to localhost
Accessed application using custom domain instead of IP and port
Understood request flow: User → Domain → Ingress → Service → Pod
Key learning: Ingress acts as a reverse proxy and enables production-ready routing using domains

---

### 🗓️ Day 51 — HPA (CPU/Memory Auto-Scaling)
Topics: Horizontal scaling, HPA, metrics-server, resource requests/limits, auto-scaling behavior

Understood the limitation of fixed replicas in Deployment (static scaling problem)
Learned that HPA (Horizontal Pod Autoscaler) automatically scales Pods based on CPU/Memory usage
Understood difference between horizontal scaling (multiple Pods) and vertical scaling (same Pod resources increase)
Enabled Metrics Server in Minikube to provide real-time resource usage data
Added CPU requests and limits in Deployment to allow HPA to calculate scaling decisions
Created HPA using kubectl autoscale with target CPU utilization (e.g., 50%)
Observed HPA status using kubectl get hpa (current vs target CPU usage)
Generated load using a busybox container to simulate real traffic
Verified automatic scaling: Pods increased as CPU usage crossed threshold
Observed max replica limit behavior (scaling stops at defined max limit)
Tested scale down when load was removed (system returned to minimum Pods)
Understood real limitation: HPA only scales Pods, not infrastructure (nodes)
Learned that cluster autoscaling is required when node capacity is exhausted

Key Learning = HPA enables dynamic scaling of applications based on load, but must be combined with proper resource limits and infrastructure scaling (Cluster Autoscaler) for full production readiness.

---

### 🗓️Day 52 — Debugging CrashLoop + CI/CD Pipeline (Docker → Kubernetes)

Topics: Kubernetes debugging, CrashLoopBackOff, CI/CD, DockerHub, deployment automation

Learned how to debug Kubernetes pods using kubectl describe, kubectl logs, and events Understood common causes of CrashLoopBackOff like wrong env variables, dependency failure, and application errors Practiced identifying root cause using logs and fixing configuration issues

Built complete CI/CD pipeline using GitHub Actions for automation Configured pipeline to run linting and pytest with Redis service integration Understood importance of testing stage before deployment to prevent broken releases

Automated Docker image build and push to DockerHub using commit SHA-based versioning Learned why immutable tagging (SHA) is better than latest for traceability and rollback

Deployed updated application to Kubernetes using kubectl set image Understood how Kubernetes automatically pulls images and performs rolling updates Verified deployment using rollout status, pod inspection, and API testing

Explored real production workflow and difference between manual deployment vs GitOps tools like Argo CD

Key learning: A complete DevOps workflow involves automated testing, versioned Docker builds, and controlled Kubernetes deployments with rolling updates and rollback capability

---

### 🗓️Day 53 — Resource Requests & Limits Validation (Performance + Stability)

Topics: Kubernetes resources, CPU/memory management, load testing, metrics validation

Learned concept of resource requests (minimum required) and limits (maximum allowed) in Kubernetes Understood how scheduler uses requests for pod placement and limits enforce resource control

Added CPU and memory configuration in deployment YAML and validated using kubectl describe pod Used kubectl top pods (metrics-server) to monitor real-time CPU and memory usage

Performed load testing using repeated API calls (curl loop) to simulate traffic Observed how CPU usage increases with load while memory remains relatively stable

Identified risk of memory nearing request limits and understood possibility of OOMKilled errors Learned how to adjust resource values based on real usage instead of guessing

Understood production strategy for resource tuning:

requests ≈ average usage
limits > peak usage

Connected resource configuration with HPA behavior (autoscaling depends on requests)

Key learning: Resource tuning should be based on real metrics and load testing, ensuring application stability, efficient scheduling, and prevention of crashes in production systems

---

### 🗓️Day 54 — Namespace Isolation + RBAC (Access Control)

Topics: Namespace, resource isolation, service discovery, RBAC, Role, RoleBinding, ServiceAccount

Understood that Namespaces provide logical isolation of resources within a Kubernetes cluster
Created separate namespace (dev) for environment isolation
Deployed application in dev namespace and verified resource separation from default namespace
Learned that resources like Pods, Services, Deployments are namespace-scoped while Nodes and PV are cluster-level
Understood that Namespace does not provide network isolation by default
Explored service discovery: same namespace → direct service name access
Learned cross-namespace communication requires full DNS (service.namespace.svc.cluster.local)
Identified dependency issue (Redis) due to namespace isolation and understood proper deployment strategy

Learned RBAC (Role-Based Access Control) for managing permissions in Kubernetes
Understood RBAC defines who can perform what actions on which resources
Implemented RBAC using Role and RoleBinding for fine-grained access control
Created Role with read-only permissions (get, list pods)
Created ServiceAccount for local RBAC testing
Configured RoleBinding to assign permissions to ServiceAccount
Verified permissions using kubectl auth can-i with --as flag
Understood RBAC flow: Request → Identity → RoleBinding → Role → Allow/Deny
Learned difference between User (external) vs ServiceAccount (internal) and apiGroup usage
Observed limitation in Minikube where default admin context bypasses RBAC restrictions

Key Learning = Namespaces provide isolation of resources, while RBAC enforces access control. Together they enable secure, multi-environment Kubernetes deployments with proper permission management.

Result = Achieved secure, isolated, and controlled Kubernetes environment with namespace-based separation and RBAC policies

---

### 🗓️ Day 55 — K9s + Debugging + Storage (PVC/PV)

Topics: K9s dashboard, real-time debugging, pod scheduling issues, PVC/PV, storage isolation

Learned how K9s provides a terminal-based dashboard for real-time Kubernetes monitoring and debugging
Explored K9s commands for faster navigation (namespace switch, logs, describe, exec)
Understood limitations of kubectl and benefits of K9s for live debugging

Debugged real production-like issue where Redis pod failed to schedule
Identified error using K9s: PersistentVolumeClaim (redis-pvc) not found
Understood that pod scheduling fails if required storage (PVC) is missing

Learned that PersistentVolumeClaim (PVC) is namespace-scoped and must exist in the same namespace as the pod
Understood difference between PVC (storage request) and PV (actual storage resource)
Explored storage flow: Pod → PVC → PV → mounted storage

Resolved issue by creating PVC in dev namespace and verified binding (Bound state)
Confirmed successful pod scheduling and application recovery

Understood difference between memory (runtime, temporary) and storage (persistent data)
Learned that memory has limits and can cause OOMKilled, while PVC defines fixed storage allocation

Applied production best practices: deploy application, database, and storage within the same namespace for proper isolation

Key Learning = K9s enables fast and efficient debugging, while proper understanding of PVC/PV and namespace-scoped storage is critical to resolve real-world Kubernetes scheduling issues

---

### 🗓️ Day 56 — Velero (Backup & Restore + PVC Data)

Topics: Velero, backup strategy, restore flow, object storage, PVC snapshots, disaster recovery

Learned how Velero is used for backup and restore of Kubernetes clusters and applications
Understood real-world problem of data loss due to cluster failure, accidental deletion, or node crashes
Explored production architecture where Velero runs inside cluster and stores backups in object storage (S3/GCS/Azure)
Learned that backups include both Kubernetes resources (pods, deployments, services, configmaps, secrets) and persistent data (PVC)

Understood complete backup flow: Velero collects cluster resources, takes PVC snapshots, and stores them in object storage
Learned restore flow: Velero recreates resources, reattaches volumes, and restores application with data
Explored different backup types: full cluster backup, namespace-level backup, and scheduled automatic backups
Understood importance of scheduled backups for production systems

Learned that PVC backup is critical because it stores application data (e.g., database), and without it data loss occurs
Explored two PVC backup methods: volume snapshots (cloud disk level) and file-level backup (Restic)
Understood difference between backup (saving state) and restore (recovering state)

Analyzed real production scenario: accidental deletion of production database and recovery using Velero restore
Learned best practices: frequent backups, separate storage location, and periodic restore testing

---

🗓️ Day 57–60 — Phase 1 Complete: Movie API on Kubernetes + Ingress

Topics: Ingress routing, service exposure, domain mapping, debugging, networking limitations

Learned how to expose applications using Ingress instead of NodePort for production-style routing
Converted service type from NodePort to ClusterIP for ingress compatibility
Configured NGINX Ingress with host-based routing (movie.local → movie-service)
Enabled ingress controller in Minikube and validated ingress resource

Configured local domain mapping using /etc/hosts to simulate real DNS routing
Verified request flow: Domain → Ingress → Service → Pods → Application

Debugged service update issue and understood need to recreate service for type changes
Resolved ingress conflict caused by duplicate host/path definitions across namespaces
Fixed DNS resolution issues for custom domain access

Investigated ingress accessibility issue despite correct configuration
Identified root cause as network isolation between WSL and Minikube (docker driver)
Understood difference between configuration issues and environment/network limitations
Used port-forwarding to validate application functionality as a workaround

Learned real-world limitation of local Kubernetes environments and importance of networking understanding

🎯 Key Learning

Ingress routing was correctly configured, but local environment networking limitations prevented external access, highlighting the importance of understanding Kubernetes networking and debugging strategies

---

🚀 Phase 2 Start
🗓️ Day 61 — GitHub Actions Reusable Workflows

Topics: Reusable workflows, workflow_call, inputs, matrix strategy, job dependencies (needs), conditional execution (always/failure/success), CI/CD modular design

Understood the problem of duplication in CI/CD (same pipeline repeated across multiple repositories)

Learned that reusable workflows allow writing CI/CD logic once and reusing it across multiple workflows or repositories

Understood that reusable workflows must be defined inside .github/workflows/ directory

Learned that workflow_call is required to make a workflow reusable (acts as entry point)

Understood that caller workflow uses uses: to trigger reusable workflow execution

Learned that actual execution (jobs + steps + runner) happens inside reusable workflow, not caller

Understood the concept of inputs to make reusable workflows dynamic (e.g., python-version, image-name)

Learned difference between .github/workflows/ (workflows) and .github/actions/ (composite actions)

Understood matrix strategy to run same job across multiple configurations (e.g., Python 3.9 and 3.11)

Learned that matrix helps avoid duplication and improves scalability of pipelines

Understood job dependency using needs to control execution order (test → build → deploy)

Learned that if a job fails, dependent jobs are skipped by default

⚙️ Control Logic (Critical Understanding)

Learned continue-on-error makes a step/job non-blocking (failure ignored, pipeline continues)

Learned if: always() ensures a job/step runs regardless of previous failures (used for logs, cleanup)

Learned if: failure() runs only when previous job fails (used for debugging/log collection)

Learned if: success() runs only when previous jobs succeed (used for deployment)

Understood difference between execution control (if) vs failure handling (continue-on-error)

🧪 Practical Implementation

Converted existing CI pipeline into reusable workflow (reusable-test.yml)

Moved CI logic (cache, lint, test, artifacts) into reusable workflow

Created main workflow (main.yml) to call reusable workflow using uses

Implemented matrix strategy in caller workflow for multiple Python versions

Debugged common error: “workflow not found” due to incorrect path (.github/actions/ vs .github/workflows/)

Verified reusable workflow execution and job structure in GitHub Actions UI

🧠 Pipeline Design Thinking

Understood importance of modular pipelines (separating test, build, scan workflows)

Learned that reusable workflows improve scalability, maintainability, and standardization

Understood difference between development pipeline (flexible, fast feedback) and production pipeline (strict quality gates)

Learned when to allow failures (dev stage) vs enforce strict checks (production)

🔥 Real Production Insights

Reusable workflows are used in centralized DevOps repositories and shared across multiple microservices

CI/CD pipelines are modular and not monolithic (test, build, deploy separated)

Logging and artifacts must be preserved even on failure using if: always()

Deployment should always be controlled using if: success() to prevent unstable releases

🎯 Key Learning

Reusable workflows enable scalable and maintainable CI/CD by centralizing common logic, reducing duplication, and allowing flexible pipeline control using inputs, matrix, dependencies, and conditional execution.

- Understood production mindset: secret scanning → image scanning (Trivy) → 
  commit hash tagging → automatic rollback using `kubectl rollout undo`

# Day 61 Revision (CI/CD Reusable Workflows)

- Revised workflow_call and uses
- Understood reusable workflow execution model
- Reviewed matrix strategy and needs dependency
- Practiced conditional execution (always, failure, success)
- Strengthened CI/CD modular design understanding

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
