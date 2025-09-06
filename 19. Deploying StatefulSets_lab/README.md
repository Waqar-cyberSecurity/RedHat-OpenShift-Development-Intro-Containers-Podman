# Lab 19: Deploying StatefulSets
## 🎯 Objectives
By the end of this lab, you will be able to:

- Understand the purpose of StatefulSets in Kubernetes/OpenShift.

- Create and deploy a StatefulSet manifest.

- Verify persistent storage and stable network identity for stateful applications.

## 📌 Prerequisites

- A running Kubernetes/OpenShift cluster (Minikube, Kind, or CRC for local testing).

- kubectl or oc CLI installed & configured.

- Basic knowledge of Pods, PVCs, Services.

- A StorageClass configured (e.g., standard in Minikube).

---

# 🛠️ Task 1: Write a StatefulSet Manifest
## Subtask 1.1: Create Manifest File

Create a file:

```
nano mysql-statefulset.yaml
```
Add content:
yaml
```
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mysql
spec:
  serviceName: "mysql"
  replicas: 2
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
      - name: mysql
        image: mysql:8.0
        env:
        - name: MYSQL_ROOT_PASSWORD
          value: "password"
        ports:
        - containerPort: 3306
          name: mysql
        volumeMounts:
        - name: mysql-persistent-storage
          mountPath: /var/lib/mysql
  volumeClaimTemplates:
  - metadata:
      name: mysql-persistent-storage
    spec:
      accessModes: [ "ReadWriteOnce" ]
      storageClassName: "standard"
      resources:
        requests:
          storage: 1Gi
```

## 🔎 Key Concepts:

- volumeClaimTemplates: Dynamically creates PVCs per Pod.

- serviceName: Ties Pods to a headless Service → stable DNS hostnames.

✅ Verify syntax (dry run):
```
kubectl apply -f mysql-statefulset.yaml --dry-run=client
```
---

# 🛠️ Task 2: Deploy the Stateful Application
## Subtask 2.1: Apply StatefulSet
```
kubectl apply -f mysql-statefulset.yaml
```
## Subtask 2.2: Create Headless Service
File: mysql-service.yaml

yaml
```
apiVersion: v1
kind: Service
metadata:
  name: mysql
spec:
  clusterIP: None
  ports:
  - port: 3306
    name: mysql
  selector:
    app: mysql
Apply:
```
- Save as mysql-service.yaml and apply:

```
kubectl apply -f mysql-service.yaml
```
 # Hands-On Expected Output:

<img src="lab 19 .png" alt="GitHub Banner" width="100%" />



## Subtask 2.3: Verify Resources
```
kubectl get statefulset,pods,pvc
```
Expected:
```
pgsql

NAME                     READY   AGE
statefulset.apps/mysql   2/2     1m

NAME                                STATUS   VOLUME   CAPACITY
pvc/mysql-persistent-storage-mysql-0   Bound   pvc-123...   1Gi
pvc/mysql-persistent-storage-mysql-1   Bound   pvc-456...   1Gi
```
---

# 🛠️ Task 3: Verify Storage & Network Identity
## Subtask 3.1: Check Pod Hostnames
```
kubectl get pods -l app=mysql -o wide
```
Pods should appear as:

- mysql-0

- mysql-1

## Subtask 3.2: Test Data Persistence
Write data:

```
kubectl exec -it mysql-0 -- mysql -uroot -ppassword -e "CREATE DATABASE lab_test;"
```
Delete Pod:

```
kubectl delete pod mysql-0
```
Verify data still exists:

```
kubectl exec -it mysql-0 -- mysql -uroot -ppassword -e "SHOW DATABASES;"
```
Expected: lab_test database should be listed.

## Subtask 3.3: Verify DNS Resolution
Run a temporary test Pod:

```
kubectl run -it --rm --image=busybox:1.28 test --restart=Never -- nslookup mysql
```
Expected: DNS resolves →

- mysql-0.mysql

- mysql-1.mysql

 # Hands-On Expected Output:

<img src="lab 19.2 .png" alt="GitHub Banner" width="100%" />



---

## 🛠️ Troubleshooting

- PVC Pending →

```
kubectl get storageclass
```
In Minikube:
```
minikube addons enable storage-provisioner
```
Pod CrashLoopBackOff →
```
kubectl logs mysql-0
```
Ensure MYSQL_ROOT_PASSWORD is correct.

---

# 🏁 Conclusion

In this lab, you:

- Created a StatefulSet manifest with PVC templates.

- Deployed a stateful MySQL app with persistent storage.

- Verified stable DNS identities + data persistence.

✅ StatefulSets are essential for apps needing:

- Stable, unique network names.

- Ordered deployment & scaling.

- Persistent storage tied to Pod lifecycle.

---

## 🧹 Cleanup
```
kubectl delete -f mysql-statefulset.yaml -f mysql-service.yaml
```
