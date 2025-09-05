# Lab 17: Compose with Dependencies
## 🎯 Objectives

By the end of this lab, you will be able to:

- Manage service dependencies with depends_on.

- Scale services with Docker Compose.

- Test connectivity between dependent services (Redis + Flask app).

## 📌 Prerequisites

- Docker 20.10+ installed → docker --version

- Docker Compose v2.0+ → docker-compose --version

- Basic Docker concepts clear

- Terminal/Command prompt access

---

# Lab Setup

Create a new directory for the lab:
```
mkdir compose-dependencies-lab
```
```
cd compose-dependencies-lab
```

---

# 🛠️ Task 1: Using depends_on Directive

## Subtask 1.1: Create Compose File

- Create a docker-compose.yml file with the following content:

```
version: '3.8'

services:
  redis:
    image: redis:alpine
    ports:
      - "6379:6379"
  
  webapp:
    image: nginx:alpine
    ports:
      - "8080:80"
    depends_on:
      - redis
```

# Subtask 1.2: Understand depends_on

- Redis starts before webapp.

- ⚠️ Does not wait until Redis is “ready,” only until “started.”

# Subtask 1.3: Start Services
```
docker-compose up -d
docker-compose ps
```
✅ Expected: Redis first → WebApp next → Both running.

---

# 🛠️ Task 2: Scale Service Replicas

## Subtask 2.1: Update Compose File
Update the docker-compose.yml:
```
version: '3.8'

services:
  redis:
    image: redis:alpine
    ports:
      - "6379:6379"
  
  webapp:
    image: nginx:alpine
    ports:
      - "8080-8082:80"
    depends_on:
      - redis
    deploy:
      replicas: 3
```

## Subtask 2.2: Start Scaled Services
```
docker-compose up -d --scale webapp=3
```
## Subtask 2.3: Verify Scaling
```
docker-compose ps
docker-compose exec webapp hostname
```
✅ Expected:

1 Redis container.

3 WebApp containers (different hostnames).

---

# 🛠️ Task 3: Test Inter-Service Connectivity
## Subtask 3.1: Create Flask App with Redis

- Create a new file app.py:

```

from flask import Flask
import redis
import os

app = Flask(__name__)
redis_host = os.environ.get('REDIS_HOST', 'redis')
redis_client = redis.Redis(host=redis_host, port=6379)

@app.route('/')
def hello():
    count = redis_client.incr('hits')
    return f'Hello World! This page has been viewed {count} times.\n'

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

- Create a Dockerfile:

dockerfile
```
FROM python:3.9-alpine
WORKDIR /app
COPY . .
RUN pip install flask redis
CMD ["python", "app.py"]
```

## Subtask 3.2: Update Compose File
yaml
```
version: '3.8'

services:
  redis:
    image: redis:alpine
  
  webapp:
    build: .
    ports:
      - "5000:5000"
    environment:
      - REDIS_HOST=redis
    depends_on:
      - redis
```

## Subtask 3.3: Test Connectivity

1. Build and start:

```
docker-compose up -d 
```
2. Test the application:

```
curl http://localhost:5000
```
✅ Expected:

Hello World! This page has been viewed 1 times.
Hello World! This page has been viewed 2 times.

Counter increments each refresh! 🎉

---

## 🐞 Troubleshooting

- Services fail to start → docker-compose logs

- Scaling error → Check port conflicts

- Connectivity issues → Ensure REDIS_HOST=redis matches service name

---

# 🏁 Conclusion

In this lab, you learned:

- How to manage service dependencies with depends_on.

- How to scale services with --scale.

- How to connect services (Flask ↔ Redis).

These are fundamentals for orchestrating multi-service apps.

---

## 🧹 Cleanup

To remove all resources:
```
docker-compose down
```
---

# Additional Resources

- Docker Compose documentation: https://docs.docker.com/compose/
- Redis official image: https://hub.docker.com/_/redis
- Flask documentation: https://flask.palletsprojects.com/



