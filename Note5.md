# Kubernetes Workshop

- ใช้ Docker Desktop + Minikube (Local)
- ใช้ kubectl CLI
- ใช้ VS Code + Extension (Kubernetes, Docker)

## What is Containerization?

- Each container is repeatable and can be easily recreated with the same behavior.
- Deployment ง่ายและรวดเร็ว
- Each Node ใน Kubernetes cluster จะรัน containers ผ่าน Pods

## What is Kubernetes?

- Open-source system สำหรับ automate container deployment
- สามารถรัน container replicas จำนวนมากบนหลาย hosts ได้
- Kubernetes เป็นระดับที่สูงกว่า Docker เพราะสามารถจัดการ container หลายตัวพร้อมกันได้

## Kubernetes Features

- Automated rollouts, scaling, rollbacks และการจัดการ replicas อัตโนมัติตามทรัพยากร
- Service discovery, load balancing และ network ingress
- รองรับทั้ง stateless และ stateful applications
- Storage management และ declarative state (กำหนด desired state แล้ว Kubernetes จัดการให้)
- Works across environments และสามารถขยายความสามารถได้ง่าย

## Architecture of Kubernetes Cluster

- Control Plane ทำหน้าที่ควบคุมการทำงานของ cluster
- Nodes คือเครื่องที่ใช้รัน containers
- User และ Admin ใช้ kubectl ติดต่อกับ Control Plane เพื่อจัดการ cluster

## Kubernetes 101

- Nodes เป็น worker machines (VM หรือ physical machines)
- Pods คือหน่วยประมวลผลพื้นฐานของ Kubernetes (1 Pod มีได้หลาย containers แต่ best practice คือ 1 container)
- Deployments ใช้กำหนด desired state ของ Pods เช่น replicas และ image
- โครงสร้าง: Deployment → ReplicaSet → Pod → Container
- Service ใช้ expose Pods สู่ network (LoadBalancer, NodePort, ClusterIP)
- Labels และ Selectors ใช้จัดกลุ่มและเลือก resource
- Namespaces ใช้แบ่ง resource ภายใน cluster
- Kubelet ทำหน้าที่รันคำสั่งจาก Control Plane บน Node
- Kube-proxy จัดการ network traffic บน Node

## Hello World Application Workshop

### Create Minikube Cluster

```bash
minikube start
minikube dashboard
```

### Create Deployment และ Service

```bash
kubectl create deployment hello-world --image=nginx
kubectl expose deployment hello-world --type=LoadBalancer --port=8080 --target-port=80
kubectl get deployments
kubectl get services
```

### Scaling Application

```bash
kubectl scale --replicas=2 deployment hello-world
kubectl get pods
```

### Deployment Example

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello-world
spec:
  replicas: 1
  selector:
    matchLabels:
      app: hello-world
  template:
    metadata:
      labels:
        app: hello-world
    spec:
      containers:
      - name: hello-world-server
        image: nginx:alpine
        ports:
        - containerPort: 80
```

### Service Example

```yaml
apiVersion: v1
kind: Service
metadata:
  name: hello-world
spec:
  selector:
    app: hello-world
  ports:
  - port: 8080
    targetPort: 80
  type: LoadBalancer
```

### Application Secrets

- สามารถสร้าง Docker Registry Secret เพื่อดึง image จาก private registry
```bash
kubectl create secret docker-registry my-registry-secret --docker-username=<username> --docker-password=<password> --docker-email=<email>
```