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

1. Define resource limits and requests for your containers.
2. Deploy the Vertical Pod Autoscaler components.
3. Create a VPA resource for your pods.

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


