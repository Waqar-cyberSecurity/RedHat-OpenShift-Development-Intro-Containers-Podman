# Lab 5: Managing Container Images
## Objectives
By the end of this lab, you will be able to:

 - Search for container images on Docker Hub

 - Pull container images to your local system

 - Inspect image metadata and layers

 - Remove container images from your system

--- 

# Prerequisites
 - Linux-based system (Ubuntu 20.04/22.04 recommended)

 - Podman installed (version 3.0+)

 - Internet connectivity

 - Basic familiarity with the command line

--- 

# Setup Instructions
Install Podman if not already installed:
```
sudo apt-get update
sudo apt-get install -y podman
```
Verify installation:
```
podman --version
```

---

# Task 1: Search for Images on Docker Hub
## Subtask 1.1: Search for an Image
Search for Ubuntu images on Docker Hub:

```
podman search ubuntu
```
Filter results to only official images:

```
podman search --filter=is-official=true ubuntu
```
## ✅ Expected Output:
A list of Ubuntu images with details like NAME, DESCRIPTION, STARS, and OFFICIAL status.

## ⚠️ Troubleshooting:
If you get rate-limited by Docker Hub, log in:

```
podman login docker.io
```
---

# Task 2: Pull Container Images
## Subtask 2.1: Pull a Specific Image
Pull the official Ubuntu latest image:

```
podman pull docker.io/library/ubuntu:latest
```
Verify:

```
podman images
```
## ✅ Expected Output:
The Ubuntu image appears in your local list with details like REPOSITORY, TAG, IMAGE ID, and SIZE.

## Subtask 2.2: Pull with a Specific Tag
Pull an older Ubuntu version:

```
podman pull docker.io/library/ubuntu:20.04
```
## Check again:

```
podman images
```
💡 Key Concept: Tags allow you to manage different versions of the same image.

---

# Task 3: Inspect Image Metadata
## Subtask 3.1: View Basic Information
Inspect the Ubuntu image:

```
podman inspect docker.io/library/ubuntu:latest
```
✅ Expected Output: JSON-formatted metadata including architecture, environment variables, and layers.

## Subtask 3.2: Extract Specific Information
Get only the environment variables:

```
podman inspect --format "{{.Config.Env}}" docker.io/library/ubuntu:latest
```
💡 Key Concept: Image inspection helps you understand configurations before deployment.

---

# Task 4: Remove Container Images
## Subtask 4.1: Remove a Single Image
Delete the Ubuntu 20.04 image:

```
podman rmi docker.io/library/ubuntu:20.04
```
Verify:

```
podman images
```
## Subtask 4.2: Remove All Unused Images
Clean up unused images:

```
podman image prune -a
```
⚠️ If an image is in use, force remove:

```
podman rmi -f IMAGE_ID
```
---

# Conclusion
In this lab, you have:

✔️ Searched for container images on Docker Hub using Podman

✔️ Pulled specific image versions to your system

✔️ Inspected detailed metadata about container images

✔️ Removed unused images to free up space


These skills are fundamental for managing containerized applications efficiently. 🚀
