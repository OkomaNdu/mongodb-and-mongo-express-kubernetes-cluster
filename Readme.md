# MongoDB & mongo-express on Kubernetes

This project deploys a **MongoDB** database and a **mongo-express** web UI on a Kubernetes cluster using YAML manifests.

---

## Files

- `mongo.yaml`  
  - MongoDB Deployment: `mongodb-deployment`  
  - MongoDB Service: `mongodb-service` (ClusterIP on port 27017)

- `mongo-configmap.yaml`  
  - ConfigMap: `mongodb-configmap`  
  - Key: `database_url = "mongodb-service:27017"`

- `mongo-express.yaml`  
  - Deployment: `mongo-express` (web UI on port 8081)  
  - Service: `mongo-express-service` (type `LoadBalancer`, `nodePort: 30000`)

- `mongo-secret`  
  - Secret: `mongodb-secret`  
  - Keys (base64 encoded):  
    - `mongo-root-username: dXNlcm5hbWU=` → `username`  
    - `mongo-root-password: cGFzc3dvcmQ=` → `password`

---

## Prerequisites

- Kubernetes cluster
- `kubectl` configured to your cluster

Resources are created in the **current namespace** (no namespace specified in YAML).

---

## Deploy

Apply manifests in this order:

```bash
kubectl apply -f mongo-secret
kubectl apply -f mongo-configmap.yaml
kubectl apply -f mongo.yaml
kubectl apply -f mongo-express.yaml


## Check everything is running:
- kubectl get pods
- kubectl get svc

##Access mongo-express
Get the Service:
- minikube service mongo-express-service

## Access options:
- Via LoadBalancer external IP (if present):
   http://<EXTERNAL-IP>:8081
- Via NodePort:
   http://<NODE-IP>:30000

## Login credentials
From mongodb-secret:
- Username: username
- Password: password
