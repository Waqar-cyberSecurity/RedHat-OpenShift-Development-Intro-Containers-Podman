
# Lab 1: Introduction to Containers

## 📝 Objectives
By the end of this lab, you will be able to:

- Understand the fundamental concepts of containerization  
- Learn about container isolation and portability  
- Gain hands-on experience running a simple container using **Podman**

---

## 📋 Prerequisites
- A Linux-based system (Ubuntu 20.04+ or Fedora recommended)  
- **Podman** installed (see setup below)  
- Basic familiarity with the command line  

---

## ⚙️ Setup

### 1. Install Podman

**For Ubuntu/Debian:**
```bash
sudo apt-get update
sudo apt-get install -y podman

```
For Fedora/RHEL/CentOS:
```
sudo dnf install -y podman
```

Verify Installation:

```
podman --version
```

✅ Expected Output:
```
podman version 4.x.x (or similar)
```


# 🧪 Lab Tasks
# Task 1: Understanding Container Basics
## What is a Container?
A container is a lightweight, standalone executable package that includes everything needed to run an application (code, runtime, libraries, and settings).
Unlike virtual machines, containers share the host OS kernel, making them faster and more efficient.

Key Characteristics of Containers:

Isolation: Containers run in isolated user spaces.

Portability: Containers can run consistently across different environments.

# Task 2: Running a Simple Container with Podman
# Subtask 1: Pull a Container Image
```
podman pull hello-world
```

✅ Expected Output:
```
Trying to pull docker.io/library/hello-world:latest...
Status: Downloaded newer image for hello-world:latest
```
# Subtask 2: Run the Container
```
podman run hello-world
```

✅ Expected Output:
```
Hello from Docker!
This message shows that your installation appears to be working correctly.
```
# Subtask 3: Verify Container Execution
```
podman ps -a
```

✅ Expected Output: A table showing the hello-world container with status Exited.

# Task 3: Exploring Container Isolation

Run an interactive container:
```
podman run -it alpine sh
```

✅ You will enter the container shell (/# prompt).

Check OS details inside container:
```
cat /etc/os-release
```

✅ Expected Output: Alpine Linux details (different from host OS).

Exit the container:
```
exit
```
# ✅ Hands-On Expected Output Preview :

 <img src="lab1 .png" alt="GitHub Banner" width="100%" />


# 🛠️ Troubleshooting Tips

 Permission Errors
Run Podman with sudo if needed:
```
sudo podman run hello-world
```

Or configure rootless mode:
```
podman system migrate
```

 Network Issues
Ensure internet access is available to pull images.
Use debug logs if needed:
```
podman --log-level=debug
```
# ✅ Conclusion

In this lab, you:

Learned the core concepts of containers (isolation & portability)

Successfully ran a hello-world container using Podman

Explored container isolation with Alpine Linux

Containers provide a powerful way to package and deploy software consistently across environments.

# 🚀 Next Steps

Explore multi-container applications with Podman Compose

Learn container orchestration with Kubernetes or OpenShift
