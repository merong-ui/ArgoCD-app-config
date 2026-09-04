# GitOps Continuous Delivery with Argo CD

##  Project Overview

This project demonstrates a **GitOps-based Continuous Delivery (CD) workflow using Argo CD and Kubernetes**.

The focus of this project is specifically on the **CD portion of the DevOps lifecycle**. It demonstrates how Kubernetes application configuration stored in a Git repository can be automatically synchronized and deployed to a Kubernetes cluster using Argo CD.

**GitHub acts as the source of truth**, while Argo CD continuously monitors the repository and ensures that the Kubernetes environment matches the desired configuration.

---

#  How the Project Works

## 1. Kubernetes Configuration

The application's desired state is defined using **Kubernetes YAML manifests**.

These manifests describe resources such as:

* Deployment
* Service

Example:

```text
deployment.yaml
service.yaml
```

---

## 2. Store Configuration in GitHub

The Kubernetes manifests are stored in a dedicated GitHub repository.

Git serves as the **single source of truth** for the application's desired state.

Any configuration changes are committed to Git instead of being manually applied directly to the Kubernetes cluster.

---

## 3. Argo CD Connects to GitHub

Argo CD is configured to connect to the Git repository and monitor the Kubernetes manifests.

Argo CD continuously checks the repository for changes to the desired application state.

---

## 4. Argo CD Compares Desired and Current States

Argo CD continuously compares the desired state stored in Git with the current state running in Kubernetes.

```text
       Desired State
             │
             ▼
      GitHub Repository
             ^ 
             │
        Argo CD
             │
             │
             ▼
       Kubernetes Cluster
        Current State
```

If the desired state and current state are different, Argo CD identifies the application as:

```text
OutOfSync
```

---

## 5. Synchronization

Argo CD synchronizes the Kubernetes cluster with the configuration stored in Git.

```text
Git Configuration
       │
    Argo CD
       │
       ▼
Kubernetes Resources
       │
       ▼
   Application
```

This allows application deployments to be managed through Git rather than manually applying Kubernetes configuration.

---

#  Kubernetes Resources

## Deployment

The Kubernetes Deployment manages the application's Pods and maintains the desired number of replicas.

It also provides capabilities such as:

* Maintaining the desired number of Pods
* Rolling updates
* Self-healing
* Managing application versions

---

## Service

The Kubernetes Service provides network access to the application running inside the Kubernetes cluster.

It provides a stable network endpoint for accessing the application's Pods.

---

#  Technologies Used

| Technology     | Purpose                                   |
| -------------- | ----------------------------------------- |
| **Argo CD**    | GitOps Continuous Delivery                |
| **Kubernetes** | Container orchestration                   |
| **Docker**     | Application containerization              |
| **GitHub**     | Git repository and source of truth        |
| **Git**        | Version control                           |
| **YAML**       | Kubernetes configuration                  |
| **kubectl**    | Kubernetes management and troubleshooting |
| **Linux**      | Administration environment                |

---

#  Commands Used in This Project

## Install Argo CD in Kubernetes

### 1. Create the Argo CD namespace

```bash
kubectl create namespace argocd
```

### 2. Install Argo CD

```bash
kubectl apply -n argocd \
  -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

---

#  Access Argo CD UI

### 1. Check Argo CD services

```bash
kubectl get svc -n argocd
```

### 2. Port-forward the Argo CD server

```bash
kubectl port-forward svc/argocd-server 8080:443 -n argocd
```

The Argo CD UI can then be accessed locally through:

```text
https://localhost:8080
```

---

#  Get the Initial Argo CD Admin Password

The initial admin password can be retrieved using:

```bash
kubectl -n argocd get secret argocd-initial-admin-secret \
  -o jsonpath="{.data.password}" | base64 --decode && echo
```

The returned password can be used to log in to the Argo CD dashboard with the `admin` user.

---

#  Deploy the Argo CD Application

After installing Argo CD, the Argo CD `Application` resource is created using the application configuration file.

```bash
kubectl apply -f application.yaml
```

The `application.yaml` file defines information such as:

* Git repository
* Repository path
* Target Kubernetes cluster
* Target namespace
* Synchronization configuration

---

#  Argo CD Synchronization

Once the application is created, Argo CD monitors the Git repository and compares the desired configuration with the Kubernetes cluster.

A successful deployment appears as:

```text
Application: Synced
Health: Healthy
```

This confirms that the Kubernetes cluster matches the desired configuration stored in Git.

---

#  Argo CD Successfully Synced

The Argo CD dashboard shows the application successfully synchronized with the Kubernetes cluster.

![Argo CD Successfully Synced]
<img width="1032" height="353" alt="Screenshot 2026-09-03 122218" src="https://github.com/user-attachments/assets/c33064d8-1f55-4c68-a0f5-087dc380c1ad" />

---

#  Troubleshooting Commands

The following `kubectl` commands were used to inspect and troubleshoot Kubernetes resources.

### Check Pods

```bash
kubectl get pods
```

### Check Deployments

```bash
kubectl get deployments
```

### Check Services

```bash
kubectl get services
```

### Check All Resources

```bash
kubectl get all
```

### Describe a Pod

```bash
kubectl describe pod <pod-name>
```

### View Pod Logs

```bash
kubectl logs <pod-name>
```

These commands help identify issues with Pods, Deployments, Services, and application containers.

---

#  GitOps Concepts Demonstrated

## Declarative Configuration

The desired application state is defined using Kubernetes YAML files.

Instead of manually configuring resources, the desired state is described declaratively.

---

## Git as the Source of Truth

The Git repository contains the desired configuration for the application.

```text
Git Repository
      │
      ▼
 Desired State
```

---

## Automated Synchronization

Argo CD continuously monitors Git and synchronizes the Kubernetes cluster with the desired state.

```text
Git Change
    │
    ▼
 Argo CD Detects Change
    │
    ▼
 Kubernetes Synchronization
```

---

## Configuration Drift

Configuration drift occurs when the actual state of the Kubernetes environment differs from the desired state stored in Git.

Argo CD can detect this difference and report the application as **OutOfSync**.

---

#  Key Learning Outcomes

Through this project, I gained hands-on experience with:

* **Argo CD**
* **GitOps methodology**
* **Continuous Delivery**
* **Kubernetes**
* **Docker containers**
* **Git and GitHub**
* **Kubernetes YAML manifests**
* **Declarative application deployment**
* **Application synchronization**
* **Configuration drift detection**

---

# 🔗 Relationship to My CI Project

This project is intentionally focused on the **CD/GitOps portion** of the DevOps lifecycle.

I have a separate project focused on **Continuous Integration (CI)** using Jenkins.

### CI Project

```text
Developer
    │
    ▼
  GitHub
    │
    ▼
  Jenkins
    │
    ├── Build
    ├── Test
    └── Trivy Scan
    │
    ▼
 Amazon ECR
```

### CD / GitOps Project

```text
GitOps GitHub Repository
           │
           ▼
        Argo CD
           │
           ▼
      Kubernetes
           │
           ▼
       Application
```

The projects are currently maintained separately to demonstrate the **CI and CD concepts independently**.

---

#  Conclusion

This project demonstrates how **Argo CD can implement GitOps-based Continuous Delivery for Kubernetes**.

The main principle demonstrated is:

> **Git defines the desired state, and Argo CD continuously works to make the Kubernetes environment match that desired state.**

This project complements my separate Jenkins CI project by providing hands-on experience with the **CD/GitOps side of the DevOps lifecycle**.

