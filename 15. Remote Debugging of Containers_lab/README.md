# Lab 15: Remote Debugging of Containers
## 🎯 Objectives
By the end of this lab, you will be able to:

- Set up remote debugging for containers without rebuilding images.

 - Expose debug ports for remote connections.

 - Mount source code directory into a container for live updates.

- Connect an IDE debugger (VS Code) to a running container.

## 📌 Prerequisites

- Basic understanding of containers and Podman/Docker.

- Podman (preferred) or Docker installed.

- VS Code with Remote - Containers extension.

- A sample application (Flask, Node.js, or Java) for debugging.

---

# 🛠️ Task 1: Expose Debug Ports

## Subtask 1.1: Identify Debug Port for Your App

- Node.js → 9229 (for --inspect)

- Python → 5678 (for debugpy)

- Java → 5005 (for JDWP)

## Subtask 1.2: Run Container with Debug Port Exposed
```
podman run -d -p 3000:3000 -p 9229:9229 --name debug-container my-node-app
```
✅ Explanation:

- -p 3000:3000 → App port.

- -p 9229:9229 → Debug port for Node.js.

- my-node-app → Replace with your app image.

Expected Outcome: Container runs with app + debug port exposed.

⚠️ Troubleshooting:

If port already in use → change host port:

```
-p 9230:9229
```
# Hands-On Expected Output:

<img src="lab 15 .png" alt="GitHub Banner" width="100%" />


---

# 🛠️ Task 2: Mount Source Code for Live Debugging
## Subtask 2.1: Run Container with Volume Mount
```
podman run -d -p 3000:3000 -p 9229:9229 -v $(pwd):/app --name debug-container my-node-app
```
✅ Explanation:

- -v $(pwd):/app → Mounts local source code to /app inside container.

- Changes on host instantly reflect inside container.

## Subtask 2.2: Verify Mounted Files
```
podman exec -it debug-container ls /app
```
✅ Expected: You should see your project files.

⚠️ If missing: Double-check path.

# Hands-On Expected Output:

<img src="lab 15 .1.png" alt="GitHub Banner" width="100%" />

---

# 🛠️ Task 3: Connect IDE Debugger to Container
## Subtask 3.1: Install Debugging Tools

- Install Remote - Containers extension in VS Code.

- For Node.js: Install Debugger for Node.js.

## Subtask 3.2: Attach Debugger in VS Code

1.  Open VS Code → Run and Debug panel.

2.  Click Create a launch.json → select Node.js.

3. Modify launch.json:

```
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "node",
      "request": "attach",
      "name": "Attach to Container",
      "address": "localhost",
      "port": 9229,
      "localRoot": "${workspaceFolder}",
      "remoteRoot": "/app"
    }
  ]
}
```

4. Start debugging → ✅ Breakpoints should work!

---

⚠️ If fail:

- Check port mapping (9229).

- Verify container is running (podman ps).

# Hands-On Expected Output:

<img src="lab 15 .2.png" alt="GitHub Banner" width="100%" />

---

# 🏁 Conclusion

- In this lab, you learned how to:

- Expose debug ports inside containers.

- Mount source code for live debugging.

- Attach VS Code debugger to a running container.

🚀 This method avoids rebuilding images and saves time in development workflows.

## 🔮 Next Steps

- Try with Python (debugpy) or Java (JDWP).

- Explore debugging in Kubernetes/OpenShift.

---

## ✅ Lab Complete! 🎉
