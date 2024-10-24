# 部署指南

## 1. 安裝 Kubernetes 和 Minikube

### 1.1 安裝 kubectl

```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
kubectl version --client
```

### 1.2 安裝 Minikube

```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64
minikube start
```

### 1.3 啟用 Ingress（可選）

為了更好地管理路由和域名，建議啟用 Ingress 控制器。

```bash
minikube addons enable ingress

```

## 2. 部署應用

### 2.1 進入 k8s 目錄

```bash
cd ~/server/k8s
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

## 4. 配置 LoadBalancer

### 4.1 啟動 Minikube Tunnel

為了讓 LoadBalancer 類型的服務在 Minikube 上可用，需啟動一個隧道：

```bash
minikube tunnel
```

### 4.2 獲取 LoadBalancer IP

等待 `nginx-service` 獲取外部 IP：

```bash
kubectl get services
```

## 5. 訪問應用

### 5.1 通過 IP 訪問

打開瀏覽器，訪問：

```
<http://xx.xx.xxx.xxx>

```

您應該能夠看到 `counterspell-event-client` 的應用界面。

### 5.2 配置域名（可選）

若要使用域名訪問，請將您的域名指向 `xx.xx.xxx.xxx`。然後，確保 Nginx 配置中的 `server_name` 設置為您的域名。

### 修改 Nginx ConfigMap

編輯 `nginx-configmap.yaml`，將 `server_name` 修改為您的域名：

```yaml
server {
    listen 80;
    server_name yourdomain.com;

    location / {
        proxy_pass <http://counterspell-event-client:4173>;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header X-Forwarded-Host $host;
        proxy_redirect off;
    }

    # 添加其他路徑的代理設定（如有需要）
}

```

應用更新後的 ConfigMap：

```bash
kubectl apply -f nginx-configmap.yaml
kubectl rollout restart deployment/nginx

```

> 提示：DNS 設置可能需要一些時間才能生效。
>
