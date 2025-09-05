# 🛠 Lab 13: Troubleshooting Containers

## 🎯 Objectives

By the end of this lab, you will be able to:

 - Diagnose container failures using logs and inspect commands

 - View container logs with filters

 - Inspect container state (status, config, resources)

 - Debug inside a running container using exec

## 📌 Prerequisites
 - CLI basics (Linux commands)

 - Podman (recommended) or Docker installed

 - Active internet connection

---

## ⚙️ Lab Setup

🔹 Install Podman
RHEL/CentOS/Fedora:

```
sudo dnf install -y podman
```
Debian/Ubuntu:

```
sudo apt-get install -y podman
```
🔹 Pull and Run Sample Container
```
podman pull docker.io/library/nginx:alpine
podman run -d --name nginx-test -p 8080:80 nginx:alpine
```
✅ Runs an Nginx container accessible on port 8080.

---

# 📌 Task 1: View Container Logs with Filters

## 🔹 Subtask 1.1: View Basic Logs

```
podman logs nginx-test
```
➡️ Displays Nginx access/error logs.

Follow logs in real-time:

```
podman logs -f nginx-test
```

➡️ Streams logs continuously until stopped (Ctrl+C).

## 🔹 Subtask 1.2: Filter Logs by Time
Logs from last 5 minutes:

```
podman logs --since 5m nginx-test
```
Last 10 log lines:

```
podman logs --tail 10 nginx-test
```

⚠️ Troubleshooting Tip: If logs are empty → check if container is running:

```
podman ps
```
---

#  📌 Task 2: Inspect Container State
##  🔹 Subtask 2.1: Check Status & Details
List running containers:

```
podman ps
```
Inspect full state:

```
podman inspect nginx-test
```
Key fields to check:

- "State" → Running, ExitCode, Error

- "Config" → Environment variables, entrypoint

- "NetworkSettings" → IP address, exposed ports

## 🔹 Subtask 2.2: Monitor Resource Usage
```
podman stats nginx-test
```
➡️ Shows live CPU, memory, and network usage.

⚠️ If container is stuck → restart it:

```
podman stop nginx-test && podman start nginx-test
```
---

# 📌 Task 3: Use exec to Debug Inside Container
## 🔹 Subtask 3.1: Enter Container Shell
```
podman exec -it nginx-test /bin/sh
```
➡️ Opens interactive shell inside the container.

Check running processes:

```
ps aux
```

## 🔹 Subtask 3.2: Debug Service Issues
Check Nginx configuration:

```
cat /etc/nginx/nginx.conf
```
Test Nginx connectivity from inside container:

```
curl localhost
```
➡️ Should return the default Nginx welcome page.

⚠️ Tip: If /bin/sh fails → try /bin/bash.

---

# ✅ Conclusion

✔️ Learned to view & filter logs for troubleshooting

✔️ Used inspect and stats to analyze container state & resources

✔️ Debugged services using exec inside running container

---

## 🚀 Next Steps
 - Practice debugging a misconfigured container (e.g., crashes on startup).

 - Explore disk usage by containers:

```
podman system df
```
---

## 🧹 Cleanup

```
podman stop nginx-test
podman rm nginx-test
```

## ⚡ Importance:
- This lab gives you real-world troubleshooting skills that every DevOps/Container engineer needs. Debugging containers is a core competency for Red Hat OpenShift certification and production support.
