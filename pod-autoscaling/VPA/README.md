# Vertical Pod Autoscaler (VPA)

This directory contains documentation and examples on how to set up Vertical Pod Autoscaler (VPA) in Kubernetes. VPA automatically adjusts the resource limits and requests for containers in a pod based on historical and real-time metrics.

## Introduction to VPA

Vertical Pod Autoscaler (VPA) is a Kubernetes feature that automatically adjusts the CPU and memory resource limits and requests for containers in a pod based on historical and real-time metrics. This helps to optimize resource utilization and improve application performance.

## Prerequisites

Before setting up VPA in your Kubernetes cluster, ensure that you have the following prerequisites:

- A Kubernetes cluster running version 1.10 or later.
- The Metrics Server installed and running in your cluster.
- The Custom Metrics API enabled in your cluster (optional, for custom metrics support).
- You are using a kubectl client that is configured to communicate with your Kubernetes cluster.

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

Follow these step-by-step instructions to configure VPA in your Kubernetes cluster:

1. Deployment of VPA components on the kubernetes cluster.
2. Define resource limits and requests for your containers.
3. Create a VPA resource for your pods.


#### 1. Deployment of VPA components on the kubernetes cluster
**i.** To deploy the Vertical Pod Autoscaler, clone the kubernetes/autoscaler GitHub repository.
```sh
git clone https://github.com/kubernetes/autoscaler.git
```
**ii.** Change to the vertical-pod-autoscaler directory.
```sh
cd autoscaler/vertical-pod-autoscaler/
```
**iii.** (Optional) If you have already deployed another version of the Vertical Pod Autoscaler, remove it with the following command.
```sh
./hack/vpa-down.sh
```
**iv.** Deploy the Vertical Pod Autoscaler to your cluster with the following command.
```sh
./hack/vpa-up.sh
```
**v.** Verify that the Vertical Pod Autoscaler Pods have been created successfully.
```sh
kubectl get pods -n kube-system
```
An example output is as follows:
```sh
NAME                                        READY   STATUS    RESTARTS   AGE
[...]
metrics-server-8459fc497-kfj8w              1/1     Running   0          83m
vpa-admission-controller-68c748777d-ppspd   1/1     Running   0          7s
vpa-recommender-6fc8c67d85-gljpl            1/1     Running   0          8s
vpa-updater-786b96955c-bgp9d                1/1     Running   0          8s
```


#### 2. Define resource limits and requests for your containers.
Deploy the **deployment.yaml** sample deployment file in the directory:
```sh
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hamster
spec:
  selector:
    matchLabels:
      app: hamster
  replicas: 2
  template:
    metadata:
      labels:
        app: hamster
    spec:
      securityContext:
        runAsNonRoot: true
        runAsUser: 65534 # nobody
      containers:
        - name: hamster
          image: registry.k8s.io/ubuntu-slim:0.1
          resources:
            requests:
              cpu: 100m
              memory: 50Mi
          command: ["/bin/sh"]
          args:
            - "-c"
            - "while true; do timeout 0.5s yes >/dev/null; sleep 0.5s; done"
```

#### 3. Create a VPA resource for your pods.
Deploy the **vpa.yaml** sample vpa file in the directory:
```sh
apiVersion: "autoscaling.k8s.io/v1"
kind: VerticalPodAutoscaler
metadata:
  name: hamster-vpa
spec:
  targetRef:
    apiVersion: "apps/v1"
    kind: Deployment
    name: hamster
  resourcePolicy:
    containerPolicies:
      - containerName: '*'
        minAllowed:
          cpu: 100m
          memory: 50Mi
        maxAllowed:
          cpu: 1
          memory: 500Mi
        controlledResources: ["cpu", "memory"]
```


## Testing VPA
To test your HPA deployemnt follow the given steps:

#### 1. Describe deployment pods for which VPA is configured   
To test whether the VPA is working or not, describe the deployment pods for which VPA was configured. As we can see, that in our deployment we have specified the pods resource requests to be 100m (for cpu) & 50Mi (for memory). 
```sh
      containers:
        - name: hamster
          image: registry.k8s.io/ubuntu-slim:0.1
          resources:
            requests:
              cpu: 100m
              memory: 50Mi
```
For this example application, 100 millicpu is less than the Pod needs to run, so it is CPU-constrained. It also reserves much less memory than it needs. The Vertical Pod Autoscaler **vpa-recommender** deployment analyzes the **hamster Pods** to see if the CPU and memory requirements are appropriate. If adjustments are needed, the **vpa-updater** relaunches the Pods with updated values.

#### 2. Wait for new pods   
Wait for the vpa-updater to launch a new hamster Pod. This should take a minute or two. You can monitor the Pods with the following command.
```sh
kubectl get --watch Pods -l app=hamster
```

#### 3. Describe new pods (To see the updated resources allocated by VPA)
When a new hamster Pod is started, describe it and view the updated CPU and memory allocation. VPA will adjust the cpu & memory allocations for the deployment pods.
```sh
kubectl describe pod hamster-xxxxxxxxx-xxxxx
```
An example output is as follows:
```sh
[...]
Containers:
  hamster:
    Container ID:  docker://xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    Image:         registry.k8s.io/ubuntu-slim:0.1
    Image ID:      docker-pullable://registry.k8s.io/ubuntu-slim@sha256:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    Port:          <none>
    Host Port:     <none>
    Command:
      /bin/sh
    Args:
      -c
      while true; do timeout 0.5s yes >/dev/null; sleep 0.5s; done
    State:          Running
      Started:      Fri, 19 Jun 2024 10:37:08 -0700
    Ready:          True
    Restart Count:  0
    Requests:
      cpu:        587m
      memory:     262144k
[...]
```

## Examples

Here are some example configurations and usage scenarios for VPA:

- Optimizing resource utilization for different workloads (e.g., CPU-bound vs. memory-bound).
- Handling spikes in traffic or load fluctuations.
- Fine-tuning resource allocation based on application requirements.

## Best Practices

To use VPA effectively in your Kubernetes cluster, consider the following best practices:

- Regularly monitor VPA recommendations and adjust resource limits accordingly.
- Test VPA configurations in staging environments before deploying to production.
- Keep an eye on application performance metrics to ensure optimal resource utilization.

## Contributing

Contributions to this VPA documentation are welcome! If you have improvements or new examples to add, please create a pull request. For major changes, please open an issue first to discuss what you would like to change.


