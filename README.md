# sampledemoprojects

## Full internal flow
Client
   ↓
Service (ClusterIP)
   ↓
kube-proxy (on node)
   ↓
Endpoints (Pod IPs)
   ↓
Pod (nginx container)

## Kubernetes Service

A Service in Kubernetes is used to expose and enable communication between different components (Pods) within or outside the cluster.
Pods are dynamic and can change over time, so a Service provides a stable network endpoint to access them.

A Kubernetes Service works by using label selectors to identify Pods, creating endpoints for them, and routing traffic via kube-proxy using a stable ClusterIP.

---

### Why Service is Needed

* Pods have dynamic IP addresses (they can change anytime)
* Direct communication between Pods is unreliable
* Service provides a **stable IP and DNS name**
* Enables load balancing across multiple Pods

---

### How Service Works

* A Service uses **labels and selectors** to identify target Pods
* Traffic is routed to matching Pods automatically
* It distributes requests across multiple Pods (load balancing)

### Key components involved
Labels & Selectors → identify Pods
Endpoints → list of Pod IPs
ClusterIP → stable entry point
kube-proxy → traffic routing
iptables/IPVS → actual network rules

### Important concept

In Kubernetes:
Endpoints = actual Pod IPs behind a Service
Service = virtual entry point
kube-proxy uses endpoints to route traffic

### Types of Services

#### 1. ClusterIP

* Default Service type
* Used for internal communication within the cluster
* Not accessible from outside

#### 2. NodePort

* Exposes the application externally
* Accessible using `<NodeIP>:NodePort`
* Used for development and testing
* uses internal ip of node

#### 3. LoadBalancer

* Exposes application to the internet
* Assigns an external IP (cloud environments)
* Used in production systems
* Loadbalancer service creates external ip.We can use cloud service provider like AWS,GCP,Azure to create LB

---

###  Key Concepts

* **Selector** → Matches Pods using labels
* **Port** → Service port
* **TargetPort** → Container port inside Pod
* **NodePort** → External access port (for NodePort Service)

---

###  Example Flow

User → Service → Pod

* User sends request to Service
* Service routes request to appropriate Pod
* Pod processes and returns response

---

###  Summary

Kubernetes Service acts as a bridge between users and Pods, ensuring reliable communication, load balancing, and stable access to applications.

---

### Commands:

### Project Structure
sampledemoprojetc/
├── pod.yaml
├── clusterip-service.yaml
├── nodeport-service.yaml
├── loadbalancer-service.yaml
├── README.md

### How to Run
kubectl apply -f pod.yaml
kubectl apply -f clusterip-service.yaml
kubectl apply -f nodeport-service.yaml
kubectl apply -f loadbalancer-service.yaml

### Checking Resources
kubectl get pods
kubectl get svc
kubectl get nodes 

### Show labels for Pods
kubectl get pods --show-labels

### Example output:
NAME        READY   STATUS    LABELS
nginxpod    1/1     Running   app=webserver

### Show labels for Services
kubectl get svc --show-labels

### To get endpoints
kubectl get endpoints

### Example output
NAME                ENDPOINTS           AGE
clusterip-service   10.244.0.2:80       2m
nodeport-svc        10.244.0.2:80       2m
loadbalancer-svc    10.244.0.2:80       2m

### Note
service name nodeport-svc
pod ip 10.244.0.2 
container port 80

### What this means
10.244.0.2 → Pod IP
80 → container port (Nginx)

### Get nodes 
kubectl get nodes
kubectl get nodes -o wide

### Example Output
NAME        STATUS   ROLES           AGE   VERSION   INTERNAL-IP     EXTERNAL-IP
minikube    Ready    control-plane   5d    v1.29.0   192.168.49.2    <none>

### What these IPs mean
INTERNAL-IP → used inside the network (most common for testing)
EXTERNAL-IP → public IP (used in cloud like AWS, Azure)

### Important for your project

For my NodePort Service, you use:
http://<NodeIP>:30019
Example:
http://192.168.49.2:30019

### describe nodes
kubectl describe svc nodeport-svc
It shows detailed information about your Service (not just basic info like get svc).











