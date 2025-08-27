# Lab 4: Creating and Managing Pods with Podman

## 🎯 Goal of this Lab
In this lab, you will learn how to:
- Understand the concept of **Pods** in container orchestration  
- Create and manage Pods using **Podman**  
- Run **multiple containers** within a single Pod  
- Inspect **pod networking** and **shared volumes**  

---

## 📌 Prerequisites
- Linux system (Ubuntu 20.04+ or CentOS 8+)  
- Podman installed  
  ```bash
  sudo apt-get install podman   # On Ubuntu/Debian  
  sudo yum install podman       # On CentOS/RHEL  


# 🧪 Task 1: Creating a Pod
## 🔹 Subtask 1.1: Create a Basic Pod

```
podman pod create --name demo-pod -p 8080:80
```
 👉 Explanation:
```
--name demo-pod → assigns a name to the pod

-p 8080:80 → maps host port 8080 to pod port 80
```
Verify pod creation:
```
podman pod list
```
## 🔹 Subtask 1.2: Add a Container to the Pod

Run Nginx container inside the pod:
```
podman run -d --pod demo-pod --name nginx-container docker.io/library/nginx:alpine
```
👉 Explanation:
```
--pod demo-pod → attaches the container to the existing pod

--name nginx-container → gives a custom name
```
## Check:
```
podman ps --pod
```
# 🧪 Task 2: Running Multiple Containers in a Pod

## 🔹 Subtask 2.1: Add a Sidecar Container

Run Redis in the same pod:

```
podman run -d --pod demo-pod --name redis-container docker.io/library/redis:alpine
```
Verify both containers:

```
podman pod inspect demo-pod | jq '.Containers[].Names'
```
# 🔹 Subtask 2.2: Verify Shared Network

Exec into Nginx container:

```
podman exec -it nginx-container sh
```
Ping Redis container:

```
ping redis-container
```
### 👉 Key Concept: Containers in the same pod share the network namespace, so they can talk via localhost.

# 🧪 Task 3: Inspecting Pod Networking and Volumes
## 🔹 Subtask 3.1: Network Inspection

Check pod network details:

```
podman pod inspect demo-pod | jq '.InfraConfig.NetworkOptions'
```
View port mappings:

```
podman port demo-pod
```
# 🔹 Subtask 3.2: Create Shared Volume

Create a volume:

```
podman volume create shared-vol
```

Run containers with shared volume:

```
podman run -d --pod demo-pod --name nginx2 -v shared-vol:/data docker.io/library/nginx:alpine
podman run -d --pod demo-pod --name redis2 -v shared-vol:/data docker.io/library/redis:alpine
```
Verify sharing:

```
podman exec -it nginx2 touch /data/testfile
podman exec -it redis2 ls /data 
```
### 👉 Both containers should see testfile.


# 🛠 Troubleshooting Tips

## Check logs if container fails:

```
podman logs <container_name>
```
For network issues, verify firewall:

```
sudo firewall-cmd --list-ports
```
Clean failed pods:

```
podman pod rm -f demo-pod
```

# ✅ Conclusion

* In this lab you have:

* Created and managed Pods with Podman

* Deployed multi-container pods with shared networking

* Used volumes for data persistence

* Inspected networking and pod structure

# 🧹 Cleanup

```
podman pod rm -f demo-pod
podman volume rm shared-vol

```
