## Cert Manager in EKS


### Install Nginx Ingress controller

```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.10.0/deploy/static/provider/cloud/deploy.yaml
```

### Install Cert Manager
```
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.14.4/cert-manager.yaml
```

### Apply CertIssuser and Certificate Configs

```
kubectl apply -f application_manifest/cert-configs.yaml
```

### Create a demo deployment, service and ingress 

```
kubectl apply -f application_manifest/monitor-app.yaml
```
