# Day 50 - Kubernetes Architecture and Cluster Setup

## Objective

Learn the fundamentals of Kubernetes, understand its architecture, set up a local Kubernetes cluster using Kind, and explore the cluster using kubectl.

---

# Task 1 - Kubernetes Story

## 1. Why was Kubernetes created?

Docker made it easy to package applications into containers, but managing hundreds or thousands of containers across multiple servers became difficult.

Kubernetes was created to solve container orchestration problems such as:

- Automatic deployment
- Scaling applications
- Load balancing
- Self-healing
- Service discovery
- Rolling updates

Docker manages containers, while Kubernetes manages containerized applications at scale.

---

## 2. Who created Kubernetes?

- Originally created by **Google**
- Inspired by Google's internal cluster management system called **Borg**
- Donated to the **Cloud Native Computing Foundation (CNCF)** in 2015

---

## 3. Meaning of Kubernetes

"Kubernetes" is a Greek word meaning:

> **Helmsman** or **Pilot**

It represents steering or managing containerized applications.

---

# Task 2 - Kubernetes Architecture

```
                    +----------------------+
                    |      kubectl         |
                    +----------+-----------+
                               |
                               v
                    +----------------------+
                    |     API Server       |
                    +----------+-----------+
                               |
          +--------------------+--------------------+
          |                    |                    |
          v                    v                    v
     +---------+        +-------------+      +--------------+
     | etcd    |        | Scheduler   |      | Controller   |
     |         |        |             |      | Manager      |
     +---------+        +-------------+      +--------------+

                        Control Plane
---------------------------------------------------------------

               Worker Node

      +------------------------------------------+

      kubelet
           |
           v

     +-------------+
     | kube-proxy  |
     +-------------+

           |
           v

     +--------------------+
     | Container Runtime  |
     | (containerd)       |
     +--------------------+

           |
           v

        Running Pods
```

---

## What happens when we run?

```bash
kubectl apply -f pod.yaml
```

### Flow

1. kubectl sends request to API Server.
2. API Server validates the YAML.
3. Desired state is stored inside etcd.
4. Scheduler selects a Worker Node.
5. kubelet receives instructions.
6. Container Runtime pulls the image.
7. Pod starts running.
8. kube-proxy configures networking.
9. Pod becomes Ready.

---

## What if API Server goes down?

- kubectl commands stop working.
- New Pods cannot be scheduled.
- Existing running Pods continue working.

---

## What if Worker Node goes down?

- Node becomes NotReady.
- Controller Manager detects failure.
- Pods are recreated on healthy nodes.

---

# Task 3 - Install kubectl

Installed kubectl.

Verification:

```bash
kubectl version --client
```

Example Output

```text
Client Version: v1.36.x
```

---

# Task 4 - Local Cluster Setup

## Selected Tool

**Kind (Kubernetes in Docker)**

### Why Kind?

- Lightweight
- Fast startup
- Runs entirely inside Docker
- Perfect for local development
- Easy to create and delete clusters

---

## Create Cluster

```bash
kind create cluster --name devops-cluster
```

---

## Verify Cluster

```bash
kubectl cluster-info

kubectl get nodes
```

Example Output

```text
NAME                            STATUS   ROLES           AGE   VERSION
devops-cluster-control-plane    Ready    control-plane   2m    v1.33.x
```

---

# Task 5 - Explore the Cluster

## Cluster Info

```bash
kubectl cluster-info
```

---

## List Nodes

```bash
kubectl get nodes
```

---

## Node Details

```bash
kubectl describe node devops-cluster-control-plane
```

---

## Namespaces

```bash
kubectl get namespaces
```

Example

```
default
kube-system
kube-public
kube-node-lease
```

---

## All Pods

```bash
kubectl get pods -A
```

---

## kube-system Pods

```bash
kubectl get pods -n kube-system
```

Typical Output

```
coredns
etcd
kindnet
kube-apiserver
kube-controller-manager
kube-proxy
kube-scheduler
```

---

# What Each kube-system Pod Does

| Pod | Purpose |
|------|----------|
| kube-apiserver | Entry point of the Kubernetes cluster |
| etcd | Stores all cluster configuration and state |
| kube-scheduler | Assigns Pods to Worker Nodes |
| kube-controller-manager | Maintains desired state |
| kube-proxy | Manages networking and Services |
| CoreDNS | DNS service inside Kubernetes |
| kindnet | Networking plugin used by Kind |

---

# Task 6 - Cluster Lifecycle

## Delete Cluster

```bash
kind delete cluster --name devops-cluster
```

---

## Recreate Cluster

```bash
kind create cluster --name devops-cluster
```

---

## Verify

```bash
kubectl get nodes
```

---

## Current Context

```bash
kubectl config current-context
```

---

## Available Contexts

```bash
kubectl config get-contexts
```

---

## View kubeconfig

```bash
kubectl config view
```

---

# What is kubeconfig?

The kubeconfig file stores:

- Cluster information
- User credentials
- Contexts
- Authentication details

Default location:

```text
~/.kube/config
```

---

# Commands Practiced

```bash
kubectl version --client

kubectl cluster-info

kubectl get nodes

kubectl describe node <node-name>

kubectl get namespaces

kubectl get pods -A

kubectl get pods -n kube-system

kubectl config current-context

kubectl config get-contexts

kubectl config view

kind create cluster --name devops-cluster

kind delete cluster --name devops-cluster
```

---

# What I Learned

- Kubernetes is a container orchestration platform.
- Kubernetes was created by Google and inspired by Borg.
- The Control Plane manages the entire cluster.
- Worker Nodes execute containerized workloads.
- kubectl communicates with the API Server.
- etcd stores cluster state.
- Scheduler places Pods on nodes.
- kubelet manages Pods.
- kube-proxy handles networking.
- Kind provides an easy way to run Kubernetes locally.

---

# Screenshots

## kubectl get nodes



---

## kubectl get pods -n kube-system

> 

---

# Conclusion

Today I started my Kubernetes journey by understanding its architecture, creating a local Kind cluster, exploring the Control Plane components, and learning how kubectl communicates with the cluster. I also practiced creating and deleting clusters and explored the kube-system namespace to understand how Kubernetes operates internally.