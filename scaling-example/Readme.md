# Kubernetes Scaling Examples

This repository demonstrates **horizontal** and **vertical** scaling concepts in Kubernetes.

### **Folder Structure**

1. **horizontal-scaling**: Demonstrates horizontal scaling by adjusting the number of pod replicas.
2. **vertical-scaling**: Demonstrates vertical scaling by adjusting CPU and memory resources for a single pod.
3. **kustomization.yaml**: Optional file to use **Kustomize** for managing YAML files.

---

## **Horizontal Scaling**

Horizontal scaling involves **adding or removing** pod replicas to handle increased or decreased traffic. This is ideal when your application can scale out (i.e., run multiple instances of the same pod).

### Example

In the `horizontal-scaling/` folder, you will find a `frontend-deployment-horizontal.yaml` file which defines the frontend application with 4 replicas.

- **Deployment YAML**: Defines the number of replicas.
- **Command to Scale**: `kubectl scale deployment frontend-deployment --replicas=4`

---

## **Vertical Scaling**

Vertical scaling involves adjusting the **resources (CPU & memory)** allocated to a pod. This is useful when you need more resources for a single instance of your application.

### Example

In the `vertical-scaling/` folder, you will find a `frontend-deployment-vertical.yaml` file, which defines the frontend application with resource requests and limits.

- **Resources**: Defines CPU and memory requests and limits.
- **Example Usage**: When a pod needs more memory or CPU to process intensive tasks.

---

## **Kustomize**

You can optionally use `kustomization.yaml` to apply all the resources together with `kubectl apply -k .`.

---

### **How to Use**

1. Clone the repository.
2. Apply the respective scaling example.
   - For horizontal scaling: `kubectl apply -f horizontal-scaling/frontend-deployment-horizontal.yaml`
   - For vertical scaling: `kubectl apply -f vertical-scaling/frontend-deployment-vertical.yaml`

---

### **🚀 Summary**

| Scaling Type           | Method                           | Best Use Case       |
| ---------------------- | -------------------------------- | ------------------- |
| **Horizontal Scaling** | Increases number of pod replicas | High traffic apps   |
| **Vertical Scaling**   | Increases CPU & memory per pod   | Resource-heavy apps |

---

## **Conclusion**

- **Horizontal scaling** increases the number of pod replicas to handle more traffic.
- **Vertical scaling** increases the CPU and memory resources of individual pods.
- Both strategies can be applied manually or through autoscalers.

For further reading on autoscaling, visit the [Kubernetes Horizontal Pod Autoscaler](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/).
