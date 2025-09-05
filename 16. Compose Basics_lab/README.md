# Lab 16: Compose Basics
## 🎯 Objectives
By the end of this lab, you will be able to:

- Define multi-container applications using Podman Compose.

- Configure services, ports, and volumes in podman-compose.yml.

- Start and stop multi-container applications with Podman Compose.

## 📌 Prerequisites

- Podman installed (v3.0+) → podman --version

- Podman Compose installed → pip install podman-compose

- Basic understanding of containers + YAML syntax

- Linux environment (Fedora/RHEL/WSL2 recommended)

---

# 🛠️ Task 1: Create a Simple podman-compose.yml File

## Subtask 1.1: Understand the Basic Structure

- services → Define app containers

- ports → Map host ↔ container ports

- volumes → Persist/share data

- environment → Set container env variables

## Subtask 1.2: Create the File

```
mkdir compose-lab && cd compose-lab
touch podman-compose.yml
```
Add the following content:

yaml
```
version: '3.8'
services:
  web:
    image: docker.io/nginx:alpine
    ports:
      - "8080:80"
    volumes:
      - ./html:/usr/share/nginx/html
  db:
    image: docker.io/postgres:13
    environment:
      POSTGRES_PASSWORD: example
```
---

# 🛠️ Task 2: Start the Multi-Container Application

## Subtask 2.1: Launch the App

```
podman-compose up -d
```
✅ Expected:

- Network compose-lab_default created

- Nginx + Postgres containers pulled and running

## Subtask 2.2: Verify Running Containers

```
podman ps
```
✅ Example Output:

```
CONTAINER ID  IMAGE                           STATUS       PORTS
abc123        docker.io/nginx:alpine          Up 2 mins    0.0.0.0:8080->80/tcp
def456        docker.io/postgres:13           Up 2 mins    5432/tcp
```
# Hands-On Expected Output:

<img src="lab 16 .png" alt="GitHub Banner" width="100%" />

---

# 🛠️ Task 3: Test the Application
## Subtask 3.1: Web Service
```
mkdir html
echo "<h1>Hello from Podman Compose!</h1>" > html/index.html
```
Now access:

```
curl http://localhost:8080
```
✅ Output:
```
html

<h1>Hello from Podman Compose!</h1>
Or open in browser → http://localhost:8080
```

## Subtask 3.2: Database Service
Connect to Postgres:

```
podman exec -it compose-lab_db_1 psql -U postgres
```
Run SQL:

sql
```
SELECT version();
\q
```
✅ Expected:

nginx
```
PostgreSQL 13.x on x86_64-pc-linux-gnu...
```
---

# 🛠️ Task 4: Stop and Clean Up
## Subtask 4.1: Stop the Application
```
podman-compose down
```
✅ Containers + network removed.

## Subtask 4.2: Verify Cleanup
```
podman ps -a
```
✅ No containers should be listed.

---

## 🐞 Troubleshooting Tips

- Port conflicts → change mapping (8081:80)

- SELinux issues → add :Z suffix in volume:
```
yaml

volumes:
  - ./html:/usr/share/nginx/html:Z
Image pull errors → check internet or image name
```
---

# 🏁 Conclusion

You have learned how to:

- Define multi-container services with podman-compose.yml.

- Configure ports, volumes, and environment variables.

- Run and test Nginx + PostgreSQL together.

- Stop and clean up with podman-compose down.

🚀 Ye foundation tumhe complex containerized apps build karne ke liye ready karti hai.
