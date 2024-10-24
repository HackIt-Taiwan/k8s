# 部署指南

## 1. 安裝 Kubernetes 和 Minikube

### 1.1 安裝 kubectl

```bash
curl -LO "<https://dl.k8s.io/release/$>(curl -L -s <https://dl.k8s.io/release/stable.txt>)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
kubectl version --client

```

### 1.2 安裝 Minikube

```bash
curl -LO <https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64>
sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64
minikube start
```

## 2. 部署應用

### 2.1 進入 k8s 目錄

```bash
cd /path/to/your/project/k8s
```

### 2.2 按順序應用配置

```bash
kubectl apply -f nginx-configmap.yaml
kubectl apply -f counterspell-event-client-deployment.yaml
kubectl apply -f counterspell-event-client-service.yaml
kubectl apply -f nginx-deployment.yaml
kubectl apply -f nginx-service.yaml
kubectl apply -f counterspell-event-client-cronjob.yaml
```

## 3. 驗證服務

### 3.1 查看 Pods 狀態

```bash
kubectl get pods
```

### 3.2 查看服務

```bash
kubectl get services
```

### 3.3 查看 CronJobs

```bash
kubectl get cronjobs
```

## 4. 訪問應用

- 確保 Minikube 使用正確的 IP 和端口轉發：
    
    ```bash
    minikube service nginx-service

    ```