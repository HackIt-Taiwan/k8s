# 部署

## 1. 安裝 Kubernetes

### 1.1 安裝 kubectl

```bash
sudo apt update
sudo apt install -y apt-transport-https
curl -s <https://packages.cloud.google.com/apt/doc/apt-key.gpg> | sudo apt-key add -
echo "deb <https://apt.kubernetes.io/> kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo apt update
sudo apt install -y kubectl

```

## 2. 部署應用

1. 進入 k8s 目錄：
    
    ```bash
    cd /path/to/your/project/k8s
    ```
    
2. 按順序應用配置：
    
    ```bash
    kubectl apply -f nginx-configmap.yaml
    kubectl apply -f counterspell-event-client-deployment.yaml
    kubectl apply -f counterspell-event-client-service.yaml
    kubectl apply -f nginx-deployment.yaml
    kubectl apply -f nginx-service.yaml
    kubectl apply -f counterspell-event-client-cronjob.yaml
    ```
    

## 3. 查看狀態

- 查看 Pods：
    
    ```bash
    kubectl get pods
    ```
    
- 查看服務：
    
    ```bash
    kubectl get services
    ```
    
- 查看 CronJobs：
    
    ```bash
    kubectl get cronjobs
    ```