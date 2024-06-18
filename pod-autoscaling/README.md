# Kubernetes Autoscaling Documentation

This directory contains information and step-by-step guides on setting up Horizontal Pod Autoscaler (HPA) and Vertical Pod Autoscaler (VPA) in Kubernetes. Autoscaling is a crucial aspect of Kubernetes that ensures your applications can handle varying loads by automatically adjusting the number of pods or the resources allocated to them.

## Directory Structure

The direcrory is organized into the following directories:

- **HPA/**: Contains information and steps to configure Horizontal Pod Autoscaler in Kubernetes.
- **VPA/**: Contains information and steps to configure Vertical Pod Autoscaler in Kubernetes.

## Horizontal Pod Autoscaler (HPA)

The HPA directory includes documentation and examples on how to set up Horizontal Pod Autoscaler in your Kubernetes cluster. HPA automatically scales the number of pods in a deployment or replication controller based on observed CPU utilization (or, with custom metrics support, on some other application-provided metrics).

### Contents

- **Introduction to HPA**: An overview of what HPA is and its benefits.
- **Prerequisites**: Requirements needed before setting up HPA.
- **Setup Guide**: Step-by-step instructions to configure HPA.
- **Testing HPA**: Testing HPA scaling with a sample deployment
- **Examples**: Example configurations and usage scenarios.
- **Best Practices**: Tips and best practices for using HPA effectively.

## Vertical Pod Autoscaler (VPA)

The VPA directory includes documentation and examples on how to set up Vertical Pod Autoscaler in your Kubernetes cluster. VPA automatically adjusts the resource limits and requests for containers in a pod based on historical and real-time metrics.

### Contents

- **Introduction to VPA**: An overview of what VPA is and its benefits.
- **Prerequisites**: Requirements needed before setting up VPA.
- **Setup Guide**: Step-by-step instructions to configure VPA.
- **Examples**: Example configurations and usage scenarios.
- **Best Practices**: Tips and best practices for using VPA effectively.

## Getting Started

1. Clone the repository to your local machine:
   ```sh
   git clone https://github.com/ak-alam/kubernetes-mastery-exercises-Hands-on.git
    ```
2. Navigate to the directory you are interested in:

   ```sh
    cd k8s-autoscaling-docs/HPA
   ```

    or 

   ```sh
    cd k8s-autoscaling-docs/VPA
   ``` 
3. Follow the instructions in the respective README files to set up HPA or VPA in your Kubernetes cluster.

Navigate to the directory you are interested in:


## Contributing
Contributions are welcome! If you have improvements or new examples to add, please create a pull request. For major changes, please open an issue first to discuss what you would like to change.

