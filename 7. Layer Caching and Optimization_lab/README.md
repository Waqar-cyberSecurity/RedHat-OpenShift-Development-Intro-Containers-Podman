# Lab 7: Layer Caching and Optimization
## 🎯 Objectives
By the end of this lab, you will be able to:

 - Understand Docker/Podman layer caching mechanism.

 - Learn techniques to optimize image layers.

 - Analyze layer sizes and build efficiency.

 - Implement best practices for image building.

---

# 📝 Prerequisites

 - Podman or Docker installed (Podman recommended for OpenShift).

 - Basic understanding of Dockerfile syntax.

 - Terminal/shell access.

 - Internet connection for pulling base images.


## Verify installation:

```
podman --version
```

## ⚙️ Setup
Create a working directory:

```
mkdir layer-optimization-lab && cd layer-optimization-lab
```
---

# 🔹 Task 1: Create Initial Dockerfile
## 1.1 Non-Optimized Dockerfile

Create a file named Dockerfile.initial:
```
dockerfile


FROM ubuntu:22.04
RUN apt-get update
RUN apt-get install -y curl wget
RUN apt-get install -y python3 python3-pip
RUN pip install flask
COPY app.py /app/
WORKDIR /app
CMD ["python3", "app.py"]
```

Create app.py:
```
python


from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello():
    return "Hello from optimized container!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=8080)
```

## 1.2 Build Initial Image
```
podman build -t myapp:initial -f Dockerfile.initial .
podman images myapp:initial 
```

✅ Expect: Image is large with multiple layers.

---

# 🔹 Task 2: Optimize the Dockerfile
## 2.1 Layer Consolidation
Create Dockerfile.optimized:

dockerfile:


FROM ubuntu:22.04
RUN apt-get update && \
    apt-get install -y curl wget python3 python3-pip && \
    pip install flask && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*
COPY app.py /app/
WORKDIR /app
CMD ["python3", "app.py"]

## 2.2 Build Optimized Image
```
podman build -t myapp:optimized -f Dockerfile.optimized .
podman images myapp:*
```
✅ Expect: Smaller image size due to fewer layers and cleanup.

--- 

# 🔹 Task 3: Leverage Build Cache
## 3.1 Cache Behavior


 - Modify app.py (e.g., add a comment).

 - Rebuild:

```
podman build -t myapp:optimized -f Dockerfile.optimized .
```
✅ Observation: Earlier steps reused from cache, only final steps rebuilt.

## 3.2 Cache Busting

Add to Dockerfile:
```
dockerfile:


ARG CACHEBUST=1
RUN echo "Cache bust: $CACHEBUST"
```

Rebuild with cache-busting:

```
podman build -t myapp:optimized \
  --build-arg CACHEBUST=$(date +%s) \
  -f Dockerfile.optimized .
```
⚠️ Tip: Use --no-cache to force full rebuild.

---

# 🔹 Task 4: Inspect Layers
## 4.1 View History

```
podman history myapp:optimized
podman inspect myapp:optimized
```
## 4.2 Analyze Layer Sizes
Install and run dive:

```
sudo apt-get install dive
dive myapp:optimized
```
✅ Dive shows contents and sizes of each layer.

---

# 🔹 Task 5: Advanced Optimization (Multi-stage Builds)
## 5.1 Multi-stage Example

Create Dockerfile.multistage:

dockerfile:
```

# Build stage
FROM ubuntu:22.04 as builder
RUN apt-get update && apt-get install -y python3 python3-pip
COPY requirements.txt .
RUN pip install -r requirements.txt

# Runtime stage
FROM ubuntu:22.04
COPY --from=builder /usr/local/lib/python3.10/dist-packages /usr/local/lib/python3.10/dist-packages
COPY app.py /app/
WORKDIR /app
CMD ["python3", "app.py"]
```

Build:
```
podman build -t myapp:multistage -f Dockerfile.multistage .
```
✅ Final image is smaller (only runtime dependencies included).
---

# ✅ Conclusion

 - Layer consolidation reduces size and improves efficiency.

 - Proper layer ordering maximizes cache usage.

 - Multi-stage builds strip out build-time dependencies.

 - Regular inspection ensures optimized builds.

# 🔍 Verification

Run optimized container:

```
podman run -d -p 8080:8080 myapp:optimized
curl localhost:8080
```
Compare image sizes:

```
podman images myapp:*
```

# 🚀 Further Exploration

 - Try different base images (e.g., Alpine, Distroless).

 - Explore Buildah for advanced builds.

 - Experiment with --layers and --squash options in Podman.
