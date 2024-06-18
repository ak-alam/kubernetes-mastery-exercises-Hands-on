# Horizontal Pod Autoscaler (HPA)

This directory contains documentation and examples on how to set up Horizontal Pod Autoscaler (HPA) in Kubernetes. HPA automatically scales the number of pods in a deployment or replication controller based on observed CPU utilization (or, with custom metrics support, on some other application-provided metrics).

## Introduction to HPA

Horizontal Pod Autoscaler (HPA) is a Kubernetes feature that automatically scales the number of pods in a replication controller, deployment, replica set, or stateful set based on observed CPU utilization (or, with custom metrics support, on some other application-provided metrics).

## Prerequisites

Before setting up HPA in your Kubernetes cluster, ensure that you have the following prerequisites:

- A Kubernetes cluster running version 1.8 or later.
- The Kubernetes Metrics Server installed and running in your cluster.

### Deploy the Metrics Server

Deploy the Metrics Server with the following command:

```sh
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

## Setup Guide

Follow these step-by-step instructions to configure HPA in your Kubernetes cluster:

1. Define resource requests and limits for your pods.
2. Enable metrics collection in your Kubernetes cluster.
3. Create an HPA resource for your deployment or replication controller.

Verify that the metrics-server deployment is running the desired number of Pods with the following command:
```sh
kubectl get deployment metrics-server -n kube-system
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


