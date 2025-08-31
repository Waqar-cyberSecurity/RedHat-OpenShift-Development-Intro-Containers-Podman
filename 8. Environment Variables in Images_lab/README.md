# Lab 8: Environment Variables in Images

## 🎯 Objectives
- Use `ENV` and `ARG` instructions in Containerfiles  
- Define environment variables in container images  
- Override environment variables at runtime  
- Inspect environment variables in running containers  

---

## 🛠️ Prerequisites
- Basic Linux command line knowledge  
- Podman or Docker installed (Podman recommended)  
- Text editor (Vim, Nano, or VS Code)  
- Internet access to pull base images  


## Check Podman version:
```bash
podman --version
```
---

# 🔹 Task 1: Define Environment Variables in Containerfile
## Step 1: Create Containerfile

```
mkdir env-lab && cd env-lab
```
```
cat <<EOF > Containerfile
FROM registry.access.redhat.com/ubi8/ubi-minimal
ENV APP_NAME="MyApp" \
    APP_VERSION="1.0" \
    APP_ENV="development"
CMD echo "Running \$APP_NAME v\$APP_VERSION in \$APP_ENV mode"
EOF
```
## Step 2: Build and Run
```
podman build -t env-demo .
podman run env-demo
```
✅ Expected Output:
```
sql

Running MyApp v1.0 in development mode
```
---

# 🔹 Task 2: Override Environment Variables at Runtime
## Step 1: Override Using CLI
```
podman run -e APP_ENV="production" -e APP_VERSION="2.0" env-demo
```
✅ Output:
```
sql

Running MyApp v2.0 in production mode
```

## Step 2: Use Environment File
```
cat <<EOF > app.env
APP_NAME=ProductionApp
APP_VERSION=3.0
APP_ENV=staging
EOF

podman run --env-file=app.env env-demo
```
✅ Output:
```
sql

Running ProductionApp v3.0 in staging mode
```
---
#🔹 Task 3: Inspect Variables
View All Environment Variables
```
podman run -d --name env-container env-demo
podman exec env-container env
```
Sample Output:
```
ini

APP_NAME=MyApp
APP_VERSION=1.0
APP_ENV=development
Use ARG for Build-Time Variables
```
# Hands-On Expected Output:
<img src="lab 8 .png" alt="GitHub Banner" width="100%" />

---

Use ARG for Build-Time Variables
```
cat <<EOF > Containerfile
FROM registry.access.redhat.com/ubi8/ubi-minimal
ARG APP_BUILD_NUMBER
ENV APP_BUILD=$APP_BUILD_NUMBER
CMD echo "Build number: $APP_BUILD"
EOF
```
```
podman build --build-arg APP_BUILD_NUMBER=42 -t arg-demo .
podman run arg-demo
```
✅ Output:
```
yaml

Build number: 42
```
# Hands-On Expected Output:
<img src="lab 8.1 .png" alt="GitHub Banner" width="100%" />


## 🛑 Troubleshooting
 - Variables not showing → check syntax (= no spaces)

 - ARG issue → must pass --build-arg at build time

 - Permission issue → try sudo podman

---

# ✅ Conclusion

In this lab, we learned:

 - How to use ENV to define persistent variables

 - How to override variables at runtime (-e, --env-file)

 - How to inspect running container environments

 - How to use ARG for build-time configuration

These are essential for configuring containerized applications in OpenShift and Kubernetes.
