# Kubernetes MongoDB Demo Application

<p align="center">
  <img src="https://raw.githubusercontent.com/kubernetes/kubernetes/master/logo/logo.svg" width="100" alt="Kubernetes Logo"/>
  <img src="https://www.vectorlogo.zone/logos/mongodb/mongodb-icon.svg" width="100" alt="MongoDB Logo"/>
</p>

Demo project for deploying MongoDB and Mongo Express in Kubernetes.

## Components

- **MongoDB** - NoSQL database
- **Mongo Express** - web-based MongoDB admin interface
- **ConfigMap** - database connection configuration
- **Secret** - MongoDB credentials

## Project Structure

```
.
├── mongo.yaml              # Deployment and Service for MongoDB
├── mongo-express.yaml      # Deployment and Service for Mongo Express
├── mongo-configmap.yaml    # ConfigMap with database URL
└── mongo-secret.yaml       # Secret with credentials
```

## Deployment

1. Create Secret with credentials:
```bash
kubectl apply -f mongo-secret.yaml
```

2. Create ConfigMap:
```bash
kubectl apply -f mongo-configmap.yaml
```

3. Deploy MongoDB:
```bash
kubectl apply -f mongo.yaml
```

4. Deploy Mongo Express:
```bash
kubectl apply -f mongo-express.yaml
```

Or apply all manifests at once:
```bash
kubectl apply -f .
```

## Access the Application

Mongo Express will be accessible through NodePort 30000:
```
http://<node-ip>:30000
```

For Minikube use:
```bash
minikube service mongo-express-service
```

## Default Credentials

- Username: `username` (base64: dXNlcm5hbWU=)
- Password: `password` (base64: cGFzc3dvcmQ=)

⚠️ **Important**: Change the credentials in `mongo-secret.yaml` before using in production!

## Check Status

```bash
# Check pods
kubectl get pods

# Check services
kubectl get services

# MongoDB logs
kubectl logs deployment/mongodb-deployment

# Mongo Express logs
kubectl logs deployment/mongo-express
```

## Cleanup

```bash
kubectl delete -f .
```

## Architecture

```
┌─────────────────────────────────────┐
│         Mongo Express UI            │
│      (LoadBalancer:30000)           │
└──────────────┬──────────────────────┘
               │
               ▼
┌─────────────────────────────────────┐
│      MongoDB Service (27017)        │
└──────────────┬──────────────────────┘
               │
               ▼
┌─────────────────────────────────────┐
│         MongoDB Pod                 │
│      (Deployment: 1 replica)        │
└─────────────────────────────────────┘
```
