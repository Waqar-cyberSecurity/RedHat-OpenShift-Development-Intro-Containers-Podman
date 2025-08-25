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
