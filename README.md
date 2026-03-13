## 🚀 Kubernetes Observability Stack
Prometheus + Grafana + Alertmanager using K3s
## 📌 Project Overview

This project demonstrates how to build a complete observability stack on Kubernetes using:

Prometheus → Metrics collection

Grafana → Visualization dashboards

Alertmanager → Alert notifications

K3s → Lightweight Kubernetes cluster

Helm → Package manager for Kubernetes

The goal of this project is to monitor Kubernetes cluster metrics and visualize them using Grafana dashboards while generating alerts for system issues.
---

## 🏗️ Architecture Diagram
![](/image/Architechure%20daigram.png)

## 🛠️ Tech Stack
```
Tool	    Purpose
K3s	        Kubernetes cluster
Helm	    Kubernetes package manager
Prometheus	Metrics collection
Grafana	    Monitoring dashboards
Alertmanager	Alert system
```
---
## ⚙️ Project Setup (Step-by-Step)
#### Step 1 – Install K3s Kubernetes Cluster
```
curl -sfL https://get.k3s.io | sh -
```
 Check cluster:
```
sudo kubectl get nodes
```
#### Step 2 – Install Helm
```
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
```
Check version:
```
helm version
```
#### Step 3 – Create Observability Namespace
```
kubectl create namespace observability
```
#### Step 4 – Add Prometheus Helm Repository
```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```
#### Step 5 – Install Prometheus + Grafana Stack
```
helm upgrade --install prometheus prometheus-community/kube-prometheus-stack -n observability
```
#### Step 6 – Verify Pods
```
kubectl get pods -n observability
```
#### Example Output:
```
alertmanager-prometheus-kube-prometheus-alertmanager-0   Running
prometheus-grafana-xxxx                                  Running
prometheus-kube-prometheus-operator                      Running
prometheus-kube-state-metrics                            Running
prometheus-prometheus-kube-prometheus-prometheus-0       Running
prometheus-node-exporter                                 Running
```
---

## 📊 Access Grafana Dashboard

#### Run port-forward:
```
kubectl -n observability port-forward svc/prometheus-grafana 3000:80
```
#### Open browser:
```
http://localhost:3000
```
## Get Grafana Admin Password
```
kubectl -n observability get secret prometheus-grafana -o jsonpath="{.data.admin-password}" | base64 --decode
```
#### Login:
```
Username : admin
Password : (command output)
```
## 🚨 Create Prometheus Alert Rule

Create file:
```
nano cpu-alert.yaml
```
![](/image/Screenshot%20(296).png)

Add:
```
apiVersion: monitoring.coreos.com/v1
kind: PrometheusRule
metadata:
  name: example-alerts
  namespace: observability
spec:
  groups:
  - name: example
    rules:
    - alert: HighCPU
      expr: 100 - (avg by(instance) (rate(node_cpu_seconds_total{mode="idle"}[5m])) * 100) > 80
      for: 2m
      labels:
        severity: warning
      annotations:
        summary: "High CPU usage detected"
```
Apply rule:
```
kubectl apply -f cpu-alert.yaml
```
## 📸 Project Screenshots
### 1️⃣ Kubernetes Cluster
```
kubectl get nodes -o wide
```
📷 Screenshot here
![](/image/Screenshot%20(282).png)
---
### 2️⃣ Running Pods
```
kubectl get pods -n observability
```
📷 Screenshot here
![](/image/Screenshot%20(294).png)
---
### 3️⃣ Grafana Dashboard

Grafana → Dashboards → Node Exporter

📷 Screenshot here
![](/image/Screenshot%20(291).png)

### 4️⃣ Alertmanager Alerts

Grafana → Alerting → Alerts

📷 Screenshot here
![](/image/Screenshot%20(293).png)

## 📂 Project Structure

```
kubernetes-observability/
│
├── README.md
├── cpu-alert.yaml
│
└── screenshots
    ├── k8s-nodes.png
    ├── observability-pods.png
    ├── grafana-dashboard.png
    └── alerts.png

 ```

## 🎯 Key Features

✔ Kubernetes monitoring
✔ Prometheus metrics collection
✔ Grafana dashboards
✔ CPU alert rule setup
✔ Alertmanager integration

## 📚 Learning Outcomes

Through this project we learned:

Kubernetes monitoring architecture

Prometheus metrics collection

Grafana dashboard visualization

Alert rule configuration

Observability best practices

## 👨‍💻 Author

Vaibhav Bhuse

DevOps | Cloud | Kubernetes Enthusiast