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
We use 2 nodes and `Standard_D2s_v3` size to comply with playground policies. Monitoring is disabled as required.
```bash
az aks create \
    --resource-group $RG_NAME \
    --name $CLUSTER_NAME \
    --node-count 2 \
    --node-vm-size Standard_D2s_v3 \
    --generate-ssh-keys \
    --disable-monitoring
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
To avoid hitting playground limits or for a fresh start:
```bash
az group delete --name $RG_NAME --yes --no-wait
```
