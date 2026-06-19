## 📊 Progress

```
█████████░░░░░ Day 97/180
```

| Phase | Days | Topics | Status |
|-------|------|---------|--------|
| Phase 1 | 1–14 | Linux Debugging Deep Dive | ✅ Complete |
| Phase 1 | 15–23 | Docker + Git Production | ✅ Complete |
| Phase 1 | 24–42 | Advanced Docker + Projects | ✅ Complete |
| Phase 2 | 43–60 | Kubernetes + CI/CD | ✅ Complete |
| Phase 2 | 60–100 | Kubernetes + AWS | In Progress |


---

Phase 1: Linux/Docker Mastery


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
### 🗓️ Day 61 — GitHub Actions Reusable Workflows

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

### 🗓️ Day 62 — Secrets + OIDC (AWS Authentication)

Topics: AWS authentication, access keys vs OIDC, IAM roles, trust policy, OIDC token flow, temporary credentials, CI/CD security
Understood the problem of authenticating GitHub Actions with AWS for deployment and infrastructure access
Learned traditional method using AWS access keys (AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY) and its limitations (long-lived, security risk, manual rotation)
Understood that storing secrets in CI/CD pipelines can lead to leakage and compromise
Learned that OIDC (OpenID Connect) is a modern and secure way to authenticate without storing long-term credentials
Understood that OIDC provides temporary, short-lived credentials instead of permanent keys

🔐 Core Concepts

Understood that GitHub generates an OIDC token during workflow execution as proof of identity
Learned that AWS IAM Role is used to define permissions (what actions are allowed)
Understood that Trust Policy defines who can assume the IAM role (e.g., specific GitHub repo and branch)
Learned that GitHub uses AssumeRoleWithWebIdentity to request access from AWS
Understood that AWS verifies the OIDC token and trust policy before granting access
Learned that temporary credentials are issued by AWS and expire automatically after a short duration

🔁 OIDC Flow (Step-by-step)
GitHub workflow runs → OIDC token generated → request sent to AWS → IAM role assumed → temporary credentials issued → CI/CD task executed → credentials expire

⚙️ GitHub Implementation

Learned that permissions: id-token: write is required to allow OIDC token generation
Used aws-actions/configure-aws-credentials@v4 to configure AWS authentication via OIDC
Understood that no AWS secrets are required when using OIDC

🧠 Key Understanding

Understood that OIDC is used only during CI/CD execution (build/deploy), not during application runtime
Clarified that once the application is deployed, it continues running independently without needing OIDC
Learned that each workflow run generates fresh credentials (no persistent login)

🔥 Security Insights

Understood that OIDC is more secure than access keys due to:

no stored credentials
short-lived access
reduced attack surface

Learned that security depends on proper configuration:
strict trust policy (repo + branch restriction)
least privilege IAM role
protected GitHub branches
Understood that even if credentials are compromised, their short lifespan limits damage

⚠️ Risk Awareness

Learned that if GitHub repository is compromised, attacker can trigger workflows and gain temporary AWS access
Understood that giving excessive permissions (e.g., AdministratorAccess) to IAM role makes system dangerous even with OIDC

🧠 Production Thinking

OIDC is widely used in production as it eliminates secret management and improves security
CI/CD pipelines should use temporary credentials and least privilege access
Authentication should be event-based (per workflow run), not persistent

🎯 Key Learning

OIDC enables secure, scalable, and secret-free authentication between GitHub Actions and AWS by using temporary credentials, IAM roles, and strict trust policies, making it the industry standard for production CI/CD pipelines.

📋 Practical Log
Date: 25 April 2026
Topic: GitHub Actions + OIDC + AWS ECR CI/CD Pipeline

🎯 Goal Achieved
Set up a keyless CI/CD pipeline where GitHub Actions automatically builds and pushes a Docker image to AWS ECR — without any static access keys.

🛠️ What I Did:
1. ECR Repository

Used existing movie-api repository on AWS ECR

2. OIDC Provider

token.actions.githubusercontent.com configured in IAM Identity Providers
Audience: sts.amazonaws.com

3. IAM Role

Created role: github-oidc-role
Trusted entity: Web Identity (GitHub OIDC)
Permission attached: AmazonEC2ContainerRegistryFullAccess
Trust Policy restricted to only your repo's main branch

4. GitHub Actions Workflow

File: .github/workflows/ecr.yml
Trigger: push to main branch
Steps: checkout → AWS login via OIDC → ECR login → Docker build → tag → push


🔐 How Authentication Worked:
Code push to main
      ↓
GitHub generates JWT token
      ↓
AWS verifies token via OIDC provider
      ↓
Trust Policy check passes
      ↓
Temporary credentials issued (1 hour validity)
      ↓
Docker image pushed to ECR ✅

💡 Key Learnings:

OIDC eliminates the need for static AWS access keys in CI/CD
Trust Policy acts as a security gate — only specific repo/branch can assume the role
Temporary credentials expire automatically — more secure than long-lived keys
GitHub Actions id-token: write permission is required for OIDC to work

---

### 🗓️ Day 63 — Helm Chart Creation (Movie API)

Topics: Helm basics, chart structure, values.yaml, templating, multi-resource deployment, environment configs, rollback, production practices

Understood the limitation of static Kubernetes YAML (duplication, manual updates, poor scalability)

Learned that Helm is a Kubernetes package manager used for templating, packaging, and managing deployments

Understood core concept: Helm chart = blueprint, values.yaml = configuration

🧠 Helm Structure

Created Helm chart using:

helm create movie-api

Understood structure:

Chart.yaml (metadata)
values.yaml (central config)
templates/ (dynamic Kubernetes manifests)
⚙️ Implementation (Movie API)

Converted existing Kubernetes YAML into Helm templates:

Deployment → dynamic replicas, image, ports, env
Service → dynamic type and ports
Ingress → dynamic host routing

Added Redis as a dependency:

Redis Deployment + Service added inside Helm chart
Configured via values.yaml (redis.enabled, redis.host)
🔧 values.yaml (Control Center)

Centralized all configs:

replicaCount
image (repo + tag)
ports (service + container)
Redis config
environment variables

Enabled dynamic templating using:

{{ .Values.* }}
🔁 Deployment Operations
Install:
helm install movie-api .
Upgrade:
helm upgrade movie-api .
Rollback:
helm rollback movie-api <revision>
🌍 Environment Management

Used separate values files for environments:

values-dev.yaml
values-staging.yaml
values-prod.yaml

Same chart → different configurations per environment

🔥 Key Benefits
Reduced duplication of YAML files
Centralized configuration
Easy updates via values.yaml
Reusable deployment across services
Built-in rollback support
⚠️ Risks & Safety

Identified risks:

wrong values → system break
shared chart → multi-service impact

Mitigation:

staging environment testing
CI/CD controlled deployment
environment-specific values files
Helm rollback
🧠 Production Thinking

Helm is used as a deployment layer in CI/CD pipelines (not automation tool itself)

Standard production flow:
CI/CD → Helm → Kubernetes

Use of versioned charts, staging validation, and rollback ensures safe deployments

🎯 Key Learning

Helm enables scalable, reusable, and configurable Kubernetes deployments by converting static manifests into dynamic templates controlled via centralized values, making it essential for managing multi-service production systems.


debug: fix image deployment issue in Kubernetes pipeline

- Observed old image running despite successful CI/CD
- Verified pod image using kubectl describe
- Identified ImagePullBackOff with "no basic auth credentials"
- Root cause: Kubernetes (Minikube) could not authenticate with private ECR
- Switched registry from ECR to DockerHub for testing
- Updated CI/CD to push image with SHA tag
- Deployed using Helm with correct image tag override
- Verified new image running successfully in pods

Key Learning:
Image push ≠ deployment, Helm upgrade is required and registry auth is critical

---


### 🗓️ Day 64 — Helm: Linting, Templates, Values & Safe Deployment

Topics: Helm lint, templates, values.yaml, overrides, dry-run, upgrade --install, atomic deployments

🧠 Problem

Managing raw Kubernetes YAML is error-prone and hard to reuse

Manual deployment logic leads to inconsistent and unsafe releases

🔍 What I Learned
🔹 Helm Lint (Validation)

Used:

helm lint .
Validates chart structure
Detects YAML issues
Prevents broken deployments
🔹 Helm Templates (Dynamic Config)

Converted static YAML to dynamic:

replicas: {{ .Values.replicaCount }}
Removed hardcoding
Enabled reusable deployments
🔹 values.yaml (Control Center)

Defined default config:

replicaCount: 2
image:
  repository: rajspy/movie-api
  tag: latest
Central place to control behavior
🔹 Values Override (Runtime Control)

Used CLI override:

--set image.tag=<SHA>
Overrides default values
Used in CI/CD for dynamic deploy
🔹 Priority Rule
--set > values file > default values.yaml
CLI always wins
🔹 Dry Run (Safe Testing)
helm upgrade movie-api . --dry-run
Simulates deployment
Shows final rendered output
🔹 Template Debug
helm template movie-api .
Converts Helm → actual Kubernetes YAML
Helps verify configuration
🔹 Helm Upgrade --install (Automation Concept)
helm upgrade --install movie-api .
Installs if not present
Upgrades if already exists
Removes need for manual checks
🔹 Idempotency Concept

Same command works in all cases:

Safe to run multiple times without breaking system
🔹 Atomic Deployment (Production Safety)
helm upgrade --install movie-api . --atomic --wait
Waits for pods to be ready
If failure → auto rollback
Prevents broken state
⚠️ Failure Scenario (Important)

Without --atomic:

Partial deployment
Some pods crash
System becomes unstable

With --atomic:

Failure detected
Automatic rollback
System remains stable
🔥 Key Learnings
Helm enables reusable and dynamic deployments
values.yaml defines defaults, overrides control runtime
Image push ≠ deployment (Helm upgrade required)
Always validate before deploy (lint + dry-run)
--install makes CI/CD idempotent
--atomic ensures production safety

🧠 Production Thinking
Never deploy blindly
Always verify rendered YAML
Prefer values files for config
Use CLI overrides for dynamic values (like image tags)
Ensure deployments are safe and reversible

🎯 Key Learning

Helm is not just a deployment tool — it is a system to create safe, repeatable, and automated infrastructure deployments.

---

### Day 65: CI → Helm Upgrade (Manual CD on Minikube)

Topics: CI vs CD, image tagging strategy, Helm upgrade, deployment trigger, Minikube limitation

Extended existing GitHub Actions pipeline from CI-only → CD-ready flow
Built and pushed Docker images using commit SHA as immutable tag
Understood that CI success does NOT mean deployment is updated
Identified issue of hardcoded image tag in values.yaml blocking updates
Learned that Kubernetes triggers rollout only when Deployment spec changes
Used Helm upgrade with --set image.tag=<SHA> to pass dynamic image version
Executed manual deployment flow due to Minikube being local (no direct CI access)
Verified deployment using kubectl rollout status and pod inspection
Practiced structured debugging: rollout → pods → describe → logs
Understood failure case where same image tag leads to no rollout (old version runs)

Key learning:
CI builds artifacts, but CD requires explicitly updating the Deployment spec. Using unique image tags (like commit SHA) with Helm ensures Kubernetes triggers a rolling update and runs the correct version in production.

---

### 🚀 Day 66: Multi-Environment Deployment (dev / prod)

Topics: Helm values override, environment isolation, resource management (requests/limits), configuration separation

🧩 What We Did
Created multiple environment configs:
values-dev.yaml
values-prod.yaml
Deployed separate Helm releases:
movie-dev
movie-prod
Used same Helm chart but different configurations per environment
Learned how -f values.yaml overrides default chart values
Configured different replica counts for dev and prod
Managed environment-specific values:
image tag
resource limits
environment variables
Understood that each Helm release creates separate Kubernetes resources
⚙️ Resource Management (IMPORTANT PART)
✅ Requests vs Limits

Configured in values:

resources:
  requests:
    cpu: 100m
    memory: 128Mi
  limits:
    cpu: 200m
    memory: 256Mi
    
🧠 Understanding
Requests → minimum resources required
👉 Scheduler uses this to place pod
Limits → maximum allowed usage
👉 Prevents pod from overusing resources

🔥 Behavior
If CPU exceeds limit → throttled
If memory exceeds limit → pod killed (OOMKilled)
🏢 Environment Difference

Env	Replica	Resources	Purpose
Dev	Low	Low	Testing
Prod	High	Higher	Real users

🧠 Namespace Isolation
Used namespace (movie) for grouping resources
Learned that environments can also be isolated using:
different namespaces
different configs
Prevents interference between environments

🧠 Key Concepts Learned
Same codebase → different behavior via config
Helm enables reusable deployments
Resource control is critical for stability
Proper configuration avoids system crashes

🔥 Key Learning

👉 Kubernetes doesn’t automatically manage resources —
you must define requests & limits to ensure stability and fair usage

---

### 🚀 Day 67: Canary Release (Manual Traffic Split)

Topics: Canary deployment, traffic splitting, Helm releases, debugging real issues

Implemented canary deployment using two Helm releases:
movie-prod → stable version
movie-canary → new version
Used same Service selector (app: movie-api) to route traffic to both versions
Learned that traffic splitting in Kubernetes happens based on pod count (not percentage config)
Verified traffic distribution using multiple curl requests
Understood that canary requires:
Same Service
Same base label (app)
Different replicas
Different image versions

🔧 Debugging & Issues Faced
Fixed Service selector mismatch (Release.Name vs common label)
Resolved ImagePullBackOff due to missing Docker image tag
Understood importance of pushing images before deployment
Fixed Ingress conflicts (duplicate host/path issues)
Handled Helm upgrade errors (invalid selector, webhook issues)
Learned immutable fields in Kubernetes (selector cannot be changed → requires redeploy)
Debugged namespace confusion (default vs movie namespace)

Fixed CI/CD failures:
Test failure due to hardcoded version
Introduced environment variable (APP_VERSION) for dynamic behavior
Resolved lint issues (flake8 errors)

🧪 Validation
Added version differentiation:
prod → "1.0.2"
canary → "canary"
Verified traffic split via repeated requests:
Mixed responses confirmed working canary
Observed approximate traffic ratio based on replica count

🔄 Rollback Strategy
Learned fastest rollback methods:
kubectl delete deployment movie-canary
kubectl scale deployment movie-canary --replicas=0
Understood that canary is temporary and removed after successful rollout

🧠 Key Learnings
Kubernetes routes traffic via Service + labels (not versions)
Docker image represents actual running code (not repo code)
Canary = controlled exposure of new version to limited users
CI/CD + Helm + Kubernetes must work together for production setups
Debugging real issues is part of actual DevOps work

Key learning: Canary deployment allows safe, incremental rollout of new versions with minimal risk, and requires proper labeling, service configuration, and monitoring for validation

---

### 🚀 Day 68: Debug Week — CI Optimization (Timeout + Cache + Matrix)

Topics: GitHub Actions optimization, caching, timeout handling, reusable workflows, CI performance tuning

🧩 What We Did
Worked on optimizing CI pipeline performance in GitHub Actions
Identified slow builds due to repeated dependency installation
Implemented caching for Python dependencies using actions/cache
Used hashFiles('requirements.txt') to intelligently invalidate cache on dependency changes
Created reusable workflow using workflow_call for better modularity
Configured Redis service for integration testing inside CI
Added artifact upload for storing test reports (test-report.txt)
Improved pipeline structure by combining dependency installation steps
⚙️ Performance Optimization

✅ Dependency Caching
key: ${{ runner.os }}-pip-${{ hashFiles('**/requirements.txt') }}
Reuses installed dependencies across runs
Automatically refreshes cache when requirements change
Reduced pipeline execution time significantly (minutes → seconds)

✅ Timeout Handling
timeout-minutes: 10
Prevents CI jobs from running indefinitely
Automatically terminates stuck or hanging jobs
Improves reliability and resource utilization

✅ Reusable Workflow
on:
  workflow_call:
Enabled modular CI design
Allowed reuse across multiple workflows
Reduced duplication and improved maintainability

✅ Test & Lint Optimization
Combined dependency installation for efficiency
Avoided duplicate pytest execution
Used tee to save logs and display output simultaneously

🧪 Validation
Ran pipeline multiple times to verify cache behavior:
First run → slower (cache creation)
Second run → significantly faster (cache hit)
Observed execution time improvement (~minutes → ~30 sec)
Verified cache restoration logs in CI output
Confirmed stable test + lint execution

🧠 Key Learnings
CI optimization is critical for developer productivity
Caching reduces redundant work and speeds up pipelines
Hash-based cache keys ensure correctness and freshness
Timeout acts as a safety mechanism for stuck jobs
Reusable workflows improve scalability and maintainability
CI is not just about running code — it's about efficiency and reliability

🔥 Key Learning

👉 Optimizing CI pipelines improves speed, reduces cost, and enhances developer experience in real-world systems

---

### 🚀 Day 69: Container Security — Trivy Scan + Vulnerability Policy

Topics: container security, image scanning, CVE handling, CI security gates, dependency management

🧩 What We Did
Integrated Trivy into CI pipeline
Scanned Docker images after build & push
Implemented vulnerability policy using severity filters
Used commit SHA for consistent image tagging across build, push, and scan
Debugged multiple real-world issues:
image not found (push vs scan mismatch)
wrong image naming (rajspy vs raj-spy)
severity typo (CRITICAl)
dependency conflicts (FastAPI vs Starlette)
Python version mismatch (3.9 vs 3.11)
Fixed CI/CD flow by aligning build → push → scan sequence
Handled vulnerabilities via upgrade / ignore / policy adjustment

⚙️ Security Implementation

✅ Image Build & Push
docker build -t <user>/movie-api:${{ github.sha }} .
docker push <user>/movie-api:${{ github.sha }}

✅ Trivy Scan Integration

- name: Run Trivy Scan
  uses: aquasecurity/trivy-action@master
  with:
    image-ref: <user>/movie-api:${{ github.sha }}
    severity: CRITICAL,HIGH
    exit-code: 1
  
✅ Vulnerability Policy

CRITICAL → ❌ Block deployment
HIGH → ❌ Block (strict mode)
MEDIUM/LOW → ✔ Allow

🧪 Validation
Verified image availability before scan (fixed manifest errors)
Confirmed Trivy cache working for faster scans
Observed pipeline failure on HIGH vulnerabilities
Fixed severity parsing issues (case-sensitive values)
Ensured consistent image tagging using ${{ github.sha }}
Achieved full pipeline pass after resolving config + flow issues

🧠 Key Learnings
Security scanning is a mandatory gate before deployment
CI/CD flow order matters (build → push → scan)
Docker image must exist before scanning
Dependency upgrades must respect compatibility (FastAPI ↔ Starlette)
Python version affects dependency resolution
CI environment is clean → local environment may cause false conflicts
Even small config errors (typo, naming) can break pipelines

🔥 Key Learning

👉 Secure pipelines prevent vulnerable code from reaching production systems

---

### 🚀 Day 70: CI/CD Observability — Slack Alerts (Deploy / Fail)

Topics: alerting, observability, incident response, team communication, CI feedback loop

🧩 What We Did
Integrated Slack notifications into CI/CD pipeline
Configured Incoming Webhook and stored it securely in GitHub Secrets


Sent automated alerts on:
✅ deployment success
❌ pipeline/build failure
Included contextual info in messages (repo, branch, commit SHA)
Debugged common issues:
curl -x vs curl -X (flag typo)
malformed JSON payload
wrong/missing webhook URL
Verified end-to-end flow: GitHub Actions → Webhook → Slack channel


⚙️ Implementation
🔐 Secret Setup
SLACK_WEBHOOK_URL = https://hooks.slack.com/services/XXXX


✅ Success Alert
- name: Slack Success
  if: success()
  run: |
    curl -X POST -H 'Content-type: application/json' \
    --data "{
      \"text\": \"✅ Deployment Successful 🚀\nRepo: ${{ github.repository }}\nBranch: ${{ github.ref_name }}\nCommit: ${{ github.sha }}\"
    }" \
    ${{ secrets.SLACK_WEBHOOK_URL }}

  
❌ Failure Alert
- name: Slack Failure
  if: failure()
  run: |
    curl -X POST -H 'Content-type: application/json' \
    --data "{
      \"text\": \"❌ Pipeline Failed 💥\nRepo: ${{ github.repository }}\nBranch: ${{ github.ref_name }}\nCommit: ${{ github.sha }}\"
    }" \
    ${{ secrets.SLACK_WEBHOOK_URL }}

  
🧪 Validation
Confirmed messages appear in channel (#all-devops-alerts)
Tested both paths:
success → success alert sent
failure → failure alert sent
Verified notifications on desktop/mobile
Ensured webhook secret is not exposed in repo

🧠 Key Learnings
Alerts are essential for real-time visibility in CI/CD
Email is passive; Slack is instant + team-visible
Notifications enable faster incident response
CI jobs are isolated → alerts must be sent within the same job
Small CLI/YAML mistakes can break pipelines
Observability = knowing system state without manual checks

🔥 Key Learning

👉 “Don’t just deploy—broadcast the outcome to the team instantly.”

---

### 🚀 Day 71: Rollback Strategy Testing

Topics: rollback, failure recovery, deployment safety, production reliability

🧩 What We Did
Learned rollback concept in production systems
Used Helm for version-based rollback
Understood Helm revisions and deployment history
Simulated failure scenario and tested rollback
Verified system recovery after rollback
Explored limitations of rollback (especially DB migrations)
Learned fallback strategies when rollback fails

⚙️ Implementation
🔹 Check history
helm history movie-prod -n movie

🔹 Rollback
helm rollback movie-prod <revision> -n movie

🧪 Validation
Checked pod status after rollback
Verified previous version restored
Ensured application working correctly

🧠 Key Learnings
Rollback is fastest recovery method
Always maintain versioned deployments
Helm makes rollback easy via revisions
Rollback may fail due to config or DB changes
DB migrations should be backward compatible
Always test rollback, not just deployment

---

### ⚙️ Day 72–84: Production-Grade CI/CD & Kubernetes Hardening

🔹 Day 72–73: Multi-Environment Architecture

Objective

Establish strict isolation between development, staging, and production environments.

Implementation
Created separate Helm values files:
values-dev.yaml
values-staging.yaml
values-prod.yaml
Provisioned dedicated Kubernetes namespaces:
dev, staging, prod
Configured environment-specific parameters:
replica counts
resource allocations
environment variables
Key Insight

Environment separation must be enforced at the namespace level, not just by release naming.

🔹 Day 74–75: CI Pipeline Stability
Objective

Achieve a deterministic and reliable CI pipeline with minimal flakiness.

Implementation
Designed a reusable GitHub Actions workflow (workflow_call)
Implemented dependency caching for faster builds
Integrated:
linting (flake8)
testing (pytest)
Added execution timeouts and artifact reporting
Key Insight

A stable CI system should produce consistent, repeatable outcomes without intermittent failures.

🔹 Day 76–77: Continuous Deployment Strategy
Objective

Implement a controlled deployment pipeline with progressive promotion.

Implementation
Automated deployments:
Dev → automatic
Staging → post-CI success
Production deployment gated via manual approval
Standardized deployments using Helm
Deployment Flow
CI → Build → Dev → Staging → Manual Approval → Production
Key Insight

Controlled promotion reduces risk and enforces validation at each stage.

🔹 Day 78–79: Secrets Management
Objective

Eliminate hardcoded credentials and enforce secure secret handling.

Implementation
Migrated all secrets to GitHub Secrets
Configured AWS authentication using OIDC (no static credentials)
Removed exposed AWS keys from repository history
Key Insight

Secrets must never reside in source code; they should be managed via secure external systems.

🔹 Day 80–81: Deployment Safety Mechanisms
Objective

Ensure zero-downtime deployments and application resilience.

Implementation
Health Probes
Readiness Probe → controls traffic routing
Liveness Probe → ensures automatic recovery
Resource Management
Defined CPU and memory requests/limits
Rolling Update Strategy
maxUnavailable: 0
maxSurge: 1
Key Insight

Deployment success is not sufficient — deployments must be safe, observable, and recoverable.

🔹 Day 82–83: Observability
Objective

Enable visibility into system behavior post-deployment.

Implementation
Integrated Slack alerts for pipeline success/failure
Used runtime log inspection (kubectl logs)
Performed active verification of deployed services
Key Insight

Operational visibility is essential; deployment without observability is incomplete.

🔹 Day 84: System Hardening & Failure Testing
Objective

Validate system behavior under failure conditions.

Testing Strategy
Failure Simulation
Deployed invalid image tags to trigger failures
Zero-Downtime Validation
Verified that existing pods continue serving traffic
Rollback Execution
Used Helm revision rollback to restore stable state
helm rollback <release> <revision>
Full Pipeline Validation

End-to-end test:

CI → Build → Push → Deploy → Verify
Key Insight

A system is production-ready only when it can fail safely and recover quickly.

🔹 Additional Enhancements
Container Registry
Migrated from DockerHub to AWS ECR for secure image storage
Security
Integrated vulnerability scanning using Trivy
Enforced failure on critical vulnerabilities
Autoscaling
Implemented Horizontal Pod Autoscaler (HPA)
Enabled dynamic scaling based on CPU utilization

🧠 Final Architecture
Code Commit →
CI Pipeline (Lint + Test) →
Docker Build →
Push to ECR →
Security Scan (Trivy) →
Deploy to Dev →
Deploy to Staging →
Manual Approval →
Deploy to Production →
Autoscaling (HPA) →
Monitoring & Alerts

🏁 Outcome
Multi-environment Kubernetes deployment architecture
Production-grade CI/CD pipeline
Secure authentication and secret management
Zero-downtime deployment strategy
Automated scaling and recovery mechanisms
Verified rollback and failure handling

💬 Professional Summary

Designed and implemented a production-grade CI/CD pipeline with multi-environment Kubernetes deployments using Helm. Integrated AWS ECR for container registry, enforced secure authentication via OIDC, implemented automated vulnerability scanning, enabled zero-downtime deployments with health probes and rolling updates, and incorporated autoscaling with HPA along with robust rollback strategies

---

### Day 85 — AWS Free Tier Setup & Secure Cloud Foundation
AWS Free Tier Setup

Initialized and configured a secure AWS Free Tier environment as the base infrastructure for upcoming cloud-native and DevOps workloads. Focused on building a production-aware setup instead of using default insecure configurations.

Root Account vs IAM Users

Secured the AWS root account with MFA and avoided using it for daily operations. Created a dedicated IAM administrative user for controlled access management, following industry best practices around identity isolation and operational security.

Learned why production systems avoid routine root-account usage and how IAM identities improve auditability, accountability, and controlled infrastructure access.

MFA (Multi-Factor Authentication)

Enabled MFA to strengthen account security and reduce the risk of unauthorized access due to credential compromise or phishing attacks. Understood how layered authentication improves cloud account protection in real-world production environments.

IAM Permissions & Least-Privilege

Explored IAM permission models and the principle of least-privilege access control. Understood how limiting permissions reduces blast radius, prevents accidental infrastructure modifications, and minimizes security exposure during credential leaks or operational mistakes.

Started thinking about cloud access from a production-security perspective rather than only functionality.

AWS CLI Configuration

Configured AWS CLI for programmatic cloud access and verified authenticated identity using:

aws sts get-caller-identity

Learned the importance of verifying:

active IAM identity
AWS account context
region configuration

before performing infrastructure operations or deployments.

Billing Budgets & Cost Alerts

Configured AWS billing budgets and Free Tier alerts to proactively monitor cloud spending and avoid unexpected infrastructure costs.

Implemented:

monthly budget threshold alerts
Free Tier usage notifications
basic cloud cost observability practices

Understood how production teams treat cloud cost monitoring as an operational responsibility alongside system monitoring and security.

Key Production Learning

This session was not only about setting up AWS services, but about understanding:

secure cloud access management
operational accountability
auditability through IAM and CloudTrail concepts
infrastructure cost protection
incident-aware engineering mindset
production-grade cloud security fundamentals

The focus shifted from:

“using AWS services”

to:

“operating cloud infrastructure responsibly and securely.”

What We Learned

Why root account should not be used for daily operations
How IAM controls authentication and authorization
Why least-privilege reduces security risk and blast radius
Why long-lived AWS access keys are dangerous
Why OIDC temporary credentials are safer for CI/CD systems
How CloudTrail helps during security investigations and audits
Why read-only access can still expose sensitive infrastructure information
How billing alerts help prevent unexpected AWS costs
How production engineers think during suspicious infrastructure activity

What We Did

Configured AWS Free Tier account securely
Enabled MFA for account protection
Created IAM admin user for daily access
Configured AWS CLI using aws configure
Verified AWS identity using:
aws sts get-caller-identity
Created billing budget alerts (< $5/month)
Enabled Free Tier usage alerts
Explored CloudTrail auditing concepts
Practiced production-level security and incident-response thinking

Key Learnings

Security is not based on trust — it is based on controlled access
Cloud costs are an operational metric and must be monitored
Auditability is critical during incidents
Temporary credentials reduce credential exposure risk
Least-privilege minimizes blast radius during compromise
Read-only access can still enable reconnaissance attacks
Engineers must always verify identity, region, and environment before deployments
Production Concepts Introduced
Blast Radius
Least-Privilege Access
Authentication vs Authorization
Audit Logging
Incident Investigation
Credential Exposure Risk
Operational Cost Monitoring
Infrastructure Accountability

Final Summary

Day 85 focused on building secure AWS foundations and understanding how real production environments handle access control, auditability, and cloud cost protection. Instead of only learning AWS setup, the session emphasized operational security, IAM best practices, billing observability, and incident-response thinking. The goal was to start developing a production engineering mindset from the first day of AWS learning.

---

### 🗓️ Day 86 — AWS VPC Architecture & Terraform Networking

Topics: AWS VPC, CIDR blocks, public/private subnets, Internet Gateway, Route Tables, Terraform basics, Infrastructure as Code (IaC), configuration drift, Terraform state, production networking

Understood that VPC acts as an isolated private network inside AWS where infrastructure, routing, and security boundaries are controlled

Learned that networking design directly impacts:

security
scalability
connectivity
infrastructure isolation

Understood core concept:

VPC = isolated network
Subnet = smaller network section
Route Table = traffic controller
Internet Gateway = internet access layer

Public vs Private Subnets

Public subnet:

connected to Internet Gateway
internet accessible
used for public-facing services

Private subnet:

no direct internet exposure
used for databases and internal services
reduces attack surface

Understood why production systems isolate sensitive workloads in private networks

⚙️ Terraform Infrastructure Setup

Created Terraform project structure:

main.tf
provider.tf
variables.tf
outputs.tf

Configured AWS provider:

provider "aws" {
  region = "ap-south-1"
}

Infrastructure Provisioned

Created VPC using Terraform:

resource "aws_vpc" "main"

Created:

public subnet
private subnet
Internet Gateway
Route Table
subnet association

Configured public subnet internet routing through:

0.0.0.0/0 → Internet Gateway

🔧 Terraform Workflow

Practiced production Terraform commands:

terraform init
terraform fmt
terraform validate
terraform plan
terraform apply

Understood importance of:

formatting
validation
previewing changes before apply

🔁 Infrastructure as Code (IaC)

Learned Terraform enables:

reproducible infrastructure
version-controlled environments
infrastructure auditability
team collaboration
reduced manual configuration

Understood:

infrastructure should be recreated from code, not manually rebuilt from memory

Configuration Drift

Learned configuration drift happens when:

actual infrastructure
≠
Terraform-defined infrastructure

Example:

manual AWS console changes
undocumented resources
inconsistent environments

Understood Terraform helps maintain infrastructure consistency

Key Benefits

reproducible infrastructure
infrastructure version control
reduced manual setup
easier environment replication
better operational visibility
infrastructure standardization

⚠️ Risks & Failure Scenarios

Identified production risks:

overlapping CIDR ranges
broken route tables
public database exposure
manual console modifications
lost Terraform state file

Understood that route table mistakes can silently break:

connectivity
deployments
internet access

Production Thinking

Networking is not just connectivity — it is security architecture

Public exposure should always be:

minimal
intentional
controlled

Terraform reduces operational risk by making infrastructure:

declarative
reviewable
reproducible

Good infrastructure must be:

understandable by other engineers
recoverable during incidents
auditable through Git history

Core Architecture KnowledgeVPC as a Boundary: Understood that VPC acts as an isolated private network inside AWS where infrastructure, routing, and security boundaries are controlled.IP Planning & CIDR Math:Slash (/) Logic: Learned that /16 locks 2 octets (65,536 IPs) and /24 locks 3 octets (256 IPs).The 5 Reserved IPs: AWS reserves .0, .1, .2, .3, and .255 in every subnet. Actual usable IPs = Total - 5.Networking Impact: Learned that networking design directly impacts Security, Scalability, Connectivity, and Infrastructure Isolation.

⚠️ Risks & Failure ScenariosThe "Destroy" Risk: Accidental terraform destroy can wipe production environments instantly.GitHub Blockers: Learned why .terraform/ (binaries) and *.tfstate must be in .gitignore after hitting GitHub's 100MB file limit.Authorization Errors: Experienced 403 Forbidden errors when IAM users lack ec2:CreateVpc permissions.

Production ThinkingLayered Defense: Successful login (Authentication) does not mean unrestricted access (Authorization).Public Exposure: Should always be minimal, intentional, and controlled.Traceability: If actions are not traceable via CloudTrail or Git, production systems become dangerous.
Key Learning

Terraform and AWS VPC together enable secure, isolated, and reproducible cloud infrastructure by defining networking architecture as code, reducing configuration drift and improving production reliability, scalability, and operational consistency.

---

### 🗓️ Day 87 — EKS Cluster Provisioning with Terraform (Multi-AZ Architecture)

Topics: Amazon EKS, Kubernetes control plane, managed node groups, Terraform EKS module, multi-AZ networking, EKS addons, IAM access mapping, Kubernetes RBAC, cluster validation, high availability

Understood the architecture of Kubernetes control plane components:

API server
scheduler
controller manager
kubelet
reconciliation/self-healing workflow

Learned that Kubernetes is an API-centric distributed orchestration platform where workloads are continuously reconciled against desired state.

EKS Architecture Understanding

Learned that Amazon EKS manages:

Kubernetes control plane
API server availability
etcd management
control plane HA

While engineers remain responsible for:

worker nodes
networking
scaling
IAM
observability
workload reliability

Understood why EKS requires:

subnets across multiple Availability Zones
distributed infrastructure for fault tolerance
resilient cluster networking

Infrastructure as Code (Terraform)

Successfully used Terraform to provision:

AWS VPC
public/private subnet architecture
multi-AZ networking
Internet Gateway
route table associations
EKS cluster
managed node groups

Practiced Terraform workflow:

terraform init
terraform validate
terraform plan
terraform apply

Understood that Terraform enables:

reproducible infrastructure
declarative provisioning
infrastructure auditability
reduced configuration drift

Multi-AZ High Availability

Provisioned EKS worker nodes across multiple Availability Zones in:

ap-south-1

Understood production importance of:

fault tolerance
workload rescheduling
distributed infrastructure resilience

Learned that single-AZ architecture creates:

single point of failure

while multi-AZ architecture improves:

platform availability
recovery capability
workload continuity during datacenter failures

IAM & Kubernetes Access Control

Configured EKS Access Entries to integrate:

AWS IAM identities
with
Kubernetes RBAC authorization

Attached:

AmazonEKSClusterAdminPolicy

to enable:

cluster administration
node visibility
workload management
Kubernetes API access

Understood relationship between:

AWS IAM authentication
Kubernetes RBAC authorization

EKS Addons

Enabled critical cluster addons:

CoreDNS
kube-proxy
VPC CNI

Understood these addons provide:

DNS resolution
pod networking
service communication
Kubernetes traffic routing

Learned that addon failures can silently break:

service discovery
networking
microservice communication

Cluster Validation & Verification

Validated cluster health using:

kubectl get nodes
kubectl get pods -A
kubectl cluster-info

Verified:

control plane Active state
worker nodes Ready state
addon health
Kubernetes API connectivity

Understood:

successful infrastructure provisioning must always be validated operationally.

Production Thinking

Kubernetes reliability depends heavily on:

networking reliability
DNS availability
infrastructure redundancy
scheduler capacity
healthy worker nodes

Learned that:

self-healing only works when healthy infrastructure capacity exists.

Understood that production-grade Kubernetes design requires:

fault tolerance
infrastructure isolation
distributed architecture
declarative infrastructure management

Key Learning

Amazon EKS combined with Terraform enables automated, highly available, and production-ready Kubernetes infrastructure provisioning by integrating distributed networking architecture, managed orchestration services, declarative infrastructure management, and resilient workload scheduling across multiple Availability Zones.

---

### Day 88 — EKS Node Groups (Spot + On-Demand)

Today focused on understanding how production Kubernetes platforms separate workloads based on reliability, scalability, and infrastructure cost using EKS Managed Node Groups. Instead of running all workloads on identical infrastructure, the cluster was designed with separate On-Demand and Spot node groups to simulate real production architecture patterns used in cloud-native environments.

Learned that EKS Node Groups are managed collections of EC2 worker nodes responsible for running Kubernetes workloads. Understood that node groups are not simply compute resources, but infrastructure strategies that help organizations balance workload stability, high availability, and cloud cost optimization. Implemented separate node groups using Terraform with different capacity types and workload labels for infrastructure segmentation.

Configured an On-Demand node group for stable and critical workloads requiring predictable availability, and a Spot node group for interruptible, non-critical workloads optimized for lower infrastructure cost. Understood the operational tradeoff that Spot instances provide significant cost savings but can be reclaimed by AWS at any time, making them unsuitable for critical production systems such as APIs, databases, authentication services, and latency-sensitive workloads.

Worked with Kubernetes scheduling concepts and understood how workload recovery behaves during Spot interruption scenarios. Learned that Kubernetes self-healing depends on healthy backup infrastructure capacity and that pod recovery is possible only when the scheduler finds available nodes with sufficient resources and valid scheduling constraints. Understood that pod scheduling depends not only on available CPU and memory, but also on constraints such as node selectors, taints, tolerations, affinity rules, and infrastructure policies.

During implementation, identified a real-world networking issue where worker nodes placed in private subnets without a NAT Gateway were unable to bootstrap successfully because they could not access required AWS endpoints and container registries. For the current learning environment and cost optimization, the architecture was temporarily adjusted to use public subnets for worker nodes while still maintaining multi-AZ high availability across the Mumbai region.

Validated the cluster using kubectl get nodes, kubectl get pods -A, and node label inspection commands to verify worker node readiness, addon health, and workload infrastructure differentiation. This session significantly improved understanding of how Kubernetes infrastructure design directly impacts workload reliability, fault tolerance, self-healing capability, and production cost management.

Key production learning from today was understanding that Kubernetes scheduling and recovery are infrastructure-dependent processes. Self-healing is not automatic magic — it works only when healthy infrastructure capacity, correct networking, and valid scheduling conditions exist. The session also reinforced the importance of balancing reliability, scalability, fault tolerance, and cloud economics while designing.

---

### Day 89 — Deploy Movie API to EKS via Helm

Topics: Helm deployment workflow, Kubernetes workload lifecycle, EKS workload scheduling, automated CI/CD deployment, immutable container images, rolling updates, readiness/liveness probes, ECR integration, GitHub Actions deployment automation, Kubernetes pod inspection and debugging

Implemented and validated a complete automated deployment workflow for the Movie API on AWS EKS using Helm, GitHub Actions, ECR, and Kubernetes rolling deployments. The session focused on understanding how production Kubernetes platforms deploy and manage workloads through immutable container artifacts and automated delivery pipelines instead of manual deployment operations.

Connected local kubectl configuration to the recreated EKS cluster and verified foundational cluster health before workload deployment. Validated successful operation of CoreDNS, kube-proxy, and the AWS VPC CNI to ensure networking, DNS resolution, service routing, and pod communication were functioning correctly before application rollout.

Reviewed and validated Helm chart configuration for:

replicas
rolling update strategy
readiness and liveness probes
resource requests and limits
service exposure
ECR image configuration

Understood that Helm acts as a deployment management layer that standardizes Kubernetes deployments through reusable templates and centralized configuration management.

Successfully deployed the Movie API workload to EKS using Helm and observed the complete Kubernetes deployment lifecycle:

Helm
↓
API Server
↓
Deployment Controller
↓
ReplicaSet
↓
Scheduler
↓
Worker Nodes
↓
Container Runtime
↓
Readiness Probe Validation
↓
Healthy Running Pods

Observed that pods initially entered:

Running

state before becoming:

Ready

after successful readiness probe validation. This demonstrated how Kubernetes prevents production traffic from reaching unhealthy or partially initialized applications during startup.

Performed deep workload inspection using:

kubectl describe pod
kubectl logs
kubectl get events

Analyzed:

scheduling events
image pull operations
container startup lifecycle
readiness failures
successful recovery behavior
resource allocation
QoS classification

Validated successful ECR image pulls directly through EKS worker node IAM permissions and understood that same-account ECR access often eliminates the need for manual Kubernetes imagePullSecrets configuration.

Integrated the deployment workflow with GitHub Actions CI/CD to fully automate:

testing
Docker image build
vulnerability scanning
immutable image tagging
ECR push
Helm deployment to EKS

Implemented immutable image deployments using:

${{ github.sha }}

instead of mutable:

latest

tags to ensure deployment traceability, rollback capability, deterministic releases, and auditability.

Configured secure AWS authentication using GitHub OIDC-based IAM role assumption instead of long-lived AWS access keys. Understood how temporary credential-based authentication improves production cloud security posture while enabling automated infrastructure and deployment operations.

Validated Kubernetes rolling deployment behavior using:

maxUnavailable: 0
maxSurge: 1

to support near zero-downtime application updates during rollout operations.

Key production understanding from the session was learning that Kubernetes deployment success depends on the combined health of infrastructure, networking, scheduling, image registry access, application startup behavior, and probe validation rather than only successful container execution.

Production learnings to remember:

A container entering Running state does not guarantee application readiness.
Readiness probes protect production traffic from unhealthy applications.
Kubernetes self-healing depends on healthy infrastructure and available capacity.
Immutable image tags are critical for rollback, traceability, and deterministic deployments.
CI/CD pipelines should automate deployment workflows to reduce human error and improve operational consistency.
Infrastructure health should always be verified before workload deployment.
Deployment systems must be observable, recoverable, repeatable, and secure.
Kubernetes scheduling and rollout behavior should always be validated through logs, events, and workload inspection.
Production deployments should avoid long-lived cloud credentials and use temporary identity-based authentication wherever possible.
Healthy infrastructure is only truly validated when real workloads execute successfully on the platform.

---

### Day 90 — Production Ingress Architecture on AWS EKS using ALB, Ingress Controller, and Kubernetes Reconciliation
Overview

Day 90 focused on building and understanding a production-style internet-facing ingress architecture on AWS EKS. The entire topic was intentionally divided into five structured stages to deeply understand Kubernetes networking, ingress abstraction, AWS Load Balancer integration, HTTPS architecture, and controller-based infrastructure reconciliation instead of blindly deploying components without architectural clarity.

The objective of this phase was not only to expose the Movie API publicly, but to understand how modern cloud-native production systems route external internet traffic into Kubernetes workloads securely, reliably, and dynamically.

Stage 1 — Understanding Production Traffic Flow Architecture

The first stage focused on understanding why NodePort-based exposure is not considered production-grade for internet-facing applications.

Initially, the Movie API was accessible through:

Node IP + NodePort

This architecture exposed several production limitations:

Direct worker node exposure increases attack surface.
Infrastructure details such as node IPs and ports become publicly visible.
Node failure can directly impact traffic accessibility.
Scaling infrastructure becomes operationally difficult.
Clients become tightly coupled to infrastructure topology.

Production systems instead rely on stable ingress layers that abstract infrastructure complexity from clients.

The production ingress architecture studied was:

Internet
↓
AWS Application Load Balancer (ALB)
↓
Ingress Rules
↓
Kubernetes Service
↓
Healthy Pods

Key architectural understanding gained:

Clients should never directly depend on pod IPs or worker node addresses.
Pods are ephemeral and infrastructure must remain replaceable.
Stable ingress layers isolate users from backend infrastructure churn.
Kubernetes Services provide internal service abstraction and endpoint discovery.
ALBs provide stable public access, traffic distribution, and backend health validation.

The distinction between Service, Ingress, and ALB was deeply clarified:

Component	Responsibility
Service	Internal pod discovery and load balancing
Ingress	Declarative traffic routing behavior
ALB	Actual internet-facing traffic engine
Stage 2 — AWS Load Balancer Controller and Reconciliation Architecture

The second stage focused on understanding how Kubernetes dynamically provisions AWS infrastructure through controllers.

A major concept learned was:

Ingress itself does NOT create ALB infrastructure.

Ingress resources are only declarative configuration objects stored in the Kubernetes API Server.

Example:

host: api.company.com
path: /
service: movie-api

This defines desired routing behavior, but does not provision infrastructure directly.

The actual infrastructure lifecycle is managed by the:

AWS Load Balancer Controller

The controller continuously watches Kubernetes API changes and reconciles AWS infrastructure accordingly.

Complete reconciliation flow studied:

Ingress YAML Applied
↓
Ingress Stored in API Server
↓
AWS Load Balancer Controller Detects Change
↓
Controller Calls AWS APIs
↓
ALB Provisioned
↓
Listeners Created
↓
Target Groups Attached
↓
Security Groups Configured
↓
Traffic Routing Activated

Deep Kubernetes architecture concepts learned:

Kubernetes is cloud-agnostic by design.
Controllers operationalize declarative desired state.
Kubernetes architecture is fundamentally reconciliation-driven.
Desired state defines system intent, not implementation logic.
Controllers continuously compare desired state with actual infrastructure state.

The AWS Load Balancer Controller itself runs as a Kubernetes workload inside:

kube-system namespace

The controller was installed through Helm and authenticated securely using:

IRSA (IAM Roles for Service Accounts)

instead of static AWS credentials.

This established secure identity-based authentication between Kubernetes workloads and AWS APIs.

Stage 3 — HTTPS, TLS, and ACM Architecture

The third stage focused on production HTTPS architecture and TLS fundamentals.

Core understanding established:

HTTP traffic is plain text and insecure.

Without HTTPS:

credentials can be intercepted
session tokens can be stolen
traffic can be modified during transmission

HTTPS was studied as a transport-layer security mechanism providing:

Encryption
Integrity
Authenticity

TLS certificates were understood as digital trust identities that validate server ownership and establish encrypted communication.

The architecture studied:

Browser
↓ HTTPS
AWS ALB
↓
TLS Handshake + Certificate Validation
↓
Traffic Decryption
↓
Ingress Routing
↓
Kubernetes Services
↓
Pods

AWS Certificate Manager (ACM) concepts learned:

ACM provides free TLS certificates for AWS-integrated services.
Certificates automate expiration and renewal management.
Production systems centralize TLS termination at the load balancer layer.
Pod-level certificate management introduces operational complexity.

A major production concept learned:

TLS termination is commonly centralized at ingress/load-balancer layers.
Stage 4 — Production Ingress Design Patterns

The fourth stage focused on modern ingress design patterns used in production Kubernetes environments.

The complete external request lifecycle studied:

User Browser
↓
DNS Resolution
↓
AWS ALB
↓
HTTPS Validation
↓
Ingress Rules
↓
Kubernetes Service
↓
Endpoints
↓
Healthy Pods

Production routing strategies learned:

Host-Based Routing
api.company.com → API Service
admin.company.com → Admin Service
Path-Based Routing
/api/* → Backend API
/admin/* → Admin Service
/images/* → Media Service

Additional production ingress concepts studied:

Internet-facing vs internal ALBs
Health-check-based traffic routing
Stable ingress endpoints
Service abstraction over pod lifecycle
Security group ingress management
Backend target health validation
Infrastructure abstraction from clients

Critical production understanding gained:

Clients should only depend on stable public domains, never backend infrastructure topology.
Stage 5 — Practical Implementation on AWS EKS

The final stage focused on implementing the complete ingress architecture practically on AWS EKS.

Infrastructure provisioning completed using Terraform:

VPC
Multi-AZ public subnets
EKS control plane
Managed node groups
IAM integration
Kubernetes addons

Movie API deployment was successfully restored using Helm:

helm upgrade --install movie-api .

The AWS Load Balancer Controller installation process included:

Helm repository configuration
IAM policy creation
IRSA setup through eksctl
Controller deployment in kube-system namespace

Ingress resource created:

apiVersion: networking.k8s.io/v1
kind: Ingress

After applying the ingress:

AWS ALB was dynamically provisioned
listeners were created automatically
target groups were configured
pod IPs were registered as healthy targets
ingress routing became publicly accessible

Successfully verified public application access through ALB DNS endpoint:

http://k8s-default-movieapi-00f02c1667-148884491.ap-south-1.elb.amazonaws.com

Successful response received:

{"message":"Movie Sentiment API 🚀 (v3 deployed)","version":"1.0.2","docs":"/docs"}

This validated:

ALB provisioning
ingress reconciliation
Kubernetes service routing
endpoint registration
pod health validation
internet-facing traffic accessibility
Final Production Architecture Achieved
Internet
↓
AWS Application Load Balancer (ALB)
↓
Ingress Rules
↓
Kubernetes Service Abstraction
↓
Healthy EKS Pods
↓
Movie API Application
Key Production Concepts Learned
Kubernetes Architecture
Declarative desired state model
Reconciliation loops
Controller-driven infrastructure automation
Ephemeral compute infrastructure
Service abstraction
Ingress architecture
Infrastructure decoupling
AWS Infrastructure
Application Load Balancer architecture
Target group registration
Security group automation
IRSA authentication model
Public ingress networking
Multi-AZ ingress exposure
Production Networking
Host-based routing
Path-based routing
Health-check-driven traffic management
Stable ingress endpoints
Backend service abstraction
Internet-facing vs internal ingress
Security and HTTPS
TLS architecture
HTTPS termination
ACM certificate management
Identity-based AWS authentication
Infrastructure attack surface reduction
Centralized ingress security
Final Engineering Understanding

The most important architectural understanding from Day 90:

Kubernetes objects define declarative desired behavior, while controllers continuously reconcile actual cloud infrastructure to match that desired state.

And:

Modern cloud-native platforms separate infrastructure intent from infrastructure implementation through controller-driven automation and declarative orchestration.

Day 90 successfully established a production-style ingress foundation on AWS EKS using Kubernetes Ingress, AWS Load Balancer Controller, ALB reconciliation, and internet-facing traffic routing architecture.


Day 90 — Practical Implementation Flow
Objective

Designed and implemented a production-style internet-facing Kubernetes ingress architecture on AWS EKS using:

Kubernetes Ingress
AWS Load Balancer Controller
IRSA (IAM Roles for Service Accounts)
Helm
AWS ALB
Declarative infrastructure reconciliation

The goal of this implementation was to expose the Movie API securely and dynamically through an AWS Application Load Balancer while understanding the complete ingress reconciliation lifecycle used in modern cloud-native production systems.

High-Level Production Architecture
                    ┌────────────────────┐
                    │    Internet User   │
                    └─────────┬──────────┘
                              │
                              ▼
                 ┌─────────────────────────┐
                 │ AWS Application Load    │
                 │ Balancer (ALB)          │
                 │ Internet Facing         │
                 └─────────┬───────────────┘
                           │
                           ▼
              ┌────────────────────────────┐
              │ Kubernetes Ingress Rules   │
              │ Host / Path Based Routing  │
              └──────────┬─────────────────┘
                         │
                         ▼
               ┌─────────────────────────┐
               │ Kubernetes Service      │
               │ Stable Internal Access  │
               └──────────┬──────────────┘
                          │
          ┌───────────────┴────────────────┐
          ▼                                ▼
┌──────────────────┐           ┌──────────────────┐
│ Movie API Pod 1  │           │ Movie API Pod 2  │
│ Healthy Endpoint │           │ Healthy Endpoint │
└──────────────────┘           └──────────────────┘
Kubernetes Reconciliation Flow

The complete ingress provisioning lifecycle was driven through Kubernetes reconciliation loops.

Ingress YAML Applied
        │
        ▼
Stored in Kubernetes API Server
        │
        ▼
AWS Load Balancer Controller Watches Changes
        │
        ▼
Controller Calls AWS APIs
        │
        ▼
ALB Provisioned Automatically
        │
        ▼
Listeners + Target Groups Created
        │
        ▼
Pod IPs Registered as Targets
        │
        ▼
Health Checks Validated
        │
        ▼
Public Traffic Routing Activated
Practical Implementation Steps
Step 1 — Provision AWS EKS Infrastructure

Created:

VPC
Multi-AZ public subnets
EKS control plane
Managed node groups
Networking resources
terraform apply
Step 2 — Configure kubectl Access

Connected local Kubernetes client to EKS cluster.

aws eks update-kubeconfig --region ap-south-1 --name raj-eks-cluster
Step 3 — Verify Cluster Health

Validated successful worker node registration.

kubectl get nodes

Expected:

STATUS = Ready
Step 4 — Deploy Movie API using Helm

Deployed Kubernetes workloads through reusable Helm templates.

helm upgrade --install movie-api .
Step 5 — Verify Application Pods and Services
kubectl get pods
kubectl get svc

Validated:

healthy pods
internal service abstraction
backend readiness
AWS Load Balancer Controller Setup
Step 6 — Verify eksctl Installation
eksctl version
Step 7 — Download Official IAM Policy
curl -O https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/main/docs/install/iam_policy.json
Step 8 — Create IAM Policy
aws iam create-policy \
  --policy-name AWSLoadBalancerControllerIAMPolicy \
  --policy-document file://iam_policy.json

Purpose:

Allow controller-managed ALB lifecycle operations.

Step 9 — Configure IRSA Authentication

Implemented production-grade AWS authentication without static credentials.

eksctl create iamserviceaccount \
  --cluster=raj-eks-cluster \
  --namespace=kube-system \
  --name=aws-load-balancer-controller \
  --role-name AmazonEKSLoadBalancerControllerRole \
  --attach-policy-arn=arn:aws:iam::257394469486:policy/AWSLoadBalancerControllerIAMPolicy \
  --approve
Secure Authentication Flow
Controller Pod
      │
      ▼
Kubernetes Service Account
      │
      ▼
IAM Role (IRSA)
      │
      ▼
Temporary AWS Credentials
      │
      ▼
AWS API Access
Controller Installation
Step 10 — Add AWS Helm Repository
helm repo add eks https://aws.github.io/eks-charts
helm repo update
Step 11 — Install AWS Load Balancer Controller
helm install aws-load-balancer-controller eks/aws-load-balancer-controller \
  -n kube-system \
  --set clusterName=raj-eks-cluster \
  --set serviceAccount.create=false \
  --set serviceAccount.name=aws-load-balancer-controller \
  --set region=ap-south-1 \
  --set vpcId=$(aws eks describe-cluster --name raj-eks-cluster --query "cluster.resourcesVpcConfig.vpcId" --output text)

Validated controller health:

kubectl get pods -n kube-system
Ingress Implementation
Step 12 — Create Ingress Resource

Configured:

internet-facing ALB
ALB ingress class
IP target mode
backend service routing
kubectl apply -f ingress.yaml
Public Traffic Flow
Internet Request
        │
        ▼
AWS ALB
        │
        ▼
Ingress Rules
        │
        ▼
Kubernetes Service
        │
        ▼
Healthy Movie API Pods
ALB Provisioning Verification
Step 13 — Validate Ingress
kubectl get ingress
kubectl describe ingress
Public Application Access

Successfully exposed Movie API publicly through AWS ALB.

http://<ALB-DNS>

Response:

{
  "message": "Movie Sentiment API 🚀 (v3 deployed)",
  "version": "1.0.2",
  "docs": "/docs"
}

Key Production Concepts Learned

Kubernetes Architecture
Declarative desired state
Reconciliation loops
Controller-driven automation
Service abstraction
Ephemeral workloads
AWS Infrastructure
Application Load Balancer provisioning
Target group registration
IRSA authentication
Dynamic infrastructure reconciliation
Production Networking
Internet-facing ingress architecture
Host/path-based routing
Stable public ingress endpoints
Backend health validation
Infrastructure abstraction
Security
Identity-based AWS authentication
Elimination of static AWS credentials
Centralized ingress management
TLS termination architecture understanding


Final Engineering Understanding

Kubernetes objects define desired infrastructure behavior, while controllers continuously reconcile actual cloud infrastructure to match that declarative state.

Modern cloud-native systems rely on controller-driven reconciliation to automate infrastructure lifecycle management dynamically and reliably.

---

### Day 91: Route53 + custom domain (free tier)

TOPIC
Route53 + Custom Domain + Production DNS Architecture

WHAT I LEARNED TODAY

DNS Fundamentals

DNS converts domain names into IP addresses

Browsers use DNS lookup to locate infrastructure
DNS is global internet routing infrastructure

Core DNS Concepts

Domain
Hosted Zone
A Record
AAAA Record
CNAME Record
MX Record
TXT Record
TTL
DNS Propagation
Subdomains


Route53 Concepts
AWS managed DNS service
DNS traffic routing
Health checks
Failover routing
Weighted routing
Latency-based routing
Multi-region traffic management

Production Infrastructure Concepts

HTTPS + SSL/TLS
Secure encrypted communication
SSL certificates
Secure API traffic
Browser trust validation
Load Balancer

Traffic distribution
Horizontal scaling
High availability
Fault tolerance
Rolling deployments
CDN

Global edge caching
Lower latency
Faster static asset delivery
Reduced backend load

Failover + Multi-Region
Disaster recovery
Automatic traffic rerouting
Regional redundancy
Global application resilience

WAF
Protection against malicious traffic
SQL injection filtering
XSS protection
DDoS mitigation concepts

Reverse Proxy
Nginx reverse proxy architecture
SSL termination
Path-based routing
Security headers
Request forwarding

Redis Caching
In-memory caching
Faster API responses
Reduced database load
Better scalability

Canary Deployment
Gradual rollout strategy
Traffic shifting
Safe deployments
Rollback strategy

PRACTICAL COMPLETED

Free DNS Practice Using DuckDNS
Created free custom domain
Mapped DuckDNS domain to AWS EC2 public IP
Accessed deployed application using domain

PRACTICAL FLOW IMPLEMENTED

DuckDNS Domain
↓
DNS Resolution
↓
AWS EC2 Public IP
↓
Docker Containers
↓
Frontend + Backend Application


# REAL PRODUCTION ARCHITECTURE UNDERSTOOD

User
↓
DNS (Route53)
↓
CDN
↓
WAF
↓
Load Balancer
↓
Reverse Proxy
↓
Containers / Kubernetes
↓
Redis Cache
↓
Database


MOST IMPORTANT UNDERSTANDING FROM TODAY

# DNS is not just domain mapping.
# DNS is internet-scale traffic routing infrastructure.
ENGINEERING MINDSET LEARNED

Infrastructure is designed around:
- scalability
- security
- availability
- latency reduction
- fault tolerance
- deployment safety
- 
STATUS
Day 91 Successfully Completed

---

# Day 92 — Deep Dive into Cost Optimization, Spot Infrastructure & Stateless Architecture

> Duration: ~3 Days of Deep Learning & Architecture Understanding

This was not just a simple "topic learning" day.

Over the last few days, I deeply studied how modern cloud-native systems are designed to survive infrastructure failures while remaining scalable, highly available, and cost-efficient.

The primary focus of this learning phase was understanding:

* Why cloud infrastructure must be disposable
* Why modern systems are designed as Stateless
* How Spot infrastructure works internally
* How enterprises reduce cloud costs at scale
* How Kubernetes survives node failures
* How production systems are architected around failure tolerance
* How senior cloud engineers think about cost, scalability, resiliency, and efficiency together

---

# The Biggest Mindset Shift

One of the most important engineering principles I learned is:

```text
Compute should be temporary.
Data should be persistent.
```

Modern cloud systems are designed with the assumption that:

* servers can die anytime
* nodes can terminate anytime
* pods can restart anytime
* regions can fail anytime

Applications must survive infrastructure replacement automatically.

This is one of the foundational principles of Cloud-Native Architecture.

---

# From "Pets" to "Cattle" Infrastructure

I learned the famous cloud engineering mindset:

## Pets Infrastructure

Traditional systems treated servers like pets:

* manually configured
* long-lived
* individually maintained
* difficult to replace

Example:

```text
server-prod-final-v3
```

If this machine died:

* engineers manually repaired it
* applications heavily depended on it

This model does not scale well in modern cloud systems.

---

## Cattle Infrastructure

Modern cloud-native systems treat infrastructure like cattle:

* anonymous
* temporary
* replaceable
* automatically recreated
* disposable

Example:

```text
pod-84x71
node-27ab
```

If infrastructure dies:

* orchestration systems recreate it automatically
* traffic shifts elsewhere
* applications continue running

This was one of the biggest conceptual shifts I understood.

---

# Understanding Stateless vs Stateful Systems

This was one of the deepest concepts I studied.

Initially, I thought Stateless meant:

```text
“system has no data”
```

But that is incorrect.

The real meaning is:

```text
Application servers do not permanently own critical business state.
```

---

# Stateless Architecture

In stateless systems:

* application containers process requests
* but important data is stored externally

Examples:

* PostgreSQL / RDS for persistent data
* Redis for sessions and cache
* S3 for uploaded files
* Queues/Kafka for async jobs

This means infrastructure becomes replaceable.

---

# Stateless Production Flow

```text
User Request
      ↓
Load Balancer
      ↓
Stateless API Container
      ↓
Database / Redis / S3
```

If a server dies:

```text
Spot Instance Terminates
            ↓
Container Dies
            ↓
New Container Launches
            ↓
Reconnects to Same DB / Redis / S3
            ↓
Application Continues Running
```

No critical data is lost because the compute layer never permanently stored the state locally.

---

# Stateful Architecture Problems

I also studied why stateful infrastructure becomes dangerous.

Example bad design:

```text
User uploads file
        ↓
Stored only on EC2 local disk
        ↓
Spot Instance terminates
        ↓
File permanently lost
```

Or:

```text
User session stored only in server RAM
        ↓
Server crashes
        ↓
All users logged out
```

This creates:

* downtime
* data loss
* scaling limitations
* recovery complexity

---

# Understanding AWS Spot Instances

I then studied AWS Spot Infrastructure deeply.

Spot Instances are:

* unused AWS cloud capacity
* sold at very low cost
* but interruptible anytime

AWS can reclaim the infrastructure whenever higher-priority customers need capacity.

This allows:

* up to 90% compute cost reduction
* extremely cheap scalable compute

---

# Spot Infrastructure Internal Flow

```text
AWS Spare Capacity
        ↓
Spot Instance Allocated
        ↓
Application Running
        ↓
AWS Needs Capacity Back
        ↓
2-Minute Interruption Notice Sent
        ↓
Graceful Shutdown Triggered
        ↓
Spot Instance Terminated
```

This taught me that production systems must always be designed assuming infrastructure failure is normal.

---

# Graceful Shutdown Architecture

I studied how production systems safely handle Spot interruptions.

Instead of instantly killing applications, production systems perform:

* connection draining
* request completion
* log flushing
* metrics flushing
* DB connection cleanup
* workload checkpointing

---

# Graceful Shutdown Flow

```text
AWS Spot Interruption Notice
              ↓
Application Detects Metadata Signal
              ↓
Load Balancer Stops New Traffic
              ↓
Active Requests Finish
              ↓
Logs + Metrics Flushed
              ↓
Safe Shutdown
              ↓
Replacement Infrastructure Launches
```

This is one of the most important reliability engineering concepts I learned.

---

# Kubernetes Spot Node Architecture

I also connected this concept with Kubernetes.

I learned that:

* Spot Nodes are actually Spot EC2 instances underneath
* Kubernetes abstracts infrastructure instability
* Pods can automatically move between nodes

---

# Kubernetes Spot Recovery Flow

```text
Spot Node Terminated
          ↓
Node Cordoned
          ↓
Node Drained
          ↓
Pods Evicted
          ↓
Pods Rescheduled on Healthy Nodes
          ↓
Cluster Survives
```

This helped me understand why Kubernetes is extremely powerful for large-scale distributed systems.

---

# Enterprise Spot Strategy

I learned that real companies never run 100% Spot infrastructure.

Instead they use hybrid architectures:

| Infrastructure Type | Purpose                      |
| ------------------- | ---------------------------- |
| On-Demand Nodes     | Critical core services       |
| Spot Nodes          | Scalable workloads           |
| Reserved Capacity   | Predictable baseline traffic |

This balances:

* cost
* reliability
* scalability
* resiliency

---

# Cost Optimization Engineering

This learning phase also introduced me to real cloud cost debugging practices.

I learned that cloud engineering is not only about scalability.

It is also about:

* eliminating waste
* maximizing utilization
* reducing idle infrastructure
* balancing reliability with efficiency

---

# Common Cloud Cost Problems I Studied

| Problem             | Impact                 |
| ------------------- | ---------------------- |
| Idle EC2 instances  | wasted compute cost    |
| Unused EBS volumes  | hidden storage charges |
| Oversized databases | overprovisioning       |
| No caching          | unnecessary DB load    |
| Low CPU utilization | poor efficiency        |
| No autoscaling      | infrastructure waste   |

---

# Hidden EBS Cost Debugging

One important lesson:

```text
DeleteOnTermination = true
```

If enabled:

* EBS volumes delete automatically when EC2 terminates

If not:

* unused storage continues generating cost silently

This is a common real-world cloud waste issue.

---

# Redis Caching & Cost Reduction

I also studied how caching directly reduces infrastructure cost.

Instead of:

```text
scaling database vertically forever
```

production systems often:

* deploy Redis cache
* absorb repeated reads
* reduce DB pressure
* improve latency
* reduce cloud bills

---

# Redis Cache Architecture

```text
User Request
      ↓
Redis Cache
   ↙       ↘
Cache Hit   Cache Miss
   ↓            ↓
Fast Response  Database Query
                    ↓
              Cache Updated
```

This showed me how performance optimization and cost optimization are often connected.

---

# Infrastructure Efficiency Thinking

I learned that senior cloud engineers focus heavily on infrastructure utilization.

Ideal average CPU utilization:

* 60%–80%

Too low:

* wasted money
* idle resources

Too high:

* instability
* scaling bottlenecks
* crash risk

Efficient systems are balanced systems.

---

# Most Important Engineering Understanding From This Learning Phase

This entire learning phase fundamentally changed how I think about infrastructure.

Modern cloud systems are designed with these assumptions:

* servers are temporary
* infrastructure can fail anytime
* workloads must move automatically
* data must survive machine failure
* scaling must be automated
* cost must always be optimized

This is the foundation of resilient, cloud-native distributed systems.

---

# Day 93 — Cluster Autoscaler + Karpenter Basics

> Deep Dive into Elastic Kubernetes Infrastructure, Intelligent Node Provisioning & Production Autoscaling

Today’s learning was not simply about scaling Kubernetes.

The real objective was understanding:

* how production infrastructure automatically reacts to workload demand
* how Kubernetes scales beyond just pods
* how cloud-native systems dynamically provision servers
* how companies optimize infrastructure cost while maintaining reliability
* how autoscaling failures create real production incidents
* why Karpenter is changing modern Kubernetes infrastructure engineering

This learning phase fundamentally shifted my understanding from:

```text
“How many servers should I launch?”
```

to:

```text
“How should infrastructure respond automatically under unpredictable traffic?”
```

That mindset transition is one of the foundations of Platform Engineering and Cloud-Native Infrastructure Design.

---

# The Biggest Realization

Kubernetes scaling is not one thing.

There are actually TWO completely different scaling layers:

| Scaling Type                    | What Scales          |
| ------------------------------- | -------------------- |
| HPA (Horizontal Pod Autoscaler) | Pods / Containers    |
| Cluster Autoscaler / Karpenter  | Infrastructure Nodes |

This distinction is extremely important because many beginners incorrectly assume that HPA alone solves scalability.

It does not.

---

# Understanding the Core Scaling Problem

I first studied what actually happens during a production traffic spike.

## Example Scenario

```text
100 users
↓
1 node works fine
```

Then suddenly:

```text
100,000 users arrive
```

Now:

* CPU spikes
* memory becomes exhausted
* request latency increases
* pods begin crashing
* users experience downtime

Traditional infrastructure required engineers to:

* manually launch EC2 instances
* configure servers
* deploy applications

This approach does not work for modern elastic cloud systems.

---

# The Cloud-Native Autoscaling Model

Modern infrastructure behaves dynamically.

Instead of manually provisioning infrastructure:

```text
Traffic Increases
        ↓
More Pods Needed
        ↓
More Nodes Needed
        ↓
Infrastructure Automatically Expands
```

This is one of the biggest advantages of Kubernetes-based cloud platforms.

---

# Horizontal Pod Autoscaler (HPA)

I studied how HPA works internally.

HPA is responsible for scaling:

* pods
* containers
* application replicas

based on metrics like:

* CPU utilization
* memory utilization
* custom metrics

---

# Internal HPA Flow

```text
Traffic Spike
        ↓
CPU Usage Increases
        ↓
HPA Detects Threshold Breach
        ↓
Replica Count Increased
        ↓
More Pods Created
```

However, I also learned the critical limitation of HPA:

HPA only creates pods.

It does NOT create infrastructure capacity.

---

# The “Pending Pods” Problem

This was one of the most important production concepts I learned.

If the cluster lacks enough CPU or RAM:

```text
HPA Creates New Pods
          ↓
No Node Capacity Available
          ↓
Pods Stay Pending
          ↓
Traffic Cannot Be Served
```

This means autoscaling at the application layer alone is insufficient.

Infrastructure itself must also scale.

This is exactly why:

* Cluster Autoscaler
* Karpenter

exist.

---

# Cluster Autoscaler

I learned that Cluster Autoscaler watches for:

```text
Unschedulable / Pending Pods
```

When Kubernetes cannot place pods onto existing nodes:

```text
Pending Pod Detected
          ↓
Cluster Autoscaler Detects Capacity Shortage
          ↓
Cloud Provider API Called
          ↓
New EC2 Node Launched
          ↓
Node Joins Cluster
          ↓
Pending Pods Scheduled
```

This is automatic infrastructure provisioning.

---

# Scale Down Is Equally Important

One major mindset shift today:

Scaling up is easy.

Production engineering is also about:

* scaling down safely
* removing waste
* reducing idle infrastructure cost

---

# Scale Down Flow

```text
Traffic Drops
       ↓
Nodes Become Underutilized
       ↓
Workloads Drained
       ↓
Unused Nodes Terminated
       ↓
Cloud Costs Reduced
```

This introduced me to real infrastructure efficiency thinking.

---

# Why Karpenter Exists

I then studied why AWS created Karpenter.

Traditional Cluster Autoscaler has several limitations:

* slower provisioning
* dependency on Auto Scaling Groups
* fixed node groups
* less intelligent node selection
* inefficient resource packing

Karpenter was designed to solve these issues.

---

# The Core Difference in Thinking

Traditional Cluster Autoscaler asks:

```text
“Which predefined node group should scale?”
```

Karpenter asks:

```text
“What is the best infrastructure configuration for this workload right now?”
```

This is a much more intelligent infrastructure model.

---

# Karpenter Internal Provisioning Logic

Karpenter dynamically analyzes:

* pending pods
* CPU requirements
* memory requirements
* GPU requirements
* architecture preferences
* Spot vs On-Demand strategy

Then dynamically launches:

* the most optimized EC2 instance type
* at that specific moment

---

# Karpenter Provisioning Flow

```text
Pending Pods
      ↓
Karpenter Analyzes Requirements
      ↓
Best EC2 Instance Type Selected
      ↓
Node Provisioned Dynamically
      ↓
Pods Scheduled Automatically
```

This is intelligent infrastructure orchestration.

---

# Understanding Bin Packing

Another major concept I studied was:

```text
Bin Packing
```

Goal:

* maximize node utilization
* minimize wasted infrastructure

---

# Bad Infrastructure Efficiency

```text
10 Nodes
↓
Each Node 20% Utilized
↓
Massive Cloud Waste
```

---

# Good Infrastructure Efficiency

```text
Fewer Nodes
↓
70–80% Utilization
↓
Better Cost Efficiency
```

Karpenter aggressively optimizes this behavior.

This was one of the clearest examples of how:

* cloud engineering
* cost optimization
* scheduling intelligence

all connect together.

---

# Spot + Karpenter Architecture

One of the most powerful concepts I learned today:

Karpenter integrates extremely well with Spot infrastructure.

Karpenter can:

* provision Spot nodes automatically
* rebalance interrupted Spot nodes
* fallback to On-Demand capacity
* optimize cloud spend dynamically

---

# Enterprise Elastic Infrastructure Flow

```text
Traffic Spike
      ↓
HPA Creates Additional Pods
      ↓
Pods Become Pending
      ↓
Karpenter Launches Spot Nodes
      ↓
Pods Scheduled
      ↓
Traffic Stabilized
      ↓
Traffic Drops
      ↓
Unused Nodes Removed
```

This is real-world elastic cloud-native infrastructure.

---

# Production Failure Scenario Studied

I also explored what happens if autoscaling itself fails.

Example:

```text
Traffic Spike
      ↓
HPA Creates Pods
      ↓
Karpenter / Cluster Autoscaler Fails
      ↓
Pods Remain Pending
      ↓
Application Becomes Unavailable
```

This helped me understand:

* autoscaling dependencies
* observability importance
* infrastructure bottlenecks
* production debugging paths

---

# Production Debugging Commands Learned

## Detect Pending Pods

```bash
kubectl get pods
```

---

## Inspect Scheduling Failure

```bash
kubectl describe pod <pod-name>
```

Common errors:

* Insufficient CPU
* Insufficient memory

---

## Verify Cluster Capacity

```bash
kubectl get nodes
```

This debugging workflow showed me how real SREs investigate scaling incidents.

---

# Most Important Engineering Mindset Learned

Modern cloud-native infrastructure is designed to be:

* elastic
* self-healing
* cost-aware
* dynamically provisioned
* infrastructure-independent

Engineers no longer think:

```text
“How many servers should exist permanently?”
```

Instead they think:

```text
“How should the platform automatically react to workload pressure?”
```

That is one of the biggest mindset shifts from traditional infrastructure engineering to cloud-native platform engineering.

---

# Real Production Architecture Understood

```text
Users
   ↓
Ingress / Load Balancer
   ↓
HPA Scales Pods
   ↓
Pods Become Pending
   ↓
Karpenter Detects Capacity Need
   ↓
EC2 Nodes Provisioned Dynamically
   ↓
Pods Scheduled
   ↓
Traffic Served
```

---

# Senior Engineering Insight From Today

Infrastructure scaling is not simply about adding servers.

It is about:

* intelligent provisioning
* workload-aware scheduling
* failure tolerance
* autoscaling reliability
* cloud cost optimization
* utilization efficiency
* automated recovery

This is what separates:

* “someone who knows Kubernetes”
  from:
* “someone who can operate Kubernetes in production.”

Key Learning:
Modern Kubernetes infrastructure is not about manually managing servers — it is about building elastic, self-healing, cost-aware systems that automatically provision, scale, recover, and optimize infrastructure based on real-time workload demand.

---

# Extended Learning Reflection — Day 93 (3-Day Deep Dive)

This topic took me nearly 3 days to properly understand because I did not want to learn Karpenter and autoscaling only at the surface level.

Instead of memorizing:

* commands
* YAML files
* Kubernetes objects

I focused on understanding:

* the real production architecture
* how autoscaling actually behaves internally
* why companies moved from traditional scaling models to intelligent provisioning
* how cloud cost optimization connects with autoscaling
* how Kubernetes infrastructure behaves under real production traffic

---

# What I Initially Thought

At first, I believed scaling in Kubernetes simply meant:

```text id="e1"
HPA creates more pods
```

But after going deeper, I realized that pod scaling alone is incomplete.

Pods still require:

* CPU
* memory
* node capacity

This completely changed my understanding of Kubernetes autoscaling.

I understood that real production scaling has TWO separate layers:

```text id="e2"
Application Scaling (Pods)
                +
Infrastructure Scaling (Nodes)
```

This was one of the biggest conceptual breakthroughs during this learning phase.

---

# What I Really Studied Deeply

Over these 3 days, I focused heavily on understanding:

* HPA internals
* Pending pod behavior
* Kubernetes scheduler limitations
* Cluster Autoscaler workflow
* Karpenter provisioning logic
* Spot-aware autoscaling
* Bin packing optimization
* Infrastructure efficiency
* Elastic infrastructure design
* Cost-aware scaling decisions
* Enterprise autoscaling architecture

I spent significant time understanding not just:

```text id="e3"
“What Karpenter does”
```

but:

```text id="e4"
“Why modern cloud infrastructure needed Karpenter in the first place”
```

---

# The Biggest Architecture Understanding

I understood that modern Kubernetes infrastructure is designed to behave dynamically.

Infrastructure is no longer:

* static
* fixed
* manually managed

Instead it becomes:

* elastic
* temporary
* workload-aware
* self-healing
* automatically provisioned

---

# Real Production Scaling Workflow I Understood

```text id="e5"
Traffic Spike
      ↓
CPU / Memory Usage Increases
      ↓
HPA Creates More Pods
      ↓
Cluster Lacks Capacity
      ↓
Pods Become Pending
      ↓
Karpenter Detects Unschedulable Workloads
      ↓
Optimal EC2 Nodes Provisioned Dynamically
      ↓
Pods Scheduled
      ↓
Traffic Stabilized
```

This workflow helped me finally understand:

* how scaling actually works internally
* how Kubernetes interacts with cloud infrastructure
* how AWS dynamically provisions compute resources
* how autoscaling behaves during real production traffic spikes

---

# Understanding Company-Level Infrastructure Thinking

One major realization during this learning phase was understanding how companies think differently from students.

Students often think:

```text id="e6"
“How do I deploy my app?”
```

Companies think:

* how to survive traffic spikes
* how to reduce infrastructure waste
* how to autoscale safely
* how to avoid overprovisioning
* how to reduce cloud bills
* how to recover automatically during failures

This shifted my thinking from:

```text id="e7"
deployment mindset
```

to:

```text id="e8"
platform engineering mindset
```

---

# Cost Optimization Understanding

Another major thing I deeply understood was how Karpenter directly connects with cloud cost optimization.

Traditional infrastructure often causes:

* idle nodes
* oversized instances
* poor utilization
* wasted compute cost

Karpenter improves this using:

* intelligent instance selection
* dynamic provisioning
* bin packing
* Spot-aware scheduling
* automatic node removal

---

# Infrastructure Efficiency Concept

I clearly understood why infrastructure efficiency matters.

Bad infrastructure:

```text id="e9"
10 nodes
↓
all 20% utilized
↓
high cloud bill
```

Optimized infrastructure:

```text id="e10"
fewer nodes
↓
70–80% utilized
↓
better efficiency
↓
lower cost
```

This helped me understand that autoscaling is not only about handling traffic.

It is also about:

* infrastructure efficiency
* intelligent resource allocation
* operational cost control

---

# Spot + Karpenter Understanding

I also deeply understood why Karpenter works extremely well with Spot infrastructure.

Karpenter can:

* dynamically provision Spot nodes
* replace interrupted Spot capacity
* fallback to On-Demand nodes
* rebalance workloads automatically

This allows companies to:

* massively reduce cloud costs
* maintain scalability
* preserve reliability

while still using interruptible infrastructure safely.

---

# Most Important Mindset Shift

The biggest thing I learned from this entire 3-day learning phase is:

```text id="e11"
Modern cloud infrastructure is designed to react automatically to workload demand.
```

Infrastructure is no longer manually managed server-by-server.

Instead:

* workloads drive infrastructure decisions
* scaling becomes dynamic
* infrastructure becomes temporary
* provisioning becomes intelligent
* recovery becomes automated

This is one of the core foundations of:

* cloud-native systems
* platform engineering
* modern Kubernetes operations
* production infrastructure design

---

# Day 94 — EBS CSI Driver + Dynamic Persistent Volumes on EKS

## Objective

Today’s goal was not just to “use storage in Kubernetes,” but to deeply understand how production-grade persistent storage actually works inside a cloud-native Kubernetes environment.

The focus of this learning session was:

* Understanding Stateful vs Stateless architecture
* Learning Kubernetes Persistent Volumes deeply
* Implementing AWS EBS CSI Driver on EKS
* Verifying dynamic storage provisioning
* Understanding how Kubernetes talks to AWS APIs
* Testing real persistence across pod recreation
* Understanding storage lifecycle and reclaim policies
* Learning production-grade storage orchestration behavior

This session was treated like a real platform engineering task, not a tutorial exercise.

---

# Why This Topic Matters

Containers are temporary.

Pods die.
Nodes terminate.
Spot instances disappear.
Deployments recreate workloads constantly.

If application data lives inside containers or node disks:

* data gets lost during restart
* recovery becomes impossible
* stateful applications fail

Modern cloud-native systems solve this by separating:

* Compute lifecycle
* Storage lifecycle

This is the foundation of:

* databases on Kubernetes
* persistent microservices
* distributed systems
* production-grade Stateful workloads

---

# Core Learning Goal

The primary concept learned today was:

```text
Pods are disposable.
Persistent storage must survive independently.
```

This is achieved through:

* PersistentVolume (PV)
* PersistentVolumeClaim (PVC)
* StorageClass
* CSI Driver

---

# Infrastructure Used

The lab was implemented on a real AWS EKS cluster created through Terraform.

Existing infrastructure already included:

* Custom VPC
* Multi-AZ public subnets
* Internet Gateway
* Route tables
* EKS cluster
* On-Demand node group
* Spot node group

---

# Production Architecture Implemented

```text
                           ┌──────────────────────┐
                           │   Kubernetes Pod     │
                           │   (storage-test)     │
                           └──────────┬───────────┘
                                      │
                                      │ mounts
                                      ▼
                           ┌──────────────────────┐
                           │        PVC           │
                           │   raj-pvc (4Gi)      │
                           └──────────┬───────────┘
                                      │ requests
                                      ▼
                           ┌──────────────────────┐
                           │    StorageClass      │
                           │    gp3-storage       │
                           └──────────┬───────────┘
                                      │ triggers
                                      ▼
                           ┌──────────────────────┐
                           │   EBS CSI Driver     │
                           │ (Controller + Node)  │
                           └──────────┬───────────┘
                                      │ AWS API Calls
                                      ▼
                           ┌──────────────────────┐
                           │   AWS EBS Volume     │
                           │   gp3 - 4Gi          │
                           └──────────┬───────────┘
                                      │ attached to
                                      ▼
                           ┌──────────────────────┐
                           │   EKS Worker Node    │
                           │   (EC2 Instance)     │
                           └──────────────────────┘
```

---

# Step-by-Step Work Completed

## 1. Fixed EKS Connectivity and Authentication

The session initially started with Kubernetes API access failures.

Observed errors:

* DNS resolution failure
* stale kubeconfig endpoint
* EKS IAM authentication issue
* RBAC authorization failure

Debugging steps performed:

* verified cluster existence
* regenerated kubeconfig
* validated AWS token generation
* diagnosed IAM authentication flow
* added EKS access entry
* granted cluster admin access

This helped understand:

```text
AWS IAM Authentication != Kubernetes Authorization
```

Deep understanding gained:

* kubeconfig internals
* EKS token-based auth
* IAM-to-Kubernetes access mapping
* control plane connectivity debugging

---

# 2. Enabled Secure IAM Access using IRSA

The EBS CSI driver requires AWS permissions to:

* create EBS volumes
* attach volumes
* detach volumes
* manage storage lifecycle

Instead of insecure static credentials, production-grade IAM Roles for Service Accounts (IRSA) was implemented.

Implemented:

* OIDC provider integration
* dedicated IAM role
* least-privilege permissions
* secure service account access

This introduced:

* secure workload identity
* cloud-native IAM integration
* production-grade Kubernetes security practices

---

# 3. Installed AWS EBS CSI Driver

The following components were successfully deployed:

## Controller Pods

Responsible for:

* talking to AWS APIs
* provisioning EBS
* attachment operations
* lifecycle orchestration

## Node Pods

Responsible for:

* mounting block devices
* node-level storage operations
* filesystem attachment

Verification completed:

```bash
kubectl get pods -n kube-system
```

Observed:

* ebs-csi-controller
* ebs-csi-node

Both successfully running.

---

# 4. Created StorageClass

A production-aware StorageClass was implemented:

Features:

* gp3 EBS type
* dynamic provisioning
* WaitForFirstConsumer scheduling
* automatic cleanup using reclaimPolicy

Important concept learned:

```text
StorageClass defines HOW storage should be provisioned.
PVC defines WHAT storage is requested.
```

---

# 5. Implemented Dynamic PVC Provisioning

A PersistentVolumeClaim requesting:

* 4Gi storage
* ReadWriteOnce access

was created.

Kubernetes automatically:

* triggered CSI driver
* provisioned EBS
* created PersistentVolume
* attached storage to node

No manual AWS volume creation was required.

This was the major conceptual breakthrough of the session.

---

# 6. Verified Real AWS Infrastructure Creation

The AWS Console was inspected directly.

Observed:

* dynamically created EBS volume
* correct gp3 type
* correct size
* active attachment to worker node

This connected Kubernetes abstractions with real cloud infrastructure behavior.

Major realization:

```text
Kubernetes was orchestrating actual AWS infrastructure automatically.
```

---

# 7. Tested Stateful Persistence

Inside the pod:

```bash
echo "raj-persistent-data" > /data/test.txt
```

Then:

* pod deleted
* pod recreated
* data verified again

Result:

* data successfully survived pod recreation

This proved:

* storage lifecycle independent from pod lifecycle
* persistence works across workload recreation

This was the most important practical verification of the day.

---

# Important Infrastructure Insight Learned

A major architectural distinction was learned today:

## Node Root Storage (20Gi EBS)

Used for:

* Operating system
* kubelet
* container runtime
* container images

Lifecycle:

* tied to EC2 instance

## PVC Storage (4Gi EBS)

Used for:

* application persistent data

Lifecycle:

* independent from pod/node lifecycle

This clarified:

* infrastructure storage
* workload storage
* persistent architecture separation

---

# Production Concepts Learned

## Stateful vs Stateless

### Stateless

* compute layer disposable
* no critical local state
* easy autoscaling
* easy recovery

Examples:

* frontend APIs
* nginx
* backend services

### Stateful

* persistent data required
* data survives restart
* requires durable storage

Examples:

* databases
* queues
* file storage

---

# Why WaitForFirstConsumer Matters

This was one of the most important concepts learned.

Without it:

* EBS may be created in wrong AZ
* pod scheduling may fail

With it:

* Kubernetes waits until pod placement decision
* then creates EBS in correct availability zone

This is production-aware storage scheduling behavior.

---

# Production Failures Simulated Mentally

Several real-world failure scenarios were analyzed:

* node failure
* spot interruption
* wrong reclaim policy
* orphaned EBS volumes
* Pending PVC debugging
* AZ mismatch issues

This built:

* operational thinking
* debugging intuition
* infrastructure troubleshooting mindset

---

# Key Engineering Learnings

## 1. Storage Must Outlive Compute

```text
Pods are temporary.
Persistent data must not be.
```

---

## 2. Kubernetes Is an Infrastructure Orchestrator

Kubernetes was not just managing containers.

It was dynamically:

* provisioning cloud disks
* attaching block storage
* mounting filesystems
* managing storage lifecycle

---

## 3. Cloud-Native Systems Separate Layers

```text
Compute Layer != Persistence Layer
```

This is foundational architecture thinking.

---

# Final Workflow Learned

```text
PVC Created
      ↓
StorageClass Selected
      ↓
CSI Driver Triggered
      ↓
AWS EBS Volume Created
      ↓
PV Auto-Created
      ↓
Volume Attached to Node
      ↓
Pod Mounted Storage
      ↓
Data Persisted Across Restarts
```

---

# Practical Verification Completed

Successfully verified:

✅ Dynamic EBS provisioning
✅ CSI driver integration
✅ IRSA secure authentication
✅ PersistentVolume auto-creation
✅ Real AWS EBS attachment
✅ Stateful persistence behavior
✅ Pod recreation recovery
✅ Kubernetes ↔ AWS orchestration
✅ Node storage vs workload storage separation

---

# Key Learning

Today’s biggest learning was understanding that Kubernetes is not merely a container scheduler — it is a distributed infrastructure orchestration platform capable of dynamically managing cloud storage lifecycles, persistence, and workload recovery in production environments.

---

Day 95 — ExternalDNS + Route53 Sync
Executive Summary

Today I studied how Kubernetes applications automatically receive DNS records using ExternalDNS and AWS Route53.

The goal was not simply understanding DNS, but understanding how production teams automate DNS management so engineers never have to manually create Route53 records whenever a new application is deployed.

I also explored how DNS fits into the complete request path:

User
↓
DNS
↓
Load Balancer
↓
Ingress
↓
Service
↓
Pods

and learned where failures occur when one of these layers breaks.

Why This Matters

In a small project:

Deploy Application
↓
Manually Create Route53 Record
↓
Done

works.

In production:

100+ Services
100+ DNS Records
Multiple Environments

manual DNS management becomes error-prone and difficult to maintain.

ExternalDNS solves this by making DNS records part of the deployment workflow.

Core Problem

Without ExternalDNS:

Deploy App
↓
Create Ingress
↓
AWS Creates ALB
↓
Engineer Logs Into Route53
↓
Creates DNS Record Manually

Every deployment requires manual work.

Solution

With ExternalDNS:

Deploy App
↓
Create Ingress
↓
AWS Creates ALB
↓
ExternalDNS Detects Resource
↓
Route53 Record Created Automatically

DNS becomes infrastructure-as-code.

Architecture Diagram
                 Kubernetes Cluster

 ┌───────────────────────────────────────────┐
 │                                           │
 │  Deployment                               │
 │       │                                   │
 │       ▼                                   │
 │      Pods                                │
 │       │                                   │
 │       ▼                                   │
 │    Service                               │
 │       │                                   │
 │       ▼                                   │
 │    Ingress                               │
 │       │                                   │
 └───────┼───────────────────────────────────┘
         │
         ▼

    AWS Load Balancer
         │
         ▼

     ExternalDNS
         │
         ▼

       Route53
         │
         ▼

   api.company.com
         │
         ▼

        User
ExternalDNS Internal Workflow
Ingress Created
↓
ExternalDNS Watches Kubernetes API
↓
Reads Hostname Annotation
↓
Finds Load Balancer Address
↓
Calls Route53 API
↓
Creates DNS Record
↓
Domain Becomes Reachable
Example

Ingress:

annotations:
  external-dns.alpha.kubernetes.io/hostname: api.company.com

ExternalDNS reads:

api.company.com

and automatically creates:

api.company.com
↓
ALB DNS Name

inside Route53.

Request Flow Learned Today
User
↓
api.company.com
↓
Route53
↓
ALB
↓
Ingress
↓
Service
↓
Pod
↓
Application

Every production request follows this chain.

Failure Scenarios Studied
Scenario 1 — ExternalDNS Down
Existing DNS Records
✅ Continue Working

New DNS Records
❌ Not Created

Updated Records
❌ Not Updated

Impact:

Existing Applications
Work Normally

Future Changes
Fail
Scenario 2 — Route53 API Unavailable
Existing Records
✅ Resolve Normally

New Records
❌ Cannot Be Created

Updates
❌ Cannot Be Applied

Key Learning:

Route53 API
≠
DNS Resolution
Scenario 3 — Pods Crash

Architecture:

DNS
✅

ALB
✅

Ingress
✅

Pods
❌

Result:

503 Service Unavailable

The DNS layer is healthy.

The application layer is broken.

Scenario 4 — Wrong Service Selector

Pods:

labels:
  app: api

Service:

selector:
  app: backend

Result:

Service
↓
No Endpoints
↓
Traffic Cannot Reach Pods
↓
503 Service Unavailable

Verification:

kubectl get endpoints
kubectl describe svc my-service
Security Concepts Learned
Principle Of Least Privilege

ExternalDNS should NOT receive:

Full Route53 Access

Instead:

Only Required Hosted Zone Permissions

Benefits:

Smaller Blast Radius
Safer Production Environment
Reduced Risk
Production Best Practices
Use Domain Filters

Example:

company.com

Only allow:

api.company.com
app.company.com

Prevent accidental modification of unrelated domains.

Avoid Manual DNS Changes

Production DNS should be:

Version Controlled
Auditable
Repeatable
Automated

Key Learnings
ExternalDNS

Automatically synchronizes Kubernetes resources with Route53.

Route53

Stores DNS records and resolves domains.

DNS Automation

Eliminates manual DNS management.

Request Path
DNS
↓
Load Balancer
↓
Ingress
↓
Service
↓
Endpoints
↓
Pods
Troubleshooting

Always identify:

Which layer is failing?

instead of assuming DNS is the problem.

Security

Use least-privilege IAM permissions and domain filtering.

---

# Day 96 — Load Testing & Performance Investigation

## Environment

```text
Platform      : Kubernetes (Kind)
Application   : Movie Sentiment API
Replicas      : 1 → 3
Cache Layer   : Redis
Load Tool     : k6
```

---

## Architecture

```text
k6
 │
 ▼
Port Forward
 │
 ▼
movie-service
 │
 ├── movie-pod-1
 ├── movie-pod-2
 └── movie-pod-3
 │
 ▼
Redis
 │
 ▼
Sentiment Analysis Engine
```

---

# Test 1 — Static Endpoint (/)

### Objective

Measure baseline cluster and networking performance.

### Results

```text
RPS            : ~221 req/sec
Avg Latency    : 449ms
P95 Latency    : 594ms
Error Rate     : 0%
```

### Analysis

System handled traffic comfortably.

No application processing involved.

This result only measures:

* Kubernetes networking
* Service routing
* FastAPI response handling

Not actual business logic.

---

# Test 2 — Business Endpoint (/analyze)

### Objective

Measure real application performance.

### Endpoint Flow

```text
Request
 ↓
Validation
 ↓
Redis Lookup
 ↓
Sentiment Analysis
 ↓
Redis Store
 ↓
Response
```

### Results

```text
RPS            : ~60 req/sec
Avg Latency    : 1.64 sec
P95 Latency    : 11.2 sec
Error Rate     : 0%
```

### Analysis

Compared to root endpoint:

```text
221 RPS
 ↓
60 RPS
```

Throughput dropped ~73%.

Business logic became the bottleneck.

No failures occurred.

System was slow but stable.

---

# Incident Investigation

### Symptom

```text
500 Internal Server Error
```

### Root Cause Chain

```text
Movie API
 ↓
Redis Dependency
 ↓
Redis Pod Pending
 ↓
PVC Missing
 ↓
StorageClass Mismatch
 ↓
PVC Name Mismatch
```

### Resolution

* Fixed StorageClass
* Corrected PVC configuration
* Redis became healthy
* Application recovered

---

# Scaling Experiment

### Change

```text
1 Pod
 ↓
3 Pods
```

### Verification

```text
movie-service endpoints:

10.244.0.13:8000
10.244.0.17:8000
10.244.0.18:8000
```

Load balancing confirmed.

---

# High Concurrency Test

### Configuration

```text
100 Virtual Users
30 Seconds
```

### Result

```text
EOF Errors
Very High Latency
100% Request Failures
```

### Investigation

Verified:

```text
Pods Healthy      ✅
Redis Healthy     ✅
Service Healthy   ✅
Endpoints Healthy ✅
No Restarts       ✅
No OOM Events     ✅
```

### Conclusion

Failure was NOT caused by:

* Kubernetes
* Pod crashes
* Redis outage
* Service misconfiguration

Most likely bottlenecks:

```text
Port Forward Saturation
Worker Saturation
CPU Constraints
Request Queueing
```

---

# Capacity Observed

Based on current environment:

```text
Static Endpoint Capacity
≈ 220 RPS

Business Endpoint Capacity
≈ 60 RPS
```

### Important Note

This is NOT the maximum application capacity.

This is only the verified capacity observed under:

```text
3 Pods
2 Workers Per Pod
Redis Cache
Kind Cluster
Port Forward Access
```

Further testing required to determine true saturation point.

---

# Key Engineering Learnings

1. Healthy Pods ≠ Healthy User Experience

2. Health Endpoint Performance ≠ Business Endpoint Performance

3. More Pods ≠ More Performance

4. Performance = Slowest Component

5. Measure Before Scaling

6. Never Assume The Bottleneck

---

# Final Outcome

Successfully completed:

✅ Application Deployment

✅ Redis Deployment

✅ PVC & Storage Troubleshooting

✅ Service Verification

✅ Endpoint Verification

✅ Load Testing

✅ Scaling Experiment

✅ Performance Investigation

✅ Production-Style Root Cause Analysis

This exercise evolved from simple load testing into a complete performance engineering and troubleshooting workflow.

---

# Day 97 – AWS CloudWatch Container Insights for EKS Monitoring and Troubleshooting

## Executive Summary

Today I explored AWS CloudWatch Container Insights and its role in monitoring, observability, and troubleshooting Kubernetes workloads running on Amazon EKS. The focus was understanding how metrics and logs are collected from nodes, pods, and containers, how CloudWatch Agent and Fluent Bit operate within a cluster, and how production teams use observability data to identify and resolve incidents.

I also studied real-world debugging scenarios involving high CPU utilization, pod restarts, OOMKilled events, application latency, and infrastructure bottlenecks. The objective was not only to collect metrics but to learn how engineers correlate metrics, logs, and Kubernetes events to identify root causes during production incidents.

---

# Why This Matters

Deploying applications is only one part of operating Kubernetes in production.

Production engineers spend significant time answering questions such as:

* Why is the application slow?
* Why are pods restarting?
* Which node is unhealthy?
* When did the incident begin?
* What changed before the outage?
* Which component is causing resource pressure?

Without observability, troubleshooting becomes guesswork.

CloudWatch Container Insights provides visibility into cluster health, workload performance, resource utilization, and operational issues, enabling engineers to reduce Mean Time To Resolution (MTTR) during incidents.

---

# Architecture Overview

```text
+----------------------+
|      EKS Cluster     |
+----------+-----------+
           |
           v
+----------------------+
| CloudWatch Agent     |
| Fluent Bit           |
+----------+-----------+
           |
           v
+----------------------+
| CloudWatch Logs      |
| CloudWatch Metrics   |
+----------+-----------+
           |
           v
+----------------------+
| Dashboards           |
| Insights Queries     |
| Alarms               |
+----------------------+
```

---

# Components Studied

## CloudWatch Agent

Responsible for collecting infrastructure and workload metrics.

Metrics collected include:

* Node CPU utilization
* Node memory utilization
* Disk utilization
* Network traffic
* Pod resource consumption
* Container resource consumption

The CloudWatch Agent typically runs as a DaemonSet, ensuring one monitoring agent is deployed per node.

---

## Fluent Bit

Responsible for log collection and forwarding.

Sources include:

* Container stdout
* Container stderr
* Application logs

Logs are forwarded into CloudWatch Logs where they can be queried and analyzed.

---

## CloudWatch Metrics

Provides visibility into:

### Cluster Level

* Total CPU usage
* Total memory usage
* Pod count
* Node count

### Node Level

* CPU utilization
* Memory utilization
* Disk utilization
* Network activity

### Pod Level

* CPU consumption
* Memory consumption
* Restart counts

### Container Level

* Container resource utilization
* Application logs

---

# Production Incident Analysis

## Scenario 1: High CPU Usage

Observed Metrics:

```text
Node CPU = 95%
Node Memory = 60%

Pod A CPU = 300%
Pod B CPU = 5%
```

### Initial Observation

The cluster was not memory constrained.

The primary indicator was excessive CPU consumption by Pod A.

### Investigation Path

1. Validate pod-level resource utilization
2. Inspect application logs
3. Review recent deployments
4. Check request traffic patterns
5. Verify CPU requests and limits
6. Investigate external dependencies

### Potential Root Causes

* Traffic spike
* Application bug
* Infinite loop
* Excessive retries
* CPU throttling
* Database latency

---

## Scenario 2: Database Timeout Leading to CPU Spike

Observed Logs:

```text
Database connection timeout
Database connection timeout
Database connection timeout
```

### Analysis

A database issue can indirectly increase application CPU usage.

Example flow:

```text
Request arrives
↓
Database query fails
↓
Application retries
↓
More retries
↓
More exception handling
↓
More logging
↓
CPU usage increases
```

### Key Learning

The component causing the incident is not always the component showing the highest resource utilization.

Root cause analysis requires understanding dependency chains.

---

## Scenario 3: Pod Restart Investigation

Observed Metrics:

```text
Node CPU = 20%
Node Memory = 25%

Pod Restart Count Increasing
```

### Analysis

Healthy CPU and memory metrics indicate that resource exhaustion is unlikely.

Investigation should shift toward:

* Application crashes
* Liveness probe failures
* Startup probe failures
* Missing secrets
* Configuration issues
* Dependency failures

### Investigation Commands

```bash
kubectl describe pod <pod-name>

kubectl logs --previous <pod-name>
```

### Key Learning

Metrics indicate that a problem exists.

Logs and events explain why it exists.

---

# Verification Commands

Verify monitoring components:

```bash
kubectl get daemonset -A
```

Verify CloudWatch namespace:

```bash
kubectl get pods -n amazon-cloudwatch
```

Verify pod metrics:

```bash
kubectl top pods -A
```

Verify node metrics:

```bash
kubectl top nodes
```

Inspect application logs:

```bash
kubectl logs <pod-name>
```

Inspect pod events:

```bash
kubectl describe pod <pod-name>
```

---

# Failure Scenarios Studied

## CloudWatch Agent Failure

Impact:

* Metrics stop updating
* Dashboards become stale

Verification:

```bash
kubectl get pods -n amazon-cloudwatch
```

---

## Fluent Bit Failure

Impact:

* Logs stop arriving
* Metrics continue functioning

Verification:

```bash
kubectl logs -n amazon-cloudwatch <fluent-bit-pod>
```

---

## IAM / IRSA Misconfiguration

Impact:

* Metrics collection fails
* Log forwarding fails

Verification:

```bash
kubectl describe serviceaccount
```

---

# Security Considerations

Production logging systems must never expose:

* Passwords
* API Keys
* JWT Tokens
* Customer Data
* Sensitive Credentials

CloudWatch stores all received log data, making log hygiene a critical security requirement.

---

# Cost Optimization Considerations

CloudWatch costs scale with:

* Log ingestion volume
* Log retention duration
* Custom metrics
* Query volume

Production best practices include:

* Appropriate retention policies
* Reduced log verbosity
* Removal of unnecessary debug logs
* Archiving historical data

---

# Key Learnings

* Monitoring is essential for operating Kubernetes in production.
* Metrics identify symptoms, not root causes.
* Logs and events provide the context needed for root cause analysis.
* High CPU utilization does not always indicate a CPU problem.
* Dependency failures can indirectly create resource pressure.
* Observability significantly reduces troubleshooting time.
* Production engineers correlate metrics, logs, and events before making infrastructure changes.

---

# Interview Questions

### What is CloudWatch Container Insights?

CloudWatch Container Insights is an AWS observability solution that collects metrics and logs from EKS clusters, nodes, pods, and containers to provide operational visibility and troubleshooting capabilities.

### How are logs collected from EKS?

Fluent Bit collects container stdout and stderr logs and forwards them to CloudWatch Logs.

### How are metrics collected?

CloudWatch Agent runs on cluster nodes and collects node, pod, and container metrics.

### What would you investigate if a pod is restarting repeatedly?

I would inspect pod events, container logs, probe configurations, resource utilization, dependency health, and restart reasons before making infrastructure changes.

### Why is observability important?

Observability enables engineers to detect issues quickly, understand system behavior, identify root causes, and reduce incident resolution time.

---

# Production Relevance

CloudWatch Container Insights is a foundational observability tool for AWS-native Kubernetes environments. Understanding how to interpret metrics, analyze logs, and investigate incidents is a critical skill for DevOps Engineers, Platform Engineers, Site Reliability Engineers, and Cloud Engineers responsible for operating production infrastructure.

---

# Day 98-100 – Debug Lab 5: EKS Pod Pending → IAM and NodeGroup Troubleshooting

# Executive Summary

During Day 98-100, I focused on understanding one of the most common Kubernetes and Amazon EKS operational incidents: Pods remaining in the Pending state. Rather than viewing Pending pods as an application issue, I learned to analyze them as scheduling and infrastructure problems.

The primary objective was to understand how Kubernetes scheduling works, how EKS worker nodes participate in workload execution, and how failures in NodeGroups, IAM permissions, cluster capacity, and scheduling constraints can prevent workloads from running successfully.

I also studied a structured troubleshooting workflow that production engineers use to diagnose Pending pods by correlating scheduler events, node availability, resource requests, and cluster infrastructure status.

---

# Why This Matters

In Kubernetes, a deployment being created successfully does not guarantee that an application will run.

Before a container starts, Kubernetes must:

1. Create the Pod object
2. Evaluate scheduling requirements
3. Find a suitable node
4. Validate available resources
5. Assign the workload

If any step fails, the Pod remains in Pending state.

Understanding Pending pod troubleshooting is critical because production outages often originate from infrastructure constraints rather than application code.

---

# Architecture Overview

```text
Developer
    │
    ▼
kubectl apply
    │
    ▼
Deployment
    │
    ▼
ReplicaSet
    │
    ▼
Pod Created
    │
    ▼
Kubernetes Scheduler
    │
    ▼
Find Suitable Node
    │
    ├── Success → Running
    │
    └── Failure → Pending
```

---

# EKS Scheduling Dependency Chain

```text
Application Pod
        │
        ▼
Kubernetes Scheduler
        │
        ▼
Worker Node
        │
        ▼
Managed NodeGroup
        │
        ▼
EC2 Instance
        │
        ▼
IAM Permissions
        │
        ▼
EKS Control Plane
```

A failure anywhere in this chain can prevent workloads from running.

---

# Key Concepts Learned

## What Does Pending Mean?

A Pending Pod means:

```text
Pod Created Successfully
But Not Scheduled Successfully
```

The container has not started.

The application has not executed.

No application logs exist yet.

This is fundamentally different from:

```text
CrashLoopBackOff
```

where the container started and then failed.

---

## Kubernetes Scheduler Fundamentals

The Kubernetes Scheduler is responsible for deciding where workloads run.

The scheduler evaluates:

* Node availability
* Resource requests
* Taints and tolerations
* Affinity rules
* Storage requirements

If no suitable node exists, scheduling fails.

---

## Requests Drive Scheduling

The scheduler evaluates:

```yaml
resources:
  requests:
    cpu: 500m
    memory: 512Mi
```

Scheduling decisions are based on requests, not limits.

This is a critical concept for Kubernetes capacity planning.

---

## Node Capacity vs Allocatable Resources

A node's physical resources are not fully available to workloads.

Example:

```text
Node Capacity:
4 CPU
8Gi Memory

Allocatable:
3.8 CPU
7.2Gi Memory
```

Kubernetes schedules using allocatable resources.

---

# Failure Scenarios Studied

## Scenario 1 – NodeGroup Failure

### Symptoms

```text
Pods remain Pending
```

### Investigation

```bash
kubectl get nodes
```

Possible output:

```text
No resources found
```

### Root Cause

Worker nodes failed to join the cluster.

Without registered nodes, workloads cannot be scheduled.

### Resolution

Restore or recreate the NodeGroup.

Verify nodes become Ready.

---

## Scenario 2 – IAM Permission Failure

### Symptoms

```text
Nodes launched
Pods Pending
```

### Root Cause

Worker nodes require IAM permissions to communicate with the EKS control plane.

Missing policies can prevent successful cluster registration.

Examples include:

* AmazonEKSWorkerNodePolicy
* AmazonEC2ContainerRegistryReadOnly
* AmazonEKS_CNI_Policy

### Resolution

Correct IAM role permissions and allow nodes to rejoin the cluster.

---

## Scenario 3 – Insufficient Resources

### Symptoms

```text
Pod Pending
```

Events:

```text
Insufficient CPU
Insufficient Memory
```

### Root Cause

The requested resources exceed cluster capacity.

### Resolution

Resize workload requests or increase available cluster capacity.

---

## Scenario 4 – Taints and Scheduling Constraints

### Symptoms

```text
Pod Pending
```

Events:

```text
Node had taint
```

### Root Cause

The scheduler found nodes but was prevented from scheduling due to missing tolerations.

### Resolution

Add matching tolerations or remove unnecessary taints.

---

# Production Debugging Workflow

Whenever a pod remains Pending:

## Step 1

Inspect Pod Status

```bash
kubectl get pods
```

---

## Step 2

Inspect Scheduler Events

```bash
kubectl describe pod <pod-name>
```

Focus on:

```text
Events
```

Most scheduling failures are explained here.

---

## Step 3

Verify Node Availability

```bash
kubectl get nodes
```

Confirm nodes are:

```text
Ready
```

---

## Step 4

Verify Resource Availability

```bash
kubectl describe node <node-name>
```

Inspect:

* Capacity
* Allocatable
* Current workload usage

---

## Step 5

Verify EKS Infrastructure

```bash
aws eks list-nodegroups
```

```bash
aws eks describe-nodegroup
```

Confirm NodeGroups are healthy and active.

---

# Verification Commands

Check pod status:

```bash
kubectl get pods
```

Describe pod:

```bash
kubectl describe pod <pod-name>
```

Check nodes:

```bash
kubectl get nodes
```

Inspect node resources:

```bash
kubectl describe node <node-name>
```

Inspect NodeGroup:

```bash
aws eks describe-nodegroup
```

---

# Security Considerations

Worker nodes should follow the principle of least privilege.

Avoid attaching:

```text
AdministratorAccess
```

to node roles.

Only required EKS policies should be granted.

Misconfigured IAM permissions can cause operational failures while also increasing security risk.

---

# Production Relevance

Pending pod troubleshooting is a core operational skill for:

* DevOps Engineers
* Cloud Engineers
* Platform Engineers
* Site Reliability Engineers

Many Kubernetes incidents are not application failures but scheduling failures caused by infrastructure constraints, misconfigurations, or resource shortages.

Understanding how to identify and resolve these issues significantly reduces incident resolution time.

---

# Key Learnings

* Pending pods indicate scheduling failures, not application failures.
* Scheduler events are the most valuable source of troubleshooting information.
* Resource requests determine scheduling decisions.
* Worker node availability directly affects workload execution.
* IAM configuration can impact node registration and cluster health.
* Infrastructure issues often appear as application outages.
* Root cause analysis should always precede scaling or configuration changes.

---

# Interview Questions

### What does a Pending pod mean?

A Pending pod has been created successfully but has not yet been scheduled onto a node.

### What is the first command you would run?

```bash
kubectl describe pod <pod-name>
```

Specifically inspect the Events section.

### Can IAM issues cause Pending pods in EKS?

Yes. Incorrect worker-node IAM permissions can prevent nodes from joining the cluster, leaving no schedulable capacity for workloads.

### How would you verify NodeGroup health?

Using:

```bash
aws eks describe-nodegroup
```

and:

```bash
kubectl get nodes
```

### What is the difference between Pending and CrashLoopBackOff?

Pending indicates scheduling failure before the container starts. CrashLoopBackOff indicates the container started but repeatedly crashed.

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
