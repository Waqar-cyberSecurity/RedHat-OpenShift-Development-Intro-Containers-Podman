# Lab 9: Using ENTRYPOINT and CMD in Containerfiles

## 🎯 Objectives
By the end of this lab, you will be able to:
- Understand the difference between `ENTRYPOINT` and `CMD` instructions  
- Configure container startup processes using `ENTRYPOINT` and `CMD`  
- Test container behavior with different command combinations  
- Override default commands at runtime  

---

## 🛠️ Prerequisites
- Podman or Docker installed (Podman recommended for OpenShift compatibility)  
- Basic understanding of Containerfile/Dockerfile syntax  
- Terminal or command-line access  

---

## 🔹 Lab Setup
Create a new directory for the lab:
```bash
mkdir entrypoint-cmd-lab && cd entrypoint-cmd-lab
```
---

# 🔹 Task 1: Writing a Containerfile with ENTRYPOINT and CMD
## Step 1: Create Containerfile
```
Dockerfile

# Base image
FROM registry.access.redhat.com/ubi9/ubi-minimal

# Set ENTRYPOINT to a shell script
ENTRYPOINT ["echo", "Entrypoint says:"]

# Set default CMD arguments
CMD ["Default CMD message"]
```

Explanation:

 - ENTRYPOINT = defines the main executable that runs when the container starts

 - CMD = provides default arguments passed to ENTRYPOINT

 - Together → echo "Entrypoint says:" "Default CMD message"

## Step 2: Build Image
```
podman build -t entrypoint-demo .
```
✅ Expected Output:

```
vbnet

STEP 1/3: FROM registry.access.redhat.com/ubi9/ubi-minimal
STEP 2/3: ENTRYPOINT ["echo", "Entrypoint says:"]
STEP 3/3: CMD ["Default CMD message"]
COMMIT entrypoint-demo
```
---


# 🔹 Task 2: Testing Container Behavior

Run with Default ENTRYPOINT and CMD

```
podman run --rm entrypoint-demo
```
✅ Output:
```
objectivec

Entrypoint says: Default CMD message
```
Override CMD at Runtime
```
podman run --rm entrypoint-demo "Custom message"
```
✅ Output:
```
vbnet

Entrypoint says: Custom message
```
👉 Key Concept: Arguments after the image name replace the CMD.

---

# 🔹 Task 3: Advanced ENTRYPOINT & CMD Combinations
## Step 1: Create a Script-Based ENTRYPOINT
greet.sh:

```
#!/bin/sh
echo "Welcome to $1 from $2"
```

Make it executable:

```
chmod +x greet.sh
```
Update Containerfile:

```
Dockerfile

FROM registry.access.redhat.com/ubi9/ubi-minimal

COPY greet.sh /usr/local/bin/
ENTRYPOINT ["/usr/local/bin/greet.sh"]
CMD ["OpenShift Lab", "Red Hat"]
```

Rebuild:
```
podman build -t greet-demo .
```

## Step 2: Test Script-Based Container
Default run:

```
podman run --rm greet-demo
```
✅ Output:
```
css

Welcome to OpenShift Lab from Red Hat
```
Override:

```

podman run --rm greet-demo "Container Workshop" "Instructor"
```
✅ Output:
```
css

Welcome to Container Workshop from Instructor
```
---

# 🔹 Task 4: Overriding ENTRYPOINT
Replace ENTRYPOINT with --entrypoint:

```
podman run --rm --entrypoint echo greet-demo "This completely replaces the ENTRYPOINT"
```
✅ Output:
```
nginx

This completely replaces the ENTRYPOINT
```
⚠️ Troubleshooting Tip: If you get permission errors, make sure the script has execute permissions (chmod +x greet.sh).

---

# 🔹 Task 5: Shell vs Exec Form
Example:
```
Dockerfile

FROM registry.access.redhat.com/ubi9/ubi-minimal

# Shell form (not recommended for most cases)
ENTRYPOINT echo "Shell form ENTRYPOINT:"
CMD echo "Shell form CMD"
```

## Key Difference:

 - Exec form (JSON array) → doesn’t use a shell, runs directly

 - Shell form → runs through /bin/sh -c

---

 # ✅ Conclusion

 - In this lab, you learned:

 - Difference between ENTRYPOINT and CMD

 - How to combine them for flexible container startup

 - Methods to override both at runtime

 - Why exec form is preferred for signal handling

## Best Practices:
 - Prefer exec form (["executable", "param"])

 - Use CMD for default arguments that can be overridden

 - Combine ENTRYPOINT with a script for complex startup logic

 - Always document expected runtime overrides
