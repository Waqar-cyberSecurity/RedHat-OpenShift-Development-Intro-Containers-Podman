#  Red Hat OpenShift Development – Container Labs Summary

## Introduction
This repository provides a complete overview of containerization concepts and hands-on practice using **Podman, Docker, and Kubernetes/OpenShift**.  
It contains a collection of 20 labs, each focusing on a different aspect of container development, debugging, storage, networking, and orchestration.  
These labs serve as a strong foundation for **DevOps practices, OpenShift certification preparation, and real-world cloud-native development**.

---

## Labs Overview

- **Lab 1: Introduction to Containers** – Basic understanding of what containers are and why they are important.  
- **Lab 2: Podman Basics** – Running simple containers and managing lifecycle using Podman.  
- **Lab 3: Building Container Images** – Creating custom images with Containerfiles.  
- **Lab 4: Running Multiple Containers** – Managing multiple containers and their interactions.  
- **Lab 5: Container Image Registries** – Pulling, pushing, and managing images in registries.  
- **Lab 6: Resource Management** – Limiting CPU and memory usage in containers.  
- **Lab 7: User and Permissions in Containers** – Handling security and user roles inside containers.  
- **Lab 8: Environment Variables in Images** – Defining and overriding environment variables at runtime.  
- **Lab 9: ENTRYPOINT and CMD** – Controlling container startup behavior.  
- **Lab 10: Persisting Data with Volumes** – Using volumes and bind mounts for persistent storage.  
- **Lab 11: Running Stateful Containers** – Running databases like MySQL/PostgreSQL with persistent storage.  
- **Lab 12: Backup and Restore Data** – Creating database dumps and restoring data in containers.  
- **Lab 13: Troubleshooting Containers** – Using logs, inspect, and exec for debugging issues.  
- **Lab 14: Networking in Containers** – Exploring Podman networks, port mapping, and custom networks.  
- **Lab 15: Remote Debugging** – Attaching IDEs like VS Code to running containers for live debugging.  
- **Lab 16: Compose Basics** – Running multi-container applications with Podman Compose.  
- **Lab 17: Compose with Dependencies** – Managing service dependencies and scaling with Docker Compose.  
- **Lab 18: Kubernetes Pod Deployment** – Creating and deploying basic Pods using YAML manifests.  
- **Lab 19: StatefulSets in Kubernetes** – Deploying stateful applications with persistent volumes.  
- **Lab 20: Services and Ingress Setup** – Exposing applications internally and externally with ClusterIP, NodePort, and Ingress.  

---

## Importance
These labs provide practical experience with container technologies that are widely used in the **IT industry**.  
They cover everything from the **basics of Podman/Docker** to advanced concepts like **Kubernetes StatefulSets and Ingress**, making them highly valuable for anyone preparing for cloud-native development and **Red Hat OpenShift certification**.

---

## Industry Relevance
- **DevOps Pipelines**: Automating build, test, and deployment workflows.  
- **Cloud-Native Applications**: Running microservices in production.  
- **Enterprise OpenShift Use**: Managing containers securely at scale.  
- **Database Persistence**: Ensuring stateful workloads survive container restarts.  

---

## Troubleshooting Quick Notes
- Use `podman logs <container>` or `kubectl logs <pod>` to check errors.  
- Verify volumes and permissions if data is missing.  
- Check firewall rules and port mappings when services are not accessible.  
- Inspect Pod details with `kubectl describe pod <name>` if status is Pending or CrashLoopBackOff.  
- Ensure Ingress or Routes are properly configured for external access.  

---


## 🤝 Contributing
- Feel free to fork this repository, make improvements, and submit pull requests.

---

## 📜 License
- This labs is licensed under the MIT License – see the LICENSE file for details.
