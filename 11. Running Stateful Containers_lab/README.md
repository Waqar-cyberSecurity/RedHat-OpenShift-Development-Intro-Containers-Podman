# Lab 11: Running Stateful Containers with Podman

## 🎯 Objectives

 - Understand and implement persistent storage for stateful containers

 - Run MySQL & PostgreSQL with volume mounts

 - Verify data persistence across container lifecycle

## 🛠 Prerequisites

 - Podman (preferred) or Docker installed

 - Basic command line proficiency

 - At least 2GB free disk space

 - Internet access to pull images

---

## ⚙️ Lab Setup

✅ Verify Podman installation:
```

podman --version
```
➡️ Shows Podman version (e.g., 4.0.0)

✅ Create working directory:

```
mkdir -p ~/stateful-lab && cd ~/stateful-lab
```

---

# 📌 Task 1: Running MySQL with Persistent Storage
## 🔹 Subtask 1.1: Create Volume Directory
```
mkdir -p mysql-data
```
➡️ mysql-data will store MySQL database files on the host machine.

## 🔹 Subtask 1.2: Launch MySQL Container
```
podman run -d \
  --name mysql-db \
  -e MYSQL_ROOT_PASSWORD=redhat123 \
  -e MYSQL_DATABASE=testdb \
  -e MYSQL_USER=testuser \
  -e MYSQL_PASSWORD=user123 \
  -v $(pwd)/mysql-data:/var/lib/mysql \
  -p 3306:3306 \
  docker.io/library/mysql:8.0
```
# Hands-On Expected Output;
<img src="lab 11.png" alt="GitHub Banner" width="100%" />

Key Parameters:

- -v $(pwd)/mysql-data:/var/lib/mysql → mounts host directory

- MYSQL_ROOT_PASSWORD → sets root password

- MYSQL_DATABASE, MYSQL_USER, MYSQL_PASSWORD → initializes DB

✅ Verify container:

```
podman ps -a --format "table {{.ID}}\t{{.Names}}\t{{.Status}}"
```
## 🔹 Subtask 1.3: Test Database
Enter MySQL shell:

```
podman exec -it mysql-db mysql -u testuser -puser123 testdb
```
Inside MySQL:
```
sql

CREATE TABLE lab_data (id INT AUTO_INCREMENT PRIMARY KEY, message VARCHAR(255));
INSERT INTO lab_data (message) VALUES ('Persistent test data');
SELECT * FROM lab_data;
exit;
```
➡️ Confirms database and table creation.

# Hands-On Expected Output;
<img src="lab 11.1 .png" alt="GitHub Banner" width="100%" />


---


#  📌 Task 2: Verifying Data Persistence
##  🔹 Subtask 2.1: Remove Container
```
podman stop mysql-db
podman rm mysql-db
```
## 🔹 Subtask 2.2: Recreate Container with Same Volume
```
podman run -d \
  --name mysql-db \
  -e MYSQL_ROOT_PASSWORD=redhat123 \
  -v $(pwd)/mysql-data:/var/lib/mysql \
  -p 3306:3306 \
  docker.io/library/mysql:8.0
```
## 🔹 Subtask 2.3: Verify Data Survival
```
podman exec -it mysql-db mysql -u testuser -puser123 testdb -e "SELECT * FROM lab_data;"
```
➡️ Should still display Persistent test data.

---

# 📌 Task 3: PostgreSQL (Optional Challenge)
## 🔹 Subtask 3.1: Create Volume Directory
```
mkdir -p pg-data
```
# Hands-On Expected Output;
<img src="lab 11.2 .png" alt="GitHub Banner" width="100%" />

## 🔹 Subtask 3.2: Launch PostgreSQL
```
podman run -d \
  --name postgres-db \
  -e POSTGRES_PASSWORD=redhat123 \
  -e POSTGRES_USER=testuser \
  -e POSTGRES_DB=testdb \
  -v $(pwd)/pg-data:/var/lib/postgresql/data \
  -p 5432:5432 \
  docker.io/library/postgres:13
```
## 🔹 Subtask 3.3: Test Data Persistence
```
podman exec -it postgres-db psql -U testuser -d testdb -c "CREATE TABLE lab_data (id SERIAL PRIMARY KEY, message TEXT); INSERT INTO lab_data (message) VALUES ('Postgres persistent data');"
```
➡️ Remove/recreate container to confirm persistence.



---

# 🛠 Troubleshooting Tips

- Permission Issues

```
sudo chown -R 1001:1001 mysql-data/
```

- Port Conflicts

```
ss -tulnp | grep 3306
```
- View Container Logs

```
podman logs mysql-db
```

# Hands-On Expected Output;
<img src="lab 11.3 .png" alt="GitHub Banner" width="100%" />

---

# ✅ Conclusion

✔️ Demonstrated persistent storage with MySQL & PostgreSQL

✔️ Verified data persistence after container removal

✔️ Understood volume mounting for stateful apps

✔️ Learned real-world database container setup

## 📖 Knowledge Check

- What happens if you omit the -v flag?

- How can you migrate persistent data to another host?

 - What are security risks of storing DB files directly on the host?
