
# 🧠 Kubernetes Monitoring Project on AWS EKS

This project sets up a complete Kubernetes monitoring stack using **Amazon EKS**, **Prometheus**, and **Grafana**, with a custom web application deployed and monitored.

<img width="1920" height="1080" alt="Screenshot (1156)" src="https://github.com/user-attachments/assets/c86fa1ee-47cd-4513-970d-c9f151e56732" />
<img width="1920" height="1080" alt="Screenshot (1153)" src="https://github.com/user-attachments/assets/872ca1bb-5ffa-4986-a093-21a534e5981b" />
<img width="1920" height="1080" alt="Screenshot (1152)" src="https://github.com/user-attachments/assets/8c5f2882-33cf-4b66-b626-a7c0d39c036b" />
<img width="1920" height="1080" alt="Screenshot (1150)" src="https://github.com/user-attachments/assets/dfbc7b7c-9e0b-4445-9265-e853d38da9a0" />
<img width="1920" height="1080" alt="Screenshot (1149)" src="https://github.com/user-attachments/assets/62bcf2e7-5adf-4530-aef1-999ba1d609c0" />
<img width="1920" height="1080" alt="Screenshot (1162)" src="https://github.com/user-attachments/assets/cb4f7d48-b2b7-483b-add1-61b5b8ee2184" />
<img width="1920" height="1080" alt="Screenshot (1161)" src="https://github.com/user-attachments/assets/360b6aac-e60c-4ddf-8206-e4831bc358dc" />
<img width="1920" height="1080" alt="Screenshot (1160)" src="https://github.com/user-attachments/assets/22086705-d3d2-4df1-972f-2d22f0e8639f" />
<img width="1920" height="1080" alt="Screenshot (1159)" src="https://github.com/user-attachments/assets/3eb6e2ce-4968-4d69-8fc6-1ae49ae69619" />
<img width="1920" height="1080" alt="Screenshot (1158)" src="https://github.com/user-attachments/assets/94b2fc78-7891-4441-b2d9-f6fdeff928bb" />
<img width="1920" height="1080" alt="Screenshot (1157)" src="https://github.com/user-attachments/assets/b06c4d97-9933-4c01-b48f-d3df9c50326f" />
<img width="1920" height="1080" alt="Screenshot (1163)" src="https://github.com/user-attachments/assets/8b25b877-ccf4-4efc-9821-aeb000b62743" />



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
