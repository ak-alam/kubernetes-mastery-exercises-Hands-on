# Horizontal Pod Autoscaler (HPA)

This directory contains documentation and examples on how to set up Horizontal Pod Autoscaler (HPA) in Kubernetes. HPA automatically scales the number of pods in a deployment or replication controller based on observed CPU utilization (or, with custom metrics support, on some other application-provided metrics).

## Introduction to HPA

Horizontal Pod Autoscaler (HPA) is a Kubernetes feature that automatically scales the number of pods in a replication controller, deployment, replica set, or stateful set based on observed CPU utilization (or, with custom metrics support, on some other application-provided metrics).

## Prerequisites

Before setting up HPA in your Kubernetes cluster, ensure that you have the following prerequisites:

- A Kubernetes cluster running version 1.8 or later.
- The Kubernetes Metrics Server installed and running in your cluster.

#### Deploy the Metrics Server

Deploy the Metrics Server with the following command:

```sh
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

Verify that the metrics-server deployment is running the desired number of Pods with the following command:
```sh
kubectl get deployment metrics-server -n kube-system
```
An example output is as follows:

```sh
NAME             READY   UP-TO-DATE   AVAILABLE   AGE
metrics-server   1/1     1            1           6m
```


## Setup Guide

Follow these step-by-step instructions to configure HPA in your Kubernetes cluster:

1. Define resource requests and limits for your pods inside a deployment.
2. Create a service for your deployment (Only to test your deployment autoscaling).
3. Create an HPA resource for your deployment or replication controller.

#### 1. Define resource requests and limits for your pods inside a deployment.
The **deployment.yaml** file in the directory contains a sample deployment file to test our HPA. We have also defined our resource limit (cpu utilization) on container level as well.   
```sh
apiVersion: apps/v1
kind: Deployment
metadata:
  name: php-apache
spec:
  selector:
    matchLabels:
      run: php-apache
  template:
    metadata:
      labels:
        run: php-apache
    spec:
      containers:
      - name: php-apache
        image: registry.k8s.io/hpa-example
        ports:
        - containerPort: 80
        resources:
          limits:
            cpu: 500m
          requests:
            cpu: 200m
```

#### 2. Create a service for your deployment.

**service.yaml** file in the directory creates a service against the previously created deployment. We will use this service to test the HPA. 
```sh
apiVersion: v1
kind: Service
metadata:
  name: php-apache
  labels:
    run: php-apache
spec:
  ports:
  - port: 80
  selector:
    run: php-apache
```

#### 3. Create an HPA resource for your deployment or replication controller.

**hpa.yaml** file in the directory creates an HPA against the previously created deployment. This resource is responsible for scaling the deployment. We have specidifed the threshold of 50 %, which means if load on our deployment pods exceeds 50 % a new pod will be spun up. HPA scales the deployment between 1-10 pods as specified in the resource.
```sh
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: php-apache-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: php-apache
  minReplicas: 1
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      targetAverageUtilization: 50
```

We can also create HPA with the following single command as well:
```sh
kubectl autoscale deployment php-apache --cpu-percent=50 --min=1 --max=10
```

Describe the autoscaler with the following command to view its details.
```sh
kubectl get hpa
```
An example output is as follows.
```sh
NAME         REFERENCE               TARGETS   MINPODS   MAXPODS   REPLICAS   AGE
php-apache   Deployment/php-apache   0%/50%    1         10        1          51s
```


## Testing HPA
To test your HPA deployemnt follow the given steps:

#### 1. Create a load for the apache web server
Execute the following command to run a sample deployment to test the HPA by applying load on the previously deployed containers:
```sh
kubectl run -i \
    --tty load-generator \
    --rm --image=busybox \
    --restart=Never \
    -- /bin/sh -c "while sleep 0.01; do wget -q -O- http://php-apache; done"
```

#### 2. Watch the deployment scale out
To watch the deployment scale out, periodically run the following command in a separate terminal from the terminal that you ran the previous step in.
```sh
kubectl get hpa php-apache
```
An example output is as follows.
```sh
NAME         REFERENCE               TARGETS    MINPODS   MAXPODS   REPLICAS   AGE
php-apache   Deployment/php-apache   250%/50%   1         10        5          4m44s
```



## Examples

Here are some example configurations and usage scenarios for HPA:

- Scaling based on CPU utilization.
- Scaling based on custom metrics.
- Auto-scaling for different workloads (e.g., web servers, databases).

## Best Practices

To use HPA effectively in your Kubernetes cluster, consider the following best practices:

- Set appropriate resource requests and limits for your pods.
- Monitor and tune HPA settings based on workload characteristics.
- Test HPA configurations in staging environments before deploying to production.

## Contributing

Contributions to this HPA documentation are welcome! If you have improvements or new examples to add, please create a pull request. For major changes, please open an issue first to discuss what you would like to change.



