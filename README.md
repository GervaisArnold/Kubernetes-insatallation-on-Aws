# Kubernetes Installation on AWS

This project provides step-by-step instructions for deploying Kubernetes on AWS using Kops.

## Prerequisites

1. An AWS account
2. AWS CLI installed on your local machine
3. Kops CLI installed on your local machine
4. A registered domain name
5. A configured S3 bucket for storing Kubernetes state

## Installation Steps

### 1. Create an S3 bucket

Create an S3 bucket to store Kubernetes state.

```sh
aws s3api create-bucket --bucket <bucket_name> --region <region> --create-bucket-configuration LocationConstraint=<region>
```

### 2. Export environment variables

Export the following environment variables on your local machine.

```sh
export KOPS_STATE_STORE=s3://<bucket_name>
export NAME=<cluster_name>
export KOPS_FEATURE_FLAGS=AlphaAllowGCE
export AWS_DEFAULT_REGION=<region>
```

### 3. Create an SSH key pair

Create an SSH key pair for accessing the Kubernetes nodes.

```sh
ssh-keygen -t rsa -N '' -f ~/.ssh/id_rsa
```

### 4. Create a Kubernetes cluster configuration

Create a Kubernetes cluster configuration using Kops.

```sh
kops create cluster \
--zones=<zones> \
--node-count=<node_count> \
--node-size=<node_size> \
--master-size=<master_size> \
--dns-zone=<dns_zone> \
--ssh-public-key=~/.ssh/id_rsa.pub \
--name=$NAME \
--cloud=aws
```

### 5. Update the Kubernetes cluster configuration

Update the Kubernetes cluster configuration to enable features such as the Kubernetes dashboard.

```sh
kops edit cluster --name $NAME
```

### 6. Validate the Kubernetes cluster configuration

Validate the Kubernetes cluster configuration.

```sh
kops validate cluster --wait 10m
```

### 7. Deploy the Kubernetes cluster

Deploy the Kubernetes cluster using Kops.

```sh
kops update cluster --name $NAME --yes
```

### 8. Validate the Kubernetes cluster

Validate the Kubernetes cluster using Kops.

```sh
kops validate cluster
```

### 9. Access the Kubernetes dashboard

Access the Kubernetes dashboard using the following command.

```sh
kubectl proxy
```

Then, open a browser and navigate to the following URL.

```sh
http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/
```

## Conclusion

This project provides a simple and easy way to deploy Kubernetes on AWS using Kops. By following the above steps, you can easily deploy and manage your Kubernetes cluster on AWS.
