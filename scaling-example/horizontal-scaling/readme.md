## **📜 Horizontal Scaling README (`horizontal-scaling/README.md`)**

````markdown
# 🚀 Horizontal Scaling in Kubernetes

## **📌 What is Horizontal Scaling?**

Horizontal scaling increases the **number of pod replicas** to distribute workload across multiple instances.  
This is useful when your application experiences **high traffic** and needs to handle more requests efficiently.

---

## **📜 Deployment File: `frontend-deployment-horizontal.yaml`**

This deployment is set to run **4 replicas** of the frontend application.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend-deployment
  labels:
    app: frontend
spec:
  replicas: 4 # Adjust the number of replicas as needed
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
```
````

---

## **🔧 How to Apply and Scale Horizontally**

1. Apply the deployment:
   ```sh
   kubectl apply -f frontend-deployment-horizontal.yaml
   ```
2. Check running pods:
   ```sh
   kubectl get pods
   ```
3. Manually scale to **6 replicas**:
   ```sh
   kubectl scale deployment frontend-deployment --replicas=6
   ```
4. Verify the scaling:
   ```sh
   kubectl get pods
   ```

---

## **📈 Benefits of Horizontal Scaling**

✅ Handles more user traffic  
✅ Provides **load balancing** across multiple pods  
✅ Improves **fault tolerance** (if one pod crashes, others remain active)  
✅ Best for **stateless applications** like web servers

---

## **🛠 Next Steps**

- Automate scaling using **Horizontal Pod Autoscaler (HPA)**
- Implement **Load Balancing** to distribute traffic evenly
- Use **Kustomize** to manage different scaling configurations
