# 💾 Lab 12: Backup and Restore Data with Podman
## 🎯 Objectives

  - Implement backup strategies for stateful containers

  - Perform database dumps inside containers

  - Store dumps on persistent volumes

  - Restore data from backup dumps

## 🛠 Prerequisites

 - Podman (v3.0+) installed

 - Linux system or VM with internet access

 - Basic container knowledge

---

# ⚙️ Lab Setup

✅ Verify Podman installation:

```
podman --version
```
✅ Create working directory:

```
mkdir -p ~/backup-lab && cd ~/backup-lab
```

---

# 📌 Task 1: Perform Database Dumps Inside Container

## 🔹 Subtask 1.1: Create MySQL Container

```
podman run -d --name mysql-db \
  -e MYSQL_ROOT_PASSWORD=redhat \
  -e MYSQL_DATABASE=testdb \
  -e MYSQL_USER=testuser \
  -e MYSQL_PASSWORD=testpass \
  docker.io/library/mysql:8.0
```

➡️ Starts a MySQL container with credentials and testdb database.

✅ Verify:

```
podman ps -f name=mysql-db
```
## 🔹 Subtask 1.2: Create Sample Data

Access MySQL:

```
podman exec -it mysql-db mysql -u root -predhat
```
Inside MySQL:
```
USE testdb;
CREATE TABLE employees (id INT AUTO_INCREMENT PRIMARY KEY, name VARCHAR(50));
INSERT INTO employees (name) VALUES ('John Doe'), ('Jane Smith');
SELECT * FROM employees;
exit;
```
➡️ Confirms data insertion.

## 🔹 Subtask 1.3: Create Database Dump

```
podman exec mysql-db /usr/bin/mysqldump -u root -predhat testdb > testdb_dump.sql
```
✅ Verify:

```
ls -l testdb_dump.sql
head -n 5 testdb_dump.sql
```
# Hands-On Expected Output;

<img src="lab 12 .png" alt="GitHub Banner" width="100%" />



---

# 📌 Task 2: Store Dumps on Volumes
## 🔹 Subtask 2.1: Create Persistent Volume
```
podman volume create backup-vol
```
## 🔹 Subtask 2.2: Copy Dump to Volume
Create temporary container with volume:

```
podman run -it --rm -v backup-vol:/backup alpine sh
```
Inside container:

sh
```
mkdir -p /backup/mysql
exit
```
Copy dump file:

```
podman cp testdb_dump.sql $(podman create --name temp -v backup-vol:/backup alpine):/backup/mysql/
podman rm temp
```
✅ Verify:

```
podman run --rm -v backup-vol:/backup alpine ls -l /backup/mysql
```

# Hands-On Expected Output;




---

# 📌 Task 3: Restore Data from Dumps
## 🔹 Subtask 3.1: Create New MySQL Container
```
podman run -d --name mysql-restore \
  -e MYSQL_ROOT_PASSWORD=redhat \
  -e MYSQL_DATABASE=testdb \
  -e MYSQL_USER=testuser \
  -e MYSQL_PASSWORD=testpass \
  docker.io/library/mysql:8.0
```
## 🔹 Subtask 3.2: Restore Data
Copy dump file back:
```
podman cp $(podman create --name temp -v backup-vol:/backup alpine):/backup/mysql/testdb_dump.sql . 
podman rm temp
```
Restore:

```
podman exec -i mysql-restore mysql -u root -predhat testdb < testdb_dump.sql
```

## 🔹 Subtask 3.3: Verify Restoration
```
podman exec -it mysql-restore mysql -u root -predhat -e "SELECT * FROM testdb.employees;"
```

✅ Expected Output:

```
pgsql

+----+------------+
| id | name       |
+----+------------+
|  1 | John Doe   |
|  2 | Jane Smith |
+----+------------+
```
# Hands-On Expected Output;

<img src="lab 12 .2.png" alt="GitHub Banner" width="100%" />

---

## 🛠 Troubleshooting Tips

Check logs if container fails:

```
podman logs mysql-db
```

 - Fix volume permission issues (SELinux):

```
-v backup-vol:/backup:Z
```

 - If dump fails, recheck MySQL credentials & DB name.

---

# ✅ Conclusion

✔️ Created a MySQL DB inside container

✔️ Generated database dump for backup

✔️ Stored dump on persistent volume

✔️ Successfully restored data into a new container


📌 These backup & restore skills are critical for data integrity in containerized apps and valuable for Red Hat OpenShift certification prep.

## 🧹 Cleanup
```
podman stop mysql-db mysql-restore
podman rm mysql-db mysql-restore
podman volume rm backup-vol
rm -f testdb_dump.sql
```
