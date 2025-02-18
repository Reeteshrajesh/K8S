# 🚀 Vertical Scaling in Kubernetes

## **📌 What is Vertical Scaling?**

Vertical scaling increases **CPU and memory resources** for a single pod.  
This is useful when an application needs **more processing power** rather than additional replicas.

---

## **📜 Deployment File: `frontend-deployment-vertical.yaml`**

This deployment sets **minimum and maximum CPU/memory limits** for the frontend pod.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend-deployment
  labels:
    app: frontend
spec:
  replicas: 2 # Keeping replicas fixed, focusing on resource scaling
  selector:
    matchLabels:
      app: frontend
  template:
    metadata:
      labels:
        app: frontend
    spec:
      containers:
        - name: frontend
          image: <your-dockerhub-username>/frontend:latest # Replace with your Docker image
          ports:
            - containerPort: 3000
          resources:
            requests:
              cpu: "500m" # Minimum 0.5 CPU
              memory: "512Mi" # Minimum 512MB RAM
            limits:
              cpu: "1" # Maximum 1 CPU
              memory: "1Gi" # Maximum 1GB RAM
```

---

## **🔧 How to Apply and Verify Vertical Scaling**

1. Apply the deployment:
   ```sh
   kubectl apply -f frontend-deployment-vertical.yaml
   ```
2. Check pod resource usage:
   ```sh
   kubectl describe pod <pod-name>
   ```
3. Monitor CPU and memory usage:
   ```sh
   kubectl top pod
   ```

---

## **📈 Benefits of Vertical Scaling**

✅ Increases **CPU & memory** allocation per pod  
✅ Ideal for **CPU-intensive** and **memory-heavy** applications  
✅ Works well for **stateful applications**  
✅ Can optimize **performance without increasing pod count**

---

## **🛠 Next Steps**

- Use **Vertical Pod Autoscaler (VPA)** to dynamically adjust resources
- Monitor resource usage with **Prometheus & Grafana**
- Combine **vertical and horizontal scaling** for optimized performance
