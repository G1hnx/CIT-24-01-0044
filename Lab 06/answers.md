# Lab 6 – Kubernetes Fundamentals with Minikube

## answers.md

# Checkpoint Q1

**Explain the difference between the control plane and a worker node.**

The **control plane** manages the Kubernetes cluster. It makes scheduling decisions, maintains the desired state of the cluster, stores cluster information, and coordinates all cluster activities. Components such as the API Server, Scheduler, Controller Manager, and etcd belong to the control plane.

A **worker node** runs the application workloads. It executes Pods assigned by the control plane and contains components such as the kubelet, kube-proxy, and the container runtime. Worker nodes perform the actual work of running containers, while the control plane manages and monitors the cluster.

---

# Checkpoint Q2

**Delete the pod, recreate it, and check its IP. Has the IP changed? Explain why Pods are considered ephemeral.**

Yes. After deleting and recreating the Pod, its IP address changed.

Pods are **ephemeral**, meaning they are temporary resources. When a Pod is deleted, Kubernetes creates a completely new Pod with a new IP address instead of restoring the old one. Applications should therefore communicate through Kubernetes Services rather than directly using Pod IP addresses.

---

# Checkpoint Q3

**Using the control-loop model, explain what happened when the Pod was deleted.**

When one of the Pods was deleted, Kubernetes followed its control-loop process:

1. The Deployment's desired state remained three replicas.
2. The Deployment Controller detected that only two Pods were running.
3. The controller compared the desired state with the actual state and identified the difference.
4. Kubernetes automatically created a replacement Pod.
5. The Scheduler assigned the new Pod to the node.
6. The kubelet started the container.
7. The Deployment returned to its desired state of three running Pods.

This demonstrates Kubernetes' self-healing capability.

---

# Checkpoint Q4

**Why can the frontend be scaled independently after deploying the database tier?**

Each application tier is deployed as an independent Kubernetes Deployment or StatefulSet. The frontend communicates with the backend through Kubernetes Services instead of being directly connected to the database.

Because each component is independent, the frontend can be increased or decreased in replicas without affecting the API, cache, or database. This allows each service to scale according to its own workload.

---

# Checkpoint Q5

**What is the difference between using port-forward and using a Service? Why are Services important?**

Port-forward creates a temporary connection from the local computer directly to a specific Pod. It is mainly used for testing and debugging.

A Kubernetes Service provides a stable network endpoint that forwards traffic to one or more Pods. Even if Pods are deleted and recreated with different IP addresses, the Service continues routing traffic correctly.

Services are important because Pods are ephemeral and their IP addresses change over time.

---

# Checkpoint Q6

**Why is rolling update and rollback harder with Docker Compose?**

Docker Compose does not provide built-in rolling updates or automatic rollback capabilities.

Kubernetes performs rolling updates gradually, replacing Pods one at a time while keeping the application available. If an update fails, Kubernetes can automatically roll back to the previous version.

With Docker Compose, updates usually require manually stopping containers, pulling new images, and restarting services, increasing the risk of downtime and making rollback more difficult.

---

# Checkpoint Q7

**Why do the frontend and API use Deployments while PostgreSQL uses a StatefulSet?**

The frontend and API are **stateless** applications. They do not permanently store data inside the container and can be freely replaced or scaled. Deployments are designed for these workloads.

PostgreSQL is a **stateful** application because it stores persistent data. A StatefulSet provides stable Pod names, ordered deployment, ordered termination, and persistent storage through PersistentVolumeClaims. These features ensure that database data is preserved even when Pods are recreated.

---

# Checkpoint Q8

**Would the data survive if PostgreSQL was deployed as a normal Deployment without a PersistentVolumeClaim?**

No.

Without a PersistentVolumeClaim, the database files would be stored only inside the container's writable layer. If the Pod were deleted or recreated, all stored data would be lost.

Using a StatefulSet together with a PersistentVolumeClaim ensures that the storage remains available after Pod recreation, allowing the database data to persist.

---

# Checkpoint Q9

**What status did the broken Pod show? What does it mean?**

The broken Pod showed the status **ImagePullBackOff** (sometimes preceded by **ErrImagePull**).

This status indicates that Kubernetes attempted to download the container image but failed because the specified image tag did not exist. Kubernetes repeatedly retries pulling the image with increasing delays, leaving the Pod unable to start until the image reference is corrected.

Although **ImagePullBackOff** is not one of the statuses listed in the lecture's basic Pod status table, it is a related error status that indicates a problem during image retrieval before the container can run.
