# Lab 2: Exploring Podman CLI

## Objectives
By the end of this lab, you will be able to:
- Understand and use basic Podman commands
- Manage container lifecycle (run, stop, restart, remove)
- Inspect container details
- List running and stopped containers

---

## Prerequisites
- A Linux system with Podman installed (Fedora, CentOS, RHEL, or Ubuntu)
- Basic familiarity with Linux command line
- Internet access (to pull container images)

---

## Setup Requirements
Install Podman if not already present:

```bash
# For Fedora/CentOS/RHEL
sudo dnf install podman

# For Ubuntu/Debian
sudo apt-get update && sudo apt-get install podman
```
Verify installation:

```
podman --version
``` 
# Lab Tasks
# Task 1: Listing Containers
Objective: Learn how to list running and all containers.

## Step 1: List running containers

```
podman ps
```
## Expected Output: If no containers are running, the output will be empty.

## Step 2: List all containers (including stopped ones)

```
podman ps -a
```
Expected Output: Shows all containers, including those that are stopped.


Troubleshooting Tip: If you see permission errors, run with sudo or configure rootless Podman properly.

# Task 2: Running a Container
Objective: Learn how to run a container.

## Step 1: Run a sample container (Alpine Linux)

```
podman run -it --name my_alpine alpine sh
```
## Explanation:

-it: Interactive terminal

--name: Assigns a name to the container

alpine: The container image

sh: The command to run inside the container

Expected Outcome: You will enter an interactive shell inside the Alpine container.
Exit the container by typing exit.

## Step 2: Verify the container is running

```
podman ps
```
## Expected Output: The my_alpine container should appear in the list.


# Hand-On Output Preview
<img src="lab2 .png" alt="GitHub Banner" width="100%" />



# Task 3: Stopping a Container

## Objective: Learn how to stop a running container.

## Step 1: Stop the running container

```
podman stop my_alpine
```

## Expected Outcome: The container stops running. Verify with:
```
podman ps -a
```

Status should show Exited.

# Task 4: Restarting a Container

Objective: Learn how to restart a stopped container.

## Step 1: Restart the stopped container
```
podman restart my_alpine
```

## Expected Outcome: The container restarts. Verify with:
```
podman ps
```

The container should show as Up.

# Task 5: Removing a Container

Objective: Learn how to remove a container.

## Step 1: Stop the container (if running)
```
podman stop my_alpine
```

# Step 2: Remove the container
```
podman rm my_alpine
```

## Expected Outcome: The container is deleted. Verify with:
```
podman ps -a
```

The container should no longer appear.

# Hand-On Output Preview 

<img src="lab 2.1 .png" alt="GitHub Banner" width="100%" />

# Task 6: Inspecting Container Details

Objective: Learn how to inspect container metadata.

## Step 1: Run a new container
```
podman run -d --name nginx_container nginx
```

## Explanation:

-d: Runs in detached mode

nginx: Official Nginx image

## Step 2: Inspect the container
```
podman inspect nginx_container
```

## Expected Output: Detailed JSON output showing container configuration, network settings, and state.

# Step 3: Clean up
```
podman stop nginx_container && podman rm nginx_container
```
# Conclusion

- In this lab, you learned essential Podman CLI commands for container lifecycle management, including:

- Listing containers (podman ps, podman ps -a)

- Running containers (podman run)

- Stopping, restarting, and removing containers (podman stop, podman restart, podman rm)

- Inspecting container details (podman inspect)

These skills are foundational for working with containers in Red Hat OpenShift and other containerized environments.
