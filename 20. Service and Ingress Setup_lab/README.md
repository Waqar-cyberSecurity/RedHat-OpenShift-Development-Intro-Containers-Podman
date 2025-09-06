# Lab 20: Service and Ingress Setup
## 🎯 Objectives
By the end of this lab, you will be able to:

- Expose applications internally using ClusterIP and NodePort services.

- Configure OpenShift Route or Kubernetes Ingress for external access.

- Test and validate application accessibility.

## 📌 Prerequisites
- Running Kubernetes or OpenShift cluster.

- kubectl or oc CLI installed & configured.

- Basic understanding of Pods, Deployments, and Services.

- A sample application deployed (e.g., Nginx).

---

# 🛠️ Task 1: Create ClusterIP and NodePort Services
## Subtask 1.1: Deploy a Sample Application
```
kubectl create deployment nginx --image=nginx:latest
```
Expected:

```
deployment.apps/nginx created
```
Verify:

```
kubectl get deployments
kubectl get pods
```
✅ Deployment and Pods should be Running.

## Subtask 1.2: Expose Application Using ClusterIP
```
kubectl expose deployment nginx --port=80 --type=ClusterIP --name=nginx-clusterip
kubectl get svc nginx-clusterip
```
Expected Output:
```
pgsql

NAME              TYPE        CLUSTER-IP      PORT(S)   AGE
nginx-clusterip   ClusterIP   10.96.123.45    80/TCP    10s
```
📌 ClusterIP → Internal-only access inside the cluster.

## Subtask 1.3: Expose Application Using NodePort
```
kubectl expose deployment nginx --port=80 --type=NodePort --name=nginx-nodeport
kubectl get svc nginx-nodeport
```
Expected Output:
```
pgsql

NAME             TYPE       CLUSTER-IP      PORT(S)        AGE
nginx-nodeport   NodePort   10.96.234.56    80:30007/TCP   5s
```
Test access:

```
curl http://<NODE_IP>:30007
```
Replace <NODE_IP> with your cluster node’s IP.

✅ You should see Nginx welcome page.

---

# 🛠️ Task 2: Configure External Access
## Subtask 2.1: OpenShift Route (for OpenShift Users)
```
oc expose svc nginx-nodeport --name=nginx-route
oc get route nginx-route
```
Expected Output:
```
pgsql

NAME          HOST/PORT                          PATH   SERVICES        PORT   TERMINATION   WILDCARD
nginx-route   nginx-route-default.example.com          nginx-nodeport   80                   None
```
Test:

```
curl http://nginx-route-default.example.com
```
## Subtask 2.2: Kubernetes Ingress (for Kubernetes Users)
Deploy NGINX Ingress Controller (if not running):

```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml
```
Create an Ingress resource:

```
cat <<EOF | kubectl apply -f -
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: nginx-ingress
spec:
  rules:
  - host: nginx.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: nginx-nodeport
            port:
              number: 80
EOF
```
Verify Ingress:

```
kubectl get ingress nginx-ingress
```
Expected:
```
nginx

NAME            CLASS   HOSTS               ADDRESS        PORTS   AGE
nginx-ingress   <none>  nginx.example.com   192.168.1.10   80      10s
```
Update /etc/hosts:

```
echo "<INGRESS_IP> nginx.example.com" | sudo tee -a /etc/hosts
```
Test access:

```
curl http://nginx.example.com
```
✅ Nginx welcome page should appear.

 # Hands-On Expected Output:

<img src="lab 20.3 .png" alt="GitHub Banner" width="100%" />


---

## 🛠️ Troubleshooting

- NodePort not accessible →

- Check firewall on worker node.

- Inspect service details:

```
kubectl describe svc nginx-nodeport
```
Ingress issues →

Check Ingress controller is running:

```
kubectl get pods -n ingress-nginx
```
Check logs:

```
kubectl logs -n ingress-nginx <ingress-pod>
```
---

# 🏁 Conclusion

In this lab, you learned to:

- Expose apps internally with ClusterIP.

- Use NodePort for static external access.

- Configure Route (OpenShift) or Ingress (Kubernetes) for hostname-based routing.

📌 These are core building blocks for making apps accessible inside and outside the cluster
