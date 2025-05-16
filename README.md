# K8s-Kubernetes-DeepDive

## About Kubernetes
- Open source container orchestration tool
- Developed by Google
- Helps to manage containerized applications in different deployment environments

## The need for a container orchestration tool
- Trend from **Monolith** to **Microservices**
- Increase the usage of containers
- Demand for a proper way of managing hundreds of containers

## Features do orchestration tools offer
- **High Availability** or no downtime
- **Scalability** or high performance
- **Disaster recovery** - backup and restorce

## I. Main K8s Components
### 1. Pods
- A **Pod** is the smallest unit you can deploy in Kubernetes
- A Pod usually runs one container, but can run a few tightly connected ones
- Containers in a Pod share the same network and storage
- Each Pod has its own IP address
- If a Pod fails, Kubernetes replaces it with a new one

### 2. Services
- Offering permanent IP address
- Lifecycle of Pod and Service **NOT conected**

### 3. Ingress
- **Ingress** manage external access (like HTTP/HTTPS) to services in Kubernetes cluster
- It routes requests from outside the cluster to the right service based on rules (like URL paths or hostnames)
- Expose multiple services using one IP or domain
- Can handle SSL/TLS, virtual hosting, and load balancing
- Needs an **Ingress Controller** to work
- Makes it easier to expose services without creating a LoadBalancer or NodePort for each one

### 4. ConfigMap & Secret
#### 4.1. ConfigMap
- Used to store non-sensitive configuration data as key-value pairs
- Makes it easy to update configuration without rebuilding container images

#### 4.2. Secret
- Used to store sensitive data such as passwords, tokens, or keys
- Data is encoded (base64) and kept separate from application code

## 5. Deployment & StatefulSet
### 5.1. Deployment
- Manages **stateless** applications
- Handles rolling updates, rollbacks, and scaling automatically
- Good for apps where each replica is the same (e.g., web servers)

### 5.2. StatefulSet
- Manages **stateful** applications
- Useful for apps needing stable network names or storage (e.g., databases)
- Each pod keeps its own data and hostname

## II. K8s Architecture

### 1. Node Processes
- Each node has multiple **Pods** on it
- 3 processes must be installed on every **Node**
    - **Docker runtime**
    - **Kubelet**: 
      - Interacts with both - the container and node
      - Starts the pod with a container inside
    - **Kube Proxy**: Forwards the requests from services to the replica that run in the same node.

### 2. Master Processes

- The **Kubernetes Master** is responsible for managing the cluster and making global decisions about the cluster (e.g., scheduling).
- Main components:
    - **API Server**: The front-end for the Kubernetes control plane. All communication (from users, CLI, and other components) goes through the API server.
    - **Scheduler**: Assigns newly created pods to nodes based on resource requirements and availability.
    - **Controller Manager**: Runs controllers that handle routine tasks, such as ensuring the desired number of pod replicas are running.
    - **etcd**: A distributed key-value store used to store all cluster data and configuration. It acts as the single source of truth for the cluster state.
- These components work together to maintain the desired state of the cluster, handle failures, and coordinate workloads.

## III. Minikube and Kubectl

### 1. Minikube (Test/Local cluster setup)
- One node cluster where the **Master Processes** and **Worker Processes** both run on one node
- Creates Virtual Box on local machine (laptop, comupter, etc.)
- Node runs in that Virtual Box
- 1 Node K8s cluster
- For testing purposes

### 2. Kubectl
- **Kubectl** is the command-line tool used to interact with a Kubernetes cluster.
- It allows you to create, update, and delete resources (such as pods, nodes, deployments, etc.) and run commands against the cluster.
- Kubectl communicates with the Kubernetes Master processes to manage the cluster state.
- There are three main ways to interact with the Kubernetes Master:
    - **UI** (Dashboard)
    - **API** (programmatic access)
    - **CLI** (Kubectl)
- Kubectl is the most common and flexible way for administrators and developers to manage and troubleshoot Kubernetes clusters.

### 3. Installation

#### 3.1. Minikube Installation
**Prerequisites:**
- Virtualization support (e.g., VirtualBox, Hyper-V, Docker)
- Latest version of [Minikube](https://minikube.sigs.k8s.io/docs/start/)

**Steps:**
1. Download and install Minikube:
    - **Windows:**  
      Download the installer from [Minikube Releases](https://github.com/kubernetes/minikube/releases) and run it.
    - **macOS:**  
      ```sh
      brew install minikube
      ```
    - **Linux:**  
      ```sh
      curl -LO "https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64"
      sudo install minikube-linux-amd64 /usr/local/bin/minikube
      ```
2. Start a local cluster:
    ```sh
    minikube start
    ```

#### 3.2. Kubectl Installation
**Steps:**
1. Download and install Kubectl:
    - **Windows:**  
      Download from [Kubernetes Releases](https://kubernetes.io/docs/tasks/tools/)
    - **macOS:**  
      ```sh
      brew install kubectl
      ```
    - **Linux:**  
      ```sh
      curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
      chmod +x kubectl
      sudo mv kubectl /usr/local/bin/
      ```
2. Verify installation:
    ```sh
    kubectl version --client
    ```

Refer to the [official documentation](https://kubernetes.io/docs/tasks/tools/) for more details and platform-specific instructions.

### 4. K8s CLI - Kubectl Commands

#### CRUD Operations

| Command                                    | Description                            |
|--------------------------------------------|----------------------------------------|
| `kubectl create deployment <name>`         | Create a Deployment                    |
| `kubectl edit deployment <name>`           | Edit an existing Deployment            |
| `kubectl delete deployment <name>`         | Delete a Deployment                    |

#### Status of Different K8s components

| Resource     | Command                                  |
|--------------|------------------------------------------|
| Nodes        | `kubectl get nodes`                      |
| Pods         | `kubectl get pods`                       |
| Services     | `kubectl get services`                   |
| ReplicaSets  | `kubectl get replicasets`                |
| Deployments  | `kubectl get deployments`                |

> **Tip:** Combine resources with commas:
> ```sh
> kubectl get pods,services,deployments
> ```

#### Debugging pods

| Command  | Description                                                                            |
|------------------------------------------------------------| -------------------------------------|
| `kubectl logs <pod-name>`                                  | Stream container logs to your console|
| `kubectl exec -it <pod-name> -- /bin/bash`                 | Open an interactive shell in the Pod |
| `kubectl describe pod <pod-name>`                          | Show detailed Pod information        |

#### Use Configuration File for CRUD

| Command                             | Description                             |
|-------------------------------------|-----------------------------------------|
| `kubectl apply -f <file.yaml>`      | Create or update resources from a file  |
| `kubectl delete -f <file.yaml>`     | Delete resources defined in a file      |

## 5. Context

- Always verify your current context before making changes:  
  ```sh
  kubectl config current-context
  ```

