# Lab 14: Networking in Containers
## Objectives

By the end of this lab, you will:

- Understand container networking fundamentals in Podman

- Learn how to inspect network configurations

 - Practice running containers with port publishing

## Prerequisites

- Linux-based system (Fedora, CentOS, or RHEL recommended)

- Podman installed (sudo dnf install podman or equivalent)

 - Basic familiarity with terminal commands

---

# Task 1: List Podman Networks

## Subtask 1.1: Check Available Networks
```
podman network ls
```

✅ Use: Lists all available Podman networks.

📌 Why Important: Helps to see which networks exist (default = podman).

Expected Output
```
nginx
NETWORK ID    NAME        DRIVER
2f259bab93aa  podman      bridge
```
👉 Podman creates a default bridge network for container-to-container communication.

## Subtask 1.2: Verify Network Details
```
podman network inspect podman
```

✅ Use: Shows detailed settings of a specific network.

📌 Why Important: Displays subnet ranges, gateway IP, and DNS settings → useful for debugging connectivity issues.

---

# Task 2: Inspect Network Settings
## Subtask 2.1: Create a Custom Network
```
podman network create lab-network
```

✅ Use: Creates a new bridge network.

📌 Why Important: Custom networks allow isolation between containers.

Verify creation:
```
podman network ls | grep lab-network
```
## Subtask 2.2: Inspect Custom Network
```
podman network inspect lab-network
```

✅ Use: Shows configuration of the custom network.

📌 Why Important: Confirms subnet, gateway, and IPAM configs.

⚠️ Troubleshooting: If creation fails → check permissions with:

```
podman info
```
---

# Task 3: Run Containers with Port Publishing
## Subtask 3.1: Run a Container with Port Mapping
```
podman run -d --name webapp -p 8080:80 docker.io/library/nginx
```
✅ Use: Runs an Nginx container and maps host port 8080 → container port 80.
📌 Why Important: Allows external access to Nginx running inside container.

## Subtask 3.2: Verify Port Accessibility
```
curl http://localhost:8080
```
✅ Use: Tests if the container is reachable.
📌 Expected Output: Nginx welcome page HTML.

⚠️ If fails:

- Check if container is running → podman ps

- Check firewall rules → sudo firewall-cmd --list-ports

## Subtask 3.3: Attach Container to Custom Network
Stop old container:

```
podman stop webapp
```
Relaunch on custom network:

```
podman run -d --name webapp -p 8080:80 --network lab-network docker.io/library/nginx
```
Verify:

```
podman inspect webapp | grep -A 5 "Networks"
```
📌 Why Important: Attaching to a custom network ensures containers can communicate in isolation.

---

# Conclusion

In this lab, you:

- Explored Podman’s default and custom networks.

- Inspected network configurations for troubleshooting.

- Published container ports to the host system.

## 🔑 Key Takeaways

- Podman uses bridge networking by default.

- Port mapping (-p) is essential for external access.

- Custom networks provide isolation and flexibility for multi-container apps.
