# Kubernetes Fundamentals with Minikube – Lab 6

## Module

**CCS3308 – Virtualization and Containers**

## Practical Lab

**Week 7 – Container Orchestration & Kubernetes**

---

# Student Information

**Name:** *[Your Name]*

**Student ID:** *[Your Student ID]*

**Module:** CCS3308 – Virtualization and Containers

**Lab:** Kubernetes Fundamentals with Minikube

**Submission Type:** Individual

---

# Objective

The objective of this lab was to gain practical experience with Kubernetes using Minikube. The lab covered creating Pods, Deployments, Services, StatefulSets, PersistentVolumeClaims, rolling updates, rollbacks, scaling applications, troubleshooting, and verifying data persistence.

---

# Software Requirements

* Docker Desktop
* Minikube
* Kubernetes (kubectl)
* Windows PowerShell / Command Prompt
* Git
* GitHub

---

# Docker Images Used

| Component | Docker Image         | Port |
| --------- | -------------------- | ---: |
| Frontend  | nginx:alpine         |   80 |
| API       | kennethreitz/httpbin |   80 |
| Cache     | redis:7-alpine       | 6379 |
| Database  | postgres:16-alpine   | 5432 |

---

# Project Structure

```text
lab6/
│
├── k8s/
│   ├── pod-frontend.yaml
│   ├── deployment-frontend.yaml
│   ├── service-frontend.yaml
│   ├── api-deployment.yaml
│   ├── api-service.yaml
│   ├── cache-deployment.yaml
│   ├── cache-service.yaml
│   ├── postgres-statefulset.yaml
│   └── postgres-service.yaml
│
├── screenshots/
│   ├── Task1.1.png
│   ├── Task2.1.png
│   ├── Task3.1.png
│   ├── Task4.1.png
│   ├── Task5.1.png
│   ├── Task6.1.png
│   ├── Task7.1.png
│   ├── Task7.2.png
│   ├── Task8.1.png
│   ├── Task9.1.png
│   └── Task10.1.png
│
├── answers.md
└── README.md
```

---

# Lab Activities

## Part 1 – Cluster Exploration

* Started the Minikube cluster.
* Inspected the Kubernetes control plane and worker node components.
* Verified cluster status using `kubectl`.

---

## Part 2 – Pod Deployment

* Created a standalone frontend Pod using `nginx:alpine`.
* Accessed the application using `kubectl port-forward`.
* Verified Pod creation and observed its ephemeral nature.

---

## Part 3 – Deployment and Self-Healing

* Converted the frontend Pod into a Deployment.
* Configured three replicas.
* Deleted one Pod and observed Kubernetes automatically creating a replacement.

---

## Part 4 – Scaling

* Scaled the frontend Deployment from 3 replicas to 5 replicas.
* Scaled the Deployment back to 2 replicas.
* Observed automatic Pod creation and termination.

---

## Part 5 – Service

* Created a NodePort Service.
* Accessed the frontend application using the Minikube service URL.
* Verified stable network access through the Service.

---

## Part 6 – Rolling Update and Rollback

* Updated the frontend image.
* Performed a rolling update.
* Verified successful deployment.
* Rolled back to the previous version successfully.

---

## Part 7 – Multi-Tier Application

The following Kubernetes resources were created:

### Frontend

* Deployment
* NodePort Service

### API

* Deployment
* ClusterIP Service

### Cache

* Deployment
* ClusterIP Service

### PostgreSQL

* StatefulSet
* PersistentVolumeClaim
* Headless Service

Internal communication between services was verified using a temporary BusyBox debug Pod.

---

## Part 8 – Persistent Storage

* Created a PostgreSQL table.
* Inserted sample data.
* Deleted the PostgreSQL Pod.
* Verified that the data remained after Kubernetes recreated the Pod.

---

## Part 9 – Troubleshooting

* Enabled the Kubernetes Metrics Server.
* Monitored resource usage.
* Created an intentionally broken Pod with an invalid image tag.
* Investigated the resulting `ImagePullBackOff` status using `kubectl describe`.

---

## Part 10 – Cleanup

* Deleted all Kubernetes resources.
* Confirmed that no application resources remained.
* Stopped the Minikube cluster.

---

# Kubernetes Commands Used

```bash
minikube start --driver=docker
kubectl get nodes
kubectl get pods
kubectl get services
kubectl get deployments
kubectl get all
kubectl describe pod
kubectl logs
kubectl apply -f
kubectl delete -f
kubectl scale deployment
kubectl rollout status
kubectl rollout undo
kubectl exec
kubectl top pods
kubectl top nodes
minikube service frontend --url
minikube stop
```

---

# Learning Outcomes

After completing this lab, I was able to:

* Understand Kubernetes architecture.
* Deploy Pods and Deployments.
* Configure Kubernetes Services.
* Scale applications dynamically.
* Observe Kubernetes self-healing.
* Perform rolling updates and rollbacks.
* Deploy stateful applications using StatefulSets.
* Configure PersistentVolumeClaims.
* Verify persistent storage.
* Troubleshoot Kubernetes workloads.
* Monitor cluster resource usage using Metrics Server.

---

# Conclusion

This lab provided practical experience with Kubernetes container orchestration using Minikube. It demonstrated how Kubernetes manages application deployment, scaling, networking, self-healing, rolling updates, persistent storage, and troubleshooting. The exercises reinforced the core concepts of container orchestration and highlighted the advantages of Kubernetes over standalone container management.
