# 🚀 RoboShop on Amazon EKS (Kubernetes)

## 📖 Project Overview

This project demonstrates deploying the complete **RoboShop Microservices Application** on **Amazon Elastic Kubernetes Service (EKS)** using Kubernetes manifests.

The project focuses on learning real-world Kubernetes concepts by deploying, configuring, troubleshooting, and managing multiple microservices running in Docker containers.

Each microservice is independently containerized, pushed to Docker Hub, and deployed on Amazon EKS using Kubernetes Deployments and Services. Configuration is externalized using ConfigMaps, while application health is monitored through Readiness, Liveness, and Startup Probes.

This project also involved extensive troubleshooting of Kubernetes deployments, Docker image management, AWS networking, and CloudFormation resources.

---

# 🛠 Tech Stack

### Cloud
- Amazon Web Services (AWS)
- Amazon EKS
- Amazon EC2
- CloudFormation
- IAM
- VPC

### Containerization
- Docker
- Docker Hub

### Container Orchestration
- Kubernetes
- eksctl
- kubectl
- k9s

### Databases & Messaging
- MongoDB
- MySQL
- Redis
- RabbitMQ

### Languages
- Java (Spring Boot)
- NodeJS
- Python (uWSGI)
- Nginx

---

# 📂 Project Structure

```
k8-roboshop/
│
├── 00-namespace.yaml
├── mongodb/
├── mysql/
├── redis/
├── rabbitmq/
├── catalogue/
├── user/
├── cart/
├── shipping/
├── payment/
├── frontend/
└── debug/
```

---

# 🏗 Kubernetes Resources Used

- Namespace
- Deployments
- ReplicaSets
- Pods
- Services
- ConfigMaps
- Labels & Selectors
- Resource Requests
- Resource Limits
- Readiness Probes
- Liveness Probes
- Startup Probes

---

# 📦 Microservices Deployed

| Service | Technology |
|----------|------------|
| Frontend | Nginx |
| Catalogue | NodeJS |
| User | NodeJS |
| Cart | NodeJS |
| Shipping | Spring Boot |
| Payment | Python |
| MongoDB | MongoDB |
| MySQL | MySQL |
| Redis | Redis |
| RabbitMQ | RabbitMQ |

---

# 🚀 Features

- Multi-container microservices deployment
- Independent deployments for every service
- Internal service discovery using Kubernetes Services
- Configuration management using ConfigMaps
- Health monitoring using probes
- Resource management using CPU & Memory limits
- Image versioning using Docker tags
- Rolling deployments
- Kubernetes debugging using kubectl and k9s

---

# 📚 What I Learned

## Kubernetes

- Kubernetes Architecture
- Amazon EKS
- Pods
- ReplicaSets
- Deployments
- Services
- ConfigMaps
- Labels & Selectors
- Namespaces
- Resource Requests
- Resource Limits
- Readiness Probes
- Liveness Probes
- Startup Probes
- Rolling Updates
- Internal DNS
- ClusterIP Services
- Container Lifecycle
- Kubernetes Debugging

---

## Docker

- Docker Image Creation
- Multi-stage Docker Builds
- Docker Image Optimization
- Docker Tagging
- Docker Hub
- Docker Build Cache
- Image Versioning
- Image Pull Policies

---

## AWS

- EKS Cluster Creation
- Worker Nodes
- VPC
- NAT Gateway
- Security Groups
- IAM Roles
- CloudFormation
- Cluster Cleanup
- CloudFormation Troubleshooting

---

# 🔧 Major Challenges & Troubleshooting

## 1. Docker Image Cache Issue

### Problem

Kubernetes continued deploying an older Docker image even after rebuilding.

### Root Cause

The image tag remained unchanged and Kubernetes reused the cached image.

### Solution

- Rebuilt images using `--no-cache`
- Tagged new image versions
- Pushed updated images to Docker Hub
- Updated Kubernetes manifests
- Used `imagePullPolicy: Always` when required

---

## 2. Cart Service CrashLoopBackOff

### Problem

The Cart service repeatedly entered CrashLoopBackOff.

### Root Cause

The deployed image did not contain the latest application code.

### Solution

- Verified Docker image contents
- Rebuilt image
- Pushed new version
- Updated Deployment image tag

---

## 3. MySQL Deployment Issues

### Problem

The MySQL container ignored the configured root password.

### Root Cause

An outdated Docker image was being deployed.

### Solution

- Updated Dockerfile
- Rebuilt image
- Tagged new version
- Pushed to Docker Hub
- Updated Deployment
- Verified environment variables

---

## 4. Shipping Service Startup Probe Failure

### Problem

Shipping pod continuously restarted.

### Root Cause

The Startup Probe timed out before the Spring Boot application completed initialization.

### Solution

- Checked pod logs
- Verified ConfigMaps
- Verified MySQL connectivity
- Verified internal DNS
- Tuned Startup Probe configuration

---

## 5. Payment Service CrashLoopBackOff

### Problem

Payment service crashed immediately after startup.

### Root Cause

The Docker image attempted privileged operations while running as a non-root user.

### Solution

- Simplified Dockerfile
- Rebuilt image
- Pushed updated image
- Redeployed application

---

## 6. RabbitMQ Service Discovery

### Problem

RabbitMQ was unavailable because Kubernetes manifests had not been pulled from GitHub.

### Root Cause

Git pull failed due to uncommitted local manifest changes.

### Solution

- Restored modified files
- Pulled latest repository
- Applied RabbitMQ manifests

---

## 7. Git Merge Issues

### Problem

Git refused to pull the latest repository changes.

### Root Cause

Local modifications existed in Kubernetes manifests.

### Solution

Used

```
git restore
git pull
```

to synchronize the repository.

---

## 8. CloudFormation Stack Deletion Failure

### Problem

EKS cluster could not be recreated.

### Root Cause

The CloudFormation stack still existed after a failed cluster deletion.

### Solution

- Disabled termination protection
- Deleted CloudFormation stack
- Investigated DELETE_FAILED resources
- Removed dependent AWS resources

---

## 9. Kubernetes Service Discovery

Learned how Kubernetes automatically creates DNS records.

Examples:

```
mongodb
mysql
redis
rabbitmq
cart
catalogue
user
shipping
payment
```

Applications communicate internally without IP addresses.

---

## 10. Kubernetes Debugging

Frequently used commands:

```bash
kubectl get pods

kubectl get svc

kubectl get configmap

kubectl describe pod

kubectl logs

kubectl logs --previous

kubectl exec

kubectl rollout restart

kubectl rollout status

kubectl get events

kubectl get namespaces

kubectl top pods
```

---

# 💡 Important Lessons Learned

- Image versioning is extremely important.
- Never reuse Docker tags during development.
- Always verify the image running inside Kubernetes.
- Health probes must match application startup time.
- ConfigMaps simplify configuration management.
- Kubernetes DNS makes service-to-service communication straightforward.
- Reading logs is the fastest way to diagnose application failures.
- `kubectl describe` provides valuable troubleshooting information.
- CloudFormation resources may require manual cleanup after failed EKS deployments.
- Small YAML mistakes (ports, labels, selectors, indentation) can prevent an application from working.
- Docker build cache can cause outdated images to be deployed.
- Always verify manifests before applying them.

---

# 🔍 Debugging Workflow Followed

Whenever a pod failed, the following workflow was followed:

1. Check Pod Status

```
kubectl get pods
```

2. Describe the Pod

```
kubectl describe pod <pod-name>
```

3. View Application Logs

```
kubectl logs <pod-name>
```

4. View Previous Logs

```
kubectl logs <pod-name> --previous
```

5. Verify ConfigMaps

```
kubectl get configmap
```

6. Test Internal DNS

```
nslookup <service-name>
```

7. Test Connectivity

```
nc -zv <service-name> <port>
```

8. Verify Deployment

```
kubectl describe deployment
```

9. Restart Deployment

```
kubectl rollout restart deployment/<deployment-name>
```

---

# 🚀 Future Improvements

- Use Kubernetes Secrets for sensitive credentials
- Add Persistent Volumes
- Deploy using Helm Charts
- Configure Horizontal Pod Autoscaler (HPA)
- Add Ingress Controller
- Configure HTTPS using TLS
- Implement GitHub Actions CI/CD
- Add Prometheus & Grafana monitoring
- Centralize logs using EFK Stack
- Deploy using GitOps (ArgoCD)

---

# 🎯 Key Takeaways

This project was much more than deploying containers on Kubernetes. It provided hands-on experience with:

- Designing Kubernetes manifests
- Managing application configuration
- Building optimized Docker images
- Deploying microservices on Amazon EKS
- Troubleshooting real-world Kubernetes issues
- Understanding Kubernetes networking and DNS
- Managing AWS infrastructure
- Debugging CloudFormation failures
- Working with Docker Hub image lifecycle
- Developing systematic debugging skills using Kubernetes tools

The project significantly strengthened my understanding of Kubernetes, Docker, AWS, container orchestration, and production-style troubleshooting, providing practical experience beyond theoretical concepts.
