# 📘 Lab 3: Running Containers with Podman

## 🎯 Goal of the Lab

The goal of this lab is to learn how to:

*  Run containers in the background (detached mode).

*  Map container ports to host ports.

*  Mount host directories into containers.

*  Assign custom names to containers.


 
By the end of this lab, you will have a strong foundation in managing containers with Podman.

# 🛠 Prerequisites

* Podman installed on your system (v3.0+ recommended).

* Basic command-line knowledge.

* A Linux-based system (recommended) or Podman installed on Windows/macOS.


Check Podman version:


```
podman --version
```

👉 This verifies that Podman is installed and ready to use.


# ⚡ Task 1: Running a Container in Detached Mode

## Steps

Run an Nginx container in detached mode:

```
podman run -d docker.io/library/nginx:alpine 
```

👉 -d means detached mode, so the container runs in the background.


Verify running containers:

```
podman ps
```

👉 Shows list of running containers with their IDs, names, and status.


Check container logs if something fails:

```
podman logs <container_id>

```
👉 Displays logs of the container (useful for debugging).


# ⚡ Task 2: Port Mapping

## Steps

Stop existing Nginx containers:

```
podman stop $(podman ps -q)
```

👉 Stops all running containers ($(podman ps -q) fetches their IDs).

Run Nginx with port mapping:
```
podman run -d -p 8080:80 docker.io/library/nginx:alpine
```

👉 -p 8080:80 maps port 8080 on the host to port 80 inside the container.

Check port mapping:
```
podman port <container_id>

```
👉 Shows which host port is connected to the container port.

Test access:

```
curl http://localhost:8080
```
👉 Fetches Nginx welcome page from the container.

If error:

```
ss -tulnp
```

👉 Shows which ports are already in use (to avoid conflicts).


# ⚡ Task 3: Volume Mounts
## Steps

Create a directory with content:
```
mkdir ~/nginx-content
echo "Hello from host!" > ~/nginx-content/index.html
```
👉 Creates a local folder and an HTML file that will be served by Nginx.

Run Nginx with volume mount:

```
podman run -d -p 8081:80 -v ~/nginx-content:/usr/share/nginx/html:Z docker.io/library/nginx:alpine
```

👉 -v host_path:container_path mounts your host directory into the container.

👉 :Z fixes SELinux permission issues (important on Fedora/RHEL).

### Test:

```
curl http://localhost:8081
```
👉 Should return Hello from host! (your custom file is served).


# ⚡ Task 4: Assigning Custom Names
## Steps

Run a container with a custom name:

```
podman run -d --name my-nginx -p 8082:80 docker.io/library/nginx:alpine
```
👉 --name my-nginx makes it easy to manage the container without using IDs.

Inspect by name:

```
podman inspect my-nginx | grep -i status
```
👉 Shows container status (running/stopped).

Stop by name:

```
podman stop my-nginx
```
👉 Stops the container using its name instead of ID.

# ✅ Conclusion

### In this lab, you learned how to:

* Run containers in detached mode.

* Map host ports to container ports.

* Use volume mounts to share data between host and container.

* Assign custom names for easier container management.

These are fundamental container management skills that are essential in production environments.




# 🚀 Next Steps

## * Clean up containers:

```
podman rm -f $(podman ps -aq)
```

👉 Removes all containers (running or stopped).

### * Explore more Podman features:

```
podman --help
```
