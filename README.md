
# 🧠 Kubernetes Monitoring Project on AWS EKS

This project sets up a complete Kubernetes monitoring stack using **Amazon EKS**, **Prometheus**, and **Grafana**, with a custom web application deployed and monitored.

## 🔧 Technologies Used

- **Amazon EKS** – Managed Kubernetes cluster
- **eksctl** – CLI for creating EKS clusters
- **Helm** – Package manager for Kubernetes
- **Prometheus** – Metrics collection & alerting
- **Grafana** – Visualization and dashboards
- **Kubernetes Dashboard** – GUI for cluster management
- **PowerShell (Windows)** – for base64 decoding and port-forwarding
- **Custom App** – `vimal13/apache-webserver-php` (Web app)

---

## 🚀 Features

- ✅ EKS Cluster created via `eksctl`
- ✅ Web app deployed using Kubernetes `Deployment` and `Service`
- ✅ Prometheus deployed with Helm
- ✅ Grafana deployed and integrated with Prometheus
- ✅ Kubernetes Dashboard with admin access
- ✅ Cluster metrics visualized in Grafana
- ✅ Manual port-forwarding for local dashboard access

---

## 📂 Folder Structure


```
.
├── mywebd-deployment.yaml         # Web app Deployment
├── mywebd-service.yaml            # Web app Service (LoadBalancer)
├── monitoring/
│   ├── prometheus-values.yaml     # (optional) Custom Prometheus values
│   ├── grafana-dashboards/        # (optional) Grafana dashboard configs
└── README.md

```

---

## 📦 Setup Instructions

### 1. Create EKS Cluster

```bash
eksctl create cluster --name=mycluster --region=ap-south-1 --nodes=2
````

### 2. Deploy Web App

```bash
kubectl apply -f mywebd-deployment.yaml
kubectl expose deployment mywebd --type=LoadBalancer --port=80 --name=mywebd-service
```

### 3. Install Prometheus

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm install prometheus prometheus-community/prometheus --namespace monitoring --create-namespace --set server.persistentVolume.enabled=false
```

### 4. Install Grafana

```bash
helm repo add grafana https://grafana.github.io/helm-charts
helm install grafana grafana/grafana --namespace monitoring
```

### 5. Get Grafana Admin Password

```powershell
[System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String((kubectl get secret --namespace monitoring grafana -o jsonpath="{.data.admin-password}")))
```

### 6. Port-Forward Grafana Locally

```bash
kubectl port-forward -n monitoring svc/grafana 3000:80
```

Then open: [http://localhost:3000](http://localhost:3000)


## 📉 Metrics & Monitoring

* Prometheus: `http://localhost:9090` (via port-forward)
* Grafana: `http://localhost:3000` (default login: `admin / <your password>`)
* Web App: LoadBalancer DNS (check via `kubectl get svc`)
