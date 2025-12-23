# AKS Lab Setup Guide (CKA_104)

This guide provides step-by-step Azure CLI commands to spin up an AKS cluster within the KodeKloud Azure Playground limits.

## Prerequisites
- Active KodeKloud Azure Playground session.
- Azure CLI installed (pre-installed in the playground terminal).

## Step-by-Step Setup

### 1. Define Variables
Set these variables to make the commands easier to run.
```bash
# Dynamically fetch the existing Resource Group name
RG_NAME=$(az group list --query "[0].name" -o tsv)
LOCATION="eastus"
CLUSTER_NAME="myAKSCluster"
```

### 2. Create a Resource Group
```bash
az group create --name $RG_NAME --location $LOCATION
```

### 3. Create the AKS Cluster
We use 1 node and `Standard_D2s_v3` size to comply with playground policies.
```bash
az aks create \
    --resource-group $RG_NAME \
    --name $CLUSTER_NAME \
    --node-count 1 \
    --node-vm-size Standard_D2s_v3 \
    --generate-ssh-keys
```

### 4. Get Cluster Credentials
Configure `kubectl` to connect to your new cluster.
```bash
az aks get-credentials --resource-group $RG_NAME --name $CLUSTER_NAME
```

### 5. Verify the Setup
Check if your nodes are up and running.
```bash
kubectl get nodes
```

## Cleanup
To delete the AKS cluster and free up resources:
```bash
# Define variables
RG_NAME=$(az group list --query "[0].name" -o tsv)
CLUSTER_NAME="myAKSCluster"

# Delete the AKS cluster
az aks delete --resource-group $RG_NAME --name $CLUSTER_NAME --yes --no-wait
```
