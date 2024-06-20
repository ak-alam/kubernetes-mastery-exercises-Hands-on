# Kubernetes Cluster Autoscaling Documentation

This repository contains comprehensive documentation and step-by-step guides for setting up cluster autoscaling in Kubernetes. It covers two main components: the Kubernetes Cluster Autoscaler and Karpenter.

## Directory Structure

The repository is organized into the following directories:

- **ClusterAutoscaler/**: Contains information and steps to configure the Kubernetes Cluster Autoscaler.
- **Karpenter/**: Contains information and steps to configure Karpenter for cluster autoscaling.

## Kubernetes Cluster Autoscaler

The `ClusterAutoscaler` directory includes documentation and examples on how to set up the Kubernetes Cluster Autoscaler. The Cluster Autoscaler automatically adjusts the size of the cluster when there are pods that can't be scheduled due to resource constraints, or when there are nodes that have been underutilized for a long period.

### Contents

- **Introduction to Cluster Autoscaler**: An overview of what Cluster Autoscaler is and its benefits.
- **Prerequisites**: Requirements needed before setting up Cluster Autoscaler.
- **Setup Guide**: Step-by-step instructions to configure Cluster Autoscaler.
- **Examples**: Example configurations and usage scenarios.
- **Best Practices**: Tips and best practices for using Cluster Autoscaler effectively.

## Karpenter

The `Karpenter` directory includes documentation and examples on how to set up Karpenter. Karpenter is an open-source node provisioning project built for Kubernetes. It simplifies Kubernetes infrastructure by replacing the cluster autoscaler with more efficient and cost-effective scaling mechanisms.

### Contents

- **Introduction to Karpenter**: An overview of what Karpenter is and its benefits.
- **Prerequisites**: Requirements needed before setting up Karpenter.
- **Setup Guide**: Step-by-step instructions to configure Karpenter.
- **Examples**: Example configurations and usage scenarios.
- **Best Practices**: Tips and best practices for using Karpenter effectively.

## Getting Started

1. Clone the repository to your local machine:
   ```sh
   git clone https://github.com/ak-alam/kubernetes-mastery-exercises-Hands-on.git
   ```
2. Navigate to the directory you are interested in:
```sh
cd k8s-autoscaling-docs/ClusterAutoscaler
```
or
```sh
cd k8s-autoscaling-docs/Karpenter
```

3. Follow the instructions in the respective README files to set up Cluster Autoscaler or Karpenter in your Kubernetes cluster.

   
## Contributing
Contributions are welcome! If you have improvements or new examples to add, please create a pull request. For major changes, please open an issue first to discuss what you would like to change.