# Lab 6: Building Custom Container Images
## Objectives
By the end of this lab, you will be able to:

 - Understand the structure of a Containerfile (Dockerfile alternative for Podman).

 - Use key Containerfile instructions (FROM, RUN, COPY, ENV) to build a custom image.

 - Build, tag, and run a container image using Podman.

--- 

# Prerequisites
- A Linux-based environment (Fedora, RHEL, Ubuntu, etc.) with Podman installed.

- Basic familiarity with the command line.

Verify Podman Installation
```
podman --version
```
Expected Output:

```
podman version 4.x.x
```
If not installed, run:

```
# Fedora/RHEL
sudo dnf install podman  

# Ubuntu/Debian
sudo apt-get install podman
```
--- 

# Lab Tasks
## Task 1: Create a Simple Containerfile
Step 1: Create a Project Directory

```
mkdir custom-nginx && cd custom-nginx
```

## Step 2: Create a Custom HTML File

```
echo "<h1>Welcome to My Custom Nginx Container!</h1>" > index.html
```
## Step 3: Write the Containerfile
Create a file named Containerfile (or Dockerfile) with the following content:
```
dockerfile

# Use the official Nginx base image
FROM docker.io/nginx:alpine

# Set an environment variable
ENV AUTHOR="OpenShift Developer"

# Copy the custom HTML file to the Nginx web root
COPY index.html /usr/share/nginx/html

# Run a command to print a message (for demonstration)
RUN echo "Container built by $AUTHOR" > /build-info.txt
```
---

# 🔑 Key Concepts:

  - FROM → Specifies the base image (Nginx Alpine).

  - ENV → Defines an environment variable.

  - COPY → Transfers files from host → container.

  - RUN → Executes commands at build time.

---

# Task 2: Build and Tag the Image
## Step 1: Build the Image

```
podman build -t my-custom-nginx .

```
 -  -t → assigns a tag (my-custom-nginx).

-   . → current directory as build context.

## Expected Output:

```

STEP 1/4: FROM docker.io/nginx:alpine
STEP 2/4: ENV AUTHOR="OpenShift Developer"
STEP 3/4: COPY index.html /usr/share/nginx/html
STEP 4/4: RUN echo "Container built by $AUTHOR" > /build-info.txt
COMMIT my-custom-nginx
```

## Step 2: Verify the Image
```
podman images
```
Expected Output:

```
REPOSITORY               TAG       IMAGE ID       CREATED        SIZE
localhost/my-custom-nginx latest    xxxxxxxxxxxx   1 minute ago   25.4 MB
```

---

# Task 3: Run and Test the Container
## Step 1: Run the Container

```
podman run -d -p 8080:80 my-custom-nginx
```
- -d → detached mode.

- -p 8080:80 → maps host port 8080 → container port 80.

Check running containers:

```
podman ps
```
# Step 2: Test the Web Server
Open a browser:

```
http://localhost:8080
```

## ✅ Expected Output:

Welcome to My Custom Nginx Container!

---

# Conclusion

 - In this lab, you learned how to:


 - Write a Containerfile with essential instructions.


 - Build and tag a custom image with Podman.


 - Run and test a containerized web server.

🎯 These are the foundations of container image creation, which can be extended for application deployment in production environments.
