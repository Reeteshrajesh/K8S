# **🚀 Three-Tier Application on Kubernetes**

This project deploys a **three-tier application** (Frontend, Backend, Database) on **Kubernetes** using **Kustomize**.

✅ **Frontend:** React application (Node.js server)  
✅ **Backend:** Node.js API server  
✅ **Database:** MongoDB running as a StatefulSet  
✅ **Ingress Controller:** Exposes services via NGINX Ingress

---

## **📂 Project Structure**

```
three-tier-app/
│── k8s/
│   ├── frontend/
│   │   ├── deployment.yaml
│   │   ├── service.yaml
│   ├── backend/
│   │   ├── deployment.yaml
│   │   ├── service.yaml
│   ├── mongodb/
│   │   ├── statefulset.yaml
│   │   ├── service.yaml
│   │   ├── secret.yaml
│   ├── ingress.yaml
│   ├── kustomization.yaml   # (For Kustomize)
│── Dockerfile-frontend
│── Dockerfile-backend
│── .env (Optional)
│── README.md
```

---

## **🚀 Prerequisites**

Before deploying, ensure you have:

- **Docker** 🐳 Installed
- **Kubernetes Cluster** (Minikube, KIND, EKS, etc.)
- **kubectl** CLI Installed
- **Kustomize** Installed (`kubectl apply -k` supports it)

---

## **🔧 Setup & Deployment**

### **1️⃣ Build Docker Images**

First, build and tag images for **frontend** and **backend**:

```sh
docker build -t frontend-image:latest -f Dockerfile-frontend .
docker build -t backend-image:latest -f Dockerfile-backend .
```

### **2️⃣ Push Images to Docker Hub (or Private Registry)**

Replace `<your-dockerhub-username>` with your actual username:

```sh
docker tag frontend-image:latest <your-dockerhub-username>/frontend:latest
docker tag backend-image:latest <your-dockerhub-username>/backend:latest

docker push <your-dockerhub-username>/frontend:latest
docker push <your-dockerhub-username>/backend:latest
```

### **3️⃣ Deploy the Application**

Run the following command to deploy all services:

```sh
kubectl apply -k k8s/
```

### **4️⃣ Verify Deployments**

Check if pods, services, and ingress are running:

```sh
kubectl get pods
kubectl get svc
kubectl get ingress
```

### **5️⃣ Access the Application**

Get the external IP of the frontend service:

```sh
kubectl get svc frontend-service
```

Visit the **LoadBalancer IP** in your browser.  
If using **Ingress**, update your `/etc/hosts` file with:

```
<EXTERNAL_IP> your-domain.com
```

Then visit `http://your-domain.com`.

---

## **🛠️ Managing the Application**

### **📌 Scaling Pods**

To scale the frontend to **4 replicas**:

```sh
kubectl scale deployment frontend-deployment --replicas=4
```

To check if scaling worked:

```sh
kubectl get pods
```

### **📌 Viewing Logs**

Check logs for the backend service:

```sh
kubectl logs -f deployment/backend-deployment
```

### **📌 Deleting the Application**

```sh
kubectl delete -k k8s/
```

---

## **📝 Frontend Dockerfile (`Dockerfile-frontend`)**

```dockerfile
# Use Node.js as the base image
FROM node:18-alpine AS build

# Set working directory
WORKDIR /app

# Copy package.json and install dependencies
COPY package.json package-lock.json ./
RUN npm install --frozen-lockfile

# Copy the entire frontend source code
COPY . .

# Build the React app
RUN npm run build

# Use Nginx to serve the static files
FROM nginx:alpine
COPY --from=build /app/build /usr/share/nginx/html

# Expose the port
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]
```

---

## **📝 Backend Dockerfile (`Dockerfile-backend`)**

```dockerfile
# Use Node.js as base image
FROM node:18-alpine

# Set working directory
WORKDIR /app

# Copy package.json and install dependencies
COPY package.json package-lock.json ./
RUN npm install --production

# Copy the entire backend source code
COPY . .

# Expose port 5000
EXPOSE 5000

# Start the backend server
CMD ["node", "server.js"]
```

---

## **📝 Kustomization File (`kustomization.yaml`)**

```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

resources:
  - frontend/deployment.yaml
  - frontend/service.yaml
  - backend/deployment.yaml
  - backend/service.yaml
  - mongodb/statefulset.yaml
  - mongodb/service.yaml
  - mongodb/secret.yaml
  - ingress.yaml
```

To apply this, use:

```sh
kubectl apply -k k8s/
```

---

## **🛠️ Next Steps**

✅ **Set Up CI/CD** (GitHub Actions for automated deployments)  
✅ **Add Monitoring** (Prometheus & Grafana for metrics)  
✅ **Secure the Cluster** (RBAC, Network Policies, and Secrets Management)
