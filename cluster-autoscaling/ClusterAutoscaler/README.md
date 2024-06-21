# Kubernetes Cluster Autoscaler

This directory contains documentation and examples on how to set up the Kubernetes Cluster Autoscaler. The Cluster Autoscaler automatically adjusts the size of the Kubernetes cluster based on the resources required by the pods.

## Introduction to Cluster Autoscaler

The Kubernetes Cluster Autoscaler is a tool that automatically adjusts the size of the cluster when there are pods that cannot be scheduled due to insufficient resources, or when there are nodes that have been underutilized for a long period. This helps in optimizing resource usage and costs.

## Prerequisites

Before setting up the Cluster Autoscaler in your Kubernetes cluster, ensure that you have the following prerequisites:

- A Kubernetes cluster running version 1.12 or later.
- A cloud provider that supports cluster autoscaling (e.g., AWS, GCP, Azure).

## Setup Guide

Follow these step-by-step instructions to configure the Cluster Autoscaler in your Kubernetes cluster:

1. Creation of an IAM policy for Cluster Autoscalar.
2. Creation of Service Account in EKS.
3. Deployment of Cluster Autoscalar.

#### 1. Creation of an IAM policy for Cluster Autoscalar.
Create an IAM policy that will allow Autoscalar to access autoscaling groups. Required permissions can be seen in the **cluster-autoscalar.json** file:
```sh
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "autoscaling:DescribeAutoScalingGroups",
                "autoscaling:DescribeAutoScalingInstances",
                "autoscaling:DescribeLaunchConfigurations",
                "autoscaling:DescribeTags",
                "autoscaling:SetDesiredCapacity",
                "autoscaling:TerminateInstanceInAutoScalingGroup",
                "ec2:DescribeLaunchTemplateVersions"
            ],
            "Resource": "*",
            "Effect": "Allow"
        }
    ]
}
```
To create the policy use the following command:
```sh
aws iam create-policy --policy-name ClusterAutoscalerPolicy --policy-document file://cluster-autoscalar.json
```

#### 2. Creation of Service Account in EKS.
Use the eksctl command to create an IAM service account. Attach the policy created in the previous step to this service account. This service account will allow the cluster autoscalar to access the desired resources.
```sh
eksctl create iamserviceaccount \
  --name cluster-autoscaler \
  --namespace kube-system \
  --cluster YOUR-CLUSTER-NAME \
  --role-name your-role-name \
  --attach-policy-arn arn:aws:iam::YOUR-ACCOUNT-ID:policy/ClusterAutoscalerPolicy \
  --approve
```
#### 3. Deployment of Cluster Autoscalar
To deploy the cluster autoscalar use the **cluster-autoscalar.yaml** deployment file. Update the cluster name in the `node-group-auto-discovery` before deploying the file to the cluster.
```sh
          command: 
            - ./cluster-autoscaler
            - --v=4
            - --stderrthreshold=info
            - --cloud-provider=aws
            - --skip-nodes-with-local-storage=false
            - --expander=least-waste
            - --node-group-auto-discovery=asg:tag=k8s.io/cluster-autoscaler/enabled,k8s.io/cluster-autoscaler/CLUSTER_NAME # Update cluster
            - --balance-similar-node-groups
            - --skip-nodes-with-system-pods=false
```

After udpating the cluster name apply the deployment to the cluster.
```sh
kubectl apply -f cluster-autoscalar.yaml
```


#### Important Note
We have created a service account with the name `cluster-autoscaler` with the help of eksctl command. IF you want to change it we have to update the `serviceAccount` name in the `cluster-autoscalar.yaml` deployment file as well. `serviceAccount` name should be updated at the following occurances: 

**i.** Inside the `deployment` config:
```yaml
    spec:
      serviceAccountName: cluster-autoscaler
      containers:
```

**ii.** Inside the `ClusterRoleBinding` config:
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: cluster-autoscaler
  labels:
    k8s-addon: cluster-autoscaler.addons.k8s.io
    k8s-app: cluster-autoscaler
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-autoscaler
subjects:
  - kind: ServiceAccount
    name: cluster-autoscaler
    namespace: kube-system
```

**iii.** Inside the `RoleBinding` config:
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: cluster-autoscaler
  namespace: kube-system
  labels:
    k8s-addon: cluster-autoscaler.addons.k8s.io
    k8s-app: cluster-autoscaler
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: cluster-autoscaler
subjects:
  - kind: ServiceAccount
    name: cluster-autoscaler
    namespace: kube-system
```


## Examples

Here are some example configurations and usage scenarios for the Cluster Autoscaler:

- Scaling based on CPU and memory usage.
- Handling sudden spikes in traffic.
- Optimizing resource usage and costs.

## Best Practices

To use the Cluster Autoscaler effectively in your Kubernetes cluster, consider the following best practices:

- Regularly monitor the Cluster Autoscaler logs and metrics.
- Test the Cluster Autoscaler configurations in a staging environment before deploying to production.
- Adjust the scaling parameters based on your application requirements and traffic patterns.

## Contributing

Contributions to this Cluster Autoscaler documentation are welcome! If you have improvements or new examples to add, please create a pull request. For major changes, please open an issue first to discuss what you would like to change.
