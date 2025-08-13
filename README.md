# Monitoring of Kubernetes cluster with Prometheus, Grafana using Helm


# 1️⃣ Cluster Setup
Use eksctl to create cluster:
```bash
eksctl create cluster --name mycluster --region ap-south-1 --nodes-min 2 --nodes-max 2
kubectl get nodes
```

# 2️⃣ Install Helm3:-
```bash
curl -fsSl -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
helm version
```
```bash
helm repo ls   # check if metrics server is present in cluster
```

# 3️⃣ Prometheus & Grafana via Helm
Add Helm repos:
```bash
helm repo add stable https://charts.helm.sh/stable   # stable helm repo added in helm chart

helm repo add prometheus-community https://prometheus-community.github.io/helm-charts   # Add the Prometheus repository to HELM
```

# 4️⃣ Install Prometheus & Grafana inside the cluster:

```bash
helm install stable prometheus-community/kube-prometheus-stack   # Prometheus/Grafana run as pods in the cluster
```


Pod / Application Metrics (annotations required)
Add annotations to Deployment YAML :

```yaml
annotations:
  prometheus.io/scrape: "true"
  prometheus.io/port: "8080"
```


where to add :- Usually in the Deployment YAML under metadata → annotations in the pod template:
```yaml
spec:
  template:
    metadata:
      annotations:
        prometheus.io/scrape: "true"
        prometheus.io/port: "8080"
```
  
  
Prometheus will scrape metrics only from annotated pods.
Without these annotations, Prometheus will not scrape metrics from that pod.

every new app you want to monitor must have these annotations 



# 5️⃣ you can access Prometheus and Grafana using the LoadBalancer URL:

Services will be of type ClusterIP by default. You can edit them to LoadBalancer to access via browser.
```bash
kubectl get svc
```

Prometheus URL: http://LBR-URL:9090/
Grafana URL: http://LBR-URL:3000/


Grafana Login using default credentials:

Username: admin

Password: prom-operator














