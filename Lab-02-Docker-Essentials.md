# Lab 2: Docker Essentials for Infrastructure & Observability Pipelines

## 📌 Objective
Learn fundamental Docker container operations, ephemeral execution, and persistent storage management using volume bindings to handle log and telemetry data flows.

## 🛠️ Steps Performed
1. **Environment Verification:** 
   - Validated Docker engine installation and active daemon status in a Linux/Codespaces environment.
2. **Interactive Container Execution:** 
   - Pulled the official lightweight `ubuntu:latest` image and deployed an interactive container instance.
   - Simulated internal application workloads by generating custom log structures (`/var/log/app-logs/service.log`).
3. **Data Persistence via Bind Mounts:** 
   - Configured local directory mapping (`-v` flag) to decouple log data storage from the container lifecycle.
   - Verified successful external extraction and persistence of telemetry status logs (`telemetry-status.log`) on the host system.

## 📸 Visual Evidence
<img width="1433" height="331" alt="Docker1" src="https://github.com/user-attachments/assets/f8698c1a-ad7b-473b-b3f5-8d2a7967eb97" />
@BogarTrejo ➜ /workspaces/az104-laboratorios (main) $ `docker --version`

Docker version 29.3.0-1, build 5927d80c76b3ce5cf782be818922966e8a0d87a3

@BogarTrejo ➜ /workspaces/az104-laboratorios (main) $ `docker ps`

CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

@BogarTrejo ➜ /workspaces/az104-laboratorios (main) $ `docker run -it --name lab-observability ubuntu:latest bash`

Unable to find image 'ubuntu:latest' locally
latest: Pulling from library/ubuntu
a3679419df18: Pull complete 
ed819469700f: Pull complete 
e16351a257e4: Download complete 
Digest: sha256:3131b4cc82a783df6c9df078f86e01819a13594b865c2cad47bd1bca2b7063bb
Status: Downloaded newer image for ubuntu:latest
root@993b5c4c0c5b:/# mkdir /var/log/app-logs
echo "2026-07-28 [INFO] Application service started successfully in container." > /var/log/app-logs/service.log
echo "2026-07-28 [ERROR] High CPU utilization detected on pipeline route." >> /var/log/app-logs/service.log
exit
exit

@BogarTrejo ➜ /workspaces/az104-laboratorios (main) $ `mkdir -p ./docker-logs-test`

@BogarTrejo ➜ /workspaces/az104-laboratorios (main) $` docker run --rm -v $(pwd)/docker-logs-test:/app/logs ubuntu:latest bash -c "echo '2026-07-28 [INFO] Telemetry pipeline route active and synchronized.' > /app/logs/telemetry-status.log"`

@BogarTrejo ➜ /workspaces/az104-laboratorios (main) $ `cat ./docker-logs-test/telemetry-status.log`

2026-07-28 [INFO] Telemetry pipeline route active and synchronized.

## 🧠 Key Learnings & Architecture Notes
- **Container Ephemerality:** Compute layers can be safely treated as disposable, isolating application runtimes from persistent storage requirements.
- **Log Routing Foundation:** Understanding volume bindings is a critical prerequisite for configuring robust telemetry forwarders (like Splunk Universal Forwarders, Cribl Edge, or Azure Monitor Agents) inside containerized cloud architectures.
