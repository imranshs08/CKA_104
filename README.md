# AKS Lab Setup Guide (CKA_104)

This guide provides step-by-step Azure CLI commands to spin up an AKS cluster within the KodeKloud Azure Playground limits.

## Prerequisites
- Active KodeKloud Azure Playground session.
- Azure CLI installed.

## Authentication

### Option 1: Interactive Login
If you are running these commands from your local PowerShell or terminal:
```powershell
az login
```

### Option 2: Service Principal Login (Playground Credentials)
If you have a **Client ID** and **Client Secret** from the playground:
```powershell
# Define credentials
$CLIENT_ID="your-application-client-id"
$CLIENT_SECRET="your-client-secret"
$TENANT_ID="your-tenant-id" # Usually found in the portal URL or overview

# Log in using Service Principal
az login --service-principal -u $CLIENT_ID -p $CLIENT_SECRET --tenant $TENANT_ID
```

> [!TIP]
> If you have multiple subscriptions, set the active one:
> `az account set --subscription "Your-Subscription-ID"`

## Step-by-Step Setup

### 1. Create the AKS Cluster
We use 2 nodes and `Standard_D2s_v3` size to comply with playground policies. The Resource Group name is fetched dynamically.
```bash
# Define variables
RG_NAME=$(az group list --query "[0].name" -o tsv)
CLUSTER_NAME="myAKSCluster"

# Create the cluster
az aks create \
    --resource-group $RG_NAME \
    --name $CLUSTER_NAME \
    --node-count 2 \
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
