# Kubernetes on Azure

Advanced Kubernetes on Azure Kubernetes Service (AKS). Terraform provisions the cluster; the `k8s/` demos cover service exposure and stateful workloads.

## Contents

| Path | What it covers |
| --- | --- |
| [`terraform/`](terraform/) | AKS cluster provisioning with Terraform (resource group, cluster, node pool, outputs) |
| [`k8s/demo1-acr-image/`](k8s/demo1-acr-image/) | Deploy the ACR image from the container-security pipeline; falls back to a public image if it is gone |
| [`k8s/demo2-loadbalancer/`](k8s/demo2-loadbalancer/) | Exposing a Deployment through a `LoadBalancer` Service with a public Azure IP |
| [`k8s/demo3-statefulset/`](k8s/demo3-statefulset/) | StatefulSet with stable network identity, headless Service, and ConfigMap |

Each directory has its own README with step-by-step instructions.

## Prerequisites

- An Azure subscription
- [Azure CLI](https://learn.microsoft.com/cli/azure/install-azure-cli)
- [Terraform](https://developer.hashicorp.com/terraform/downloads) >= 1.0
- [kubectl](https://kubernetes.io/docs/tasks/tools/)

## Quick start

```bash
az login
cd terraform
# set subscription_id in terraform.tfvars first
terraform init
terraform apply
az aks get-credentials --resource-group <resource-group> --name <cluster-name>
kubectl get nodes
```

Then run the demos:

```bash
kubectl apply -f k8s/demo1-acr-image/
kubectl apply -f k8s/demo2-loadbalancer/
kubectl apply -f k8s/demo3-statefulset/
```

## Cleanup

AKS clusters cost money while they run. Tear down when finished:

```bash
kubectl delete -f k8s/demo3-statefulset/ -f k8s/demo2-loadbalancer/ -f k8s/demo1-acr-image/
cd terraform && terraform destroy
```

## Note on `terraform.tfvars`

`subscription_id` is a placeholder. Replace it with your own subscription ID (`az account show --query id -o tsv`) before running Terraform.
