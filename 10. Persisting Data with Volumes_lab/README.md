# Lab 10: Persisting Data with Volumes

## 🔑 Importance
One of the biggest challenges in containerized environments is data persistence. By default, containers are ephemeral — meaning once a container is removed, its data is lost. Using volumes and bind mounts, we can make data persistent, ensuring it survives container restarts, upgrades, or removals. This concept is critical for running databases, web servers, and stateful applications inside containers.


## 🎯 Objectives
By the end of this lab, you will be able to:

 - Configure persistent storage for containers using volumes

 - Create and manage named volumes

 - Mount volumes in containers

 - Use bind mounts to persist data using host directories


## ⚙️ Prerequisites

 - Podman installed (v3.0+ recommended)

 - Basic command-line knowledge

 - Linux-based system (Fedora, Ubuntu, RHEL, or WSL2 on Windows)

---

# 🧪 Lab Tasks
## Task 1: Creating Named Volumes
```
# Create a named volume
podman volume create myapp_data  

# Verify
podman volume ls  

# Inspect volume details
podman volume inspect myapp_data
```
---

# Task 2: Mounting Volumes in Containers
```
# Run container with volume
podman run -d --name webapp -v myapp_data:/var/www/html docker.io/library/nginx  

# Persist data in volume
podman exec webapp sh -c "echo 'Hello, Volume!' > /var/www/html/index.html"  
podman exec webapp cat /var/www/html/index.html  

# Verify persistence (after removing and recreating container)
podman rm -f webapp  
podman run -d --name webapp_new -v myapp_data:/var/www/html docker.io/library/nginx  
podman exec webapp_new cat /var/www/html/index.html  

```
✅ Output should still be:
```
Hello, Volume!
```
---

# Task 3: Using Bind Mounts with Host Directories
```

# Create host directory
mkdir ~/host_data  
echo "Hello, Bind Mount!" > ~/host_data/index.html  

# Run container with bind mount
podman run -d --name bind_example -v ~/host_data:/usr/share/nginx/html:Z docker.io/library/nginx  

# Verify inside container
podman exec bind_example cat /usr/share/nginx/html/index.html  

# Modify host file and re-check
echo "Updated content!" >> ~/host_data/index.html  
podman exec bind_example cat /usr/share/nginx/html/index.html
```
  ---

# 🛠️ Troubleshooting Tips

- Permission Issues → Use :Z or :z for SELinux contexts

 - Volume Not Found → Check with podman volume ls

 - Bind Mount Path → Always use absolute paths

---

# 📌 Conclusion
In this lab, you learned how to:

 - Create & manage named volumes for persistent storage

 - Attach volumes to containers and keep data even after container removal

 - Use bind mounts to directly map host directories into containers


➡️ These techniques are critical for databases, logs, and web content where data persistence matters.

---
⚡ Recommended Next Step: Try combining volumes with a database container (e.g., PostgreSQL or MySQL) to see real-world persistent storage in action.
