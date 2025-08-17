# Kubernetes Dashboard UI Setup

This repository contains the necessary files to set up a local Kubernetes cluster using Docker Desktop and deploy the Kubernetes Dashboard UI.

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Setup Instructions](#setup-instructions)
3. [Accessing the Dashboard](#accessing-the-dashboard)
4. [Troubleshooting](#troubleshooting)
5. [Cleanup](#cleanup)

## Prerequisites

- [Docker Desktop](https://www.docker.com/products/docker-desktop) installed and running
- Kubernetes enabled in Docker Desktop
- `kubectl` command-line tool installed
- Admin/privileged access to your machine

## Setup Instructions

### 1. Enable Kubernetes in Docker Desktop

1. Open Docker Desktop
2. Go to Settings > Kubernetes
3. Check "Enable Kubernetes"
4. Click "Apply & Restart"

### 2. Verify Kubernetes Cluster

```bash
kubectl cluster-info
kubectl get nodes
```

### 3. Deploy Kubernetes Dashboard
```bash
# Deploy Kubernetes Dashboard
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml

# Create admin user and role binding
kubectl apply -f dashboard-adminuser.yaml
kubectl apply -f dashboard-rolebinding.yaml
kubectl apply -f secret.yaml
```

### 4. Create Authentication Token
```bash
# Get the token for the admin user
kubectl -n kubernetes-dashboard create token admin-user
```

## Accessing the Dashboard
### Method 1: kubectl proxy (Recommended)
**1. Start the proxy:**
```bash
kubectl proxy
```

**2. Access the dashboard at:**
```
http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/
```
**3. Select "Token" and paste the token obtained in the previous step.**

### Method 2: Port Forwarding
**1. Get the dashboard pod name:**
```bash
kubectl get pods -n kubernetes-dashboard
```
**2. Set up port forwarding:**
```bash
kubectl port-forward -n kubernetes-dashboard <pod-name> 8443:8443
```
**3. Access the dashboard at:**
```
https://localhost:8443
```
