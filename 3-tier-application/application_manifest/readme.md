# 3 Tier Application Deployment

## What is 3 Tier Application

3 tier application is a software partner, In which we have 3 layers. 
1. Frontend: Where client/users interact with the application.
2. Application: In the app layer, all the bussiness logic is implemented.
3. Database: Store the information and other relevent stuff about the software.

## Prerequisites

Before deploying this application, we need to have a kubernetes cluster setup and ready to roll.
- aws cli and kubectl configure to connect to cluster.
- Deploy Nginx Controller.
- Deploy Certificate Manager.

### Deploy Nginx controller

Deploy nginx controller using the following command.

```sh
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.10.0/deploy/static/provider/cloud/deploy.yaml
```

Verify the installation is complete using the following command.
```sh
kubectl get all -n ingress-nginx
```

### Deploy Certificate Manager

Install certificate manager to SSL/TLs using the following manfinest.

```sh
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.14.4/cert-manager.yaml
```


### Deploy Application 

In the application_manifest folder, We have all the available manfiest for frontend, backend, certificate configuration and mysql database. Deploy the application from application_manifest folder using the following command.

```sh
kubectl apply -f .
```

This will deploy everything in the application_manifest folder and to check if the pods are running or not using the following command.

```sh
kubectl get pod
```
It will list all the pods deployed in the default namespace.


### Access the fronend application.

We have yet not configured a domain for frontend application, we will using local port forwarding to access our frontend application.

List the all the services and copy the name of frontend service
```sh
kubectl get svc
```

Kube Port forward, replace the <frontend-service-name> with one you copied from above.

```sh
kubectl port-forward svc/<frontend-service-name> 3000:3000
```

Now open your browser and enter http://localhost:3000 








<!-- ## Create Generic secrets for mysql database
kubectl create secret generic db-secret --from-literal=MYSQL_DATABASE=article --from-literal=MYSQL_USER=user  --from-literal=MYSQL_PASSWORD=password --from-literal=MYSQL_ROOT_PASSWORD=password --dry-run=client -oyaml 


kubectl create secret generic backend-api-secret --from-literal=MYSQL_DB_HOST=mysql-sts-svc --from-literal=MYSQL_DB_USER=user  --from-literal=MYSQL_DB_PASSWORD=password --from-literal=MYSQL_DB_NAME=article --dry-run=client -oyaml 


CREATE TABLE articles (
    id INT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(100),
    body TEXT,
    date DATETIME DEFAULT CURRENT_TIMESTAMP
)
 -->
