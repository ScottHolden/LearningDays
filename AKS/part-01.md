# Part 2 - Taking it to production
## Overview
In this section we will cover off common scenarios for advanced use of Azure OpenAI, including steps to production, RAG patterns & embeddings, and security techniques.

Each steps contain the following sections:
- **Aim**: The goal of the step, will instruct what to do but not how
- **Questions**: A set of follow-up questions to self-test your knowledge
- **Resources**: Documentation links to help guide you if you get stuck
- **Stretch**: A secondary goal to either push yourself, or revisit later

## Step 1 - Deploying AKS
### Aim
Deploy an AKS cluster, it must have the following configuration:
- System & user node pools (separate)
- Azure CNI
- Microsoft Entra ID with Azure RBAC for Kubernetes Authorization

### Questions
- Why would you separate system and user node pools?
- How many nodes should be in the system pool? How many in the user pool?
- What is the difference between Azure CNI and Kubenet?
- What is the benefit of using Managed Identity over Service Principal when configuring Microsoft Entra ID for AKS?

### Resources
- https://learn.microsoft.com/en-us/azure/architecture/reference-architectures/containers/aks-start-here
- https://learn.microsoft.com/en-us/azure/aks/learn/quick-kubernetes-deploy-portal?tabs=azure-cli
- https://learn.microsoft.com/en-us/azure/aks/azure-cni-overview
- https://learn.microsoft.com/en-us/azure/aks/operator-best-practices-identity

### Stretch
Write an ARM/Bicep/Terraform template to deploy an AKS cluster with the above configuration.

## Step 2 - Scaling/adding Node Pools
### Aim
Add a new node pool to your AKS cluster, and scale it to 3 nodes. Once running, scale it back down to 1 node.

### Questions
- What VM sizes could you pick for the new node pool?
- Can a node pool be placed onto a new subnet?
- What operating systems were supported for the new node pool? Is this different from the system node pool?
- How long did scaling take for the node pool?

### Resources
- https://learn.microsoft.com/en-us/azure/aks/create-node-pools
- https://learn.microsoft.com/en-us/azure/aks/manage-node-pools

### Stretch
Add the additional node pool to your template from step 1, with a default scale of 3 nodes.

## Step 3 - Setting up Cluster autoscaling
### Aim
Configure the cluster autoscaler for your AKS cluster, and test it by deploying a workload that requires more nodes than are currently available. (You can use the sample application provided in the portal)

### Questions
- How long did it take for the cluster to scale up?
- What was the status of the pods during the scaling?
- What is the difference between the cluster autoscaler and the horizontal pod autoscaler?

### Resources
- https://learn.microsoft.com/en-us/azure/aks/cluster-autoscaler-overview
- https://learn.microsoft.com/en-us/azure/aks/concepts-scale
- https://learn.microsoft.com/en-us/azure/aks/cluster-autoscaler?tabs=azure-cli

### Stretch
Implement a HPA (Horizontal Pod Autoscaler) for the sample application, and get the cluster to scale based on the application's load. Consider using Azure Load Test to generate load.

## Step 4 - Configure Workload Identity
### Aim
Configure Microsoft Entra Workload ID on your AKS cluster and deploy a sample application that uses it to access an Azure Key Vault (eg: ghcr.io/azure/azure-workload-identity/msal-go). Use kubectl logs to show that it is working.

### Questions
- Why is giving applications an identity different on AKS than other Azure PaaS resources?
- Why was Pod Identity deprecated?
- Who manages the ServiceAccount? What about federation?
- How would an application consume & use the identity?

### Resources
- https://learn.microsoft.com/en-us/azure/aks/workload-identity-overview?tabs=dotnet
- https://learn.microsoft.com/en-us/azure/aks/learn/tutorial-kubernetes-workload-identity
- https://learn.microsoft.com/en-us/azure/aks/concepts-clusters-workloads
- https://learn.microsoft.com/en-us/azure/active-directory/workload-identities/workload-identities-overview
- https://learn.microsoft.com/en-us/azure/aks/workload-identity-deploy-cluster#optional---grant-permissions-to-access-azure-key-vault

## Step 5 - Deploying Container Insights
### Aim
Deploy and use container insights to investigate resource usage and performance of your AKS cluster. You should be able to visually show the resource usage of your cluster for a given application, as well as a full cluster view.

### Questions
- What is the difference between Container Insights, Prometheus, and Grafana?
- What filters are available in Container Insights?
- Could you build custom views from the data gathered by Container Insights? Why would you want to?
- What reports are available out of the box?

### Resources
- https://learn.microsoft.com/en-us/azure/azure-monitor/containers/kubernetes-monitoring-enable
- https://learn.microsoft.com/en-us/azure/azure-monitor/containers/container-insights-analyze

## Step 6 - Deploying Defender for Containers
### Aim
Deploy Azure Defender for Containers to your AKS cluster, and investigate the security recommendations it provides. You should be able to show the security recommendations for your cluster, and for a given application. Simulate a security event using `kubectl get pods --namespace=asc-alerttest-662jfi039n` 

### Questions
- What does Defender for Containers provide?
- Where is the Defender for Containers data logged?
- How can you ensure Defender for Containers is enabled on all clusters?
- Is there a difference in configuration for control plane vs data plane hardening?

### Resources
- https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-containers-introduction
- https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-containers-enable

## Step 7 - Configuring Backup
### Aim
Configure and run AKS backup using the Backup extension. You should be able to show the backup configuration, and details of a backed-up workload.

### Questions
- What does the AKS backup extension backup? What doesn't it backup?
- Can you restore cross-region?
- What granularity can you restore to?
- How do you monitor backups?

### Resources
- https://learn.microsoft.com/en-us/azure/backup/azure-kubernetes-service-backup-overview
- https://learn.microsoft.com/en-us/azure/backup/azure-kubernetes-service-cluster-backup

### Stretch
Configure the backup to only include a particular namespace, and restore a workload from a backup.

## Step 8 - Configure Automatic Upgrades
### Aim
Configure automatic upgrades for your AKS cluster to only be deployed on Tuesdays except for in the last week of December.

### Questions
- What components can you configure automatic upgrades for?
- When would you want to enable automatic upgrades? When wouldn't you?
- How is the upgrade process applied?
- What configuration is required on each pod or deployment to ensure that they remain available during an upgrade?
- How can we make upgrades safer to apply?

### Resources
- https://learn.microsoft.com/en-us/azure/aks/upgrade-cluster
- https://learn.microsoft.com/en-us/azure/aks/upgrade-aks-cluster?tabs=azure-cli
- https://learn.microsoft.com/en-us/azure/aks/node-image-upgrade
- https://learn.microsoft.com/en-us/azure/aks/node-updates-kured
- https://learn.microsoft.com/en-us/azure/aks/auto-upgrade-cluster
- https://learn.microsoft.com/en-us/azure/aks/stop-cluster-upgrade-api-breaking-changes

## Step 9 - Configure Planned Maintenance
### Aim
Disable any automatic upgrades for the cluster and node OS, but configure any planned maintenance to occur on a Saturday. You should be able to show the planned maintenance configuration for your cluster, and show that cluster and node OS automatic upgrades are disabled.

### Questions
- What schedules are available to configure?
- Can you set specific time of day for maintenance?
- How would you add a change freeze window?
- When would you want to disable automatic upgrades?

### Resources
- https://learn.microsoft.com/en-us/azure/aks/auto-upgrade-cluster
- https://learn.microsoft.com/en-us/azure/aks/auto-upgrade-node-os-image
- https://learn.microsoft.com/en-us/azure/aks/planned-maintenance

### Stretch
Add your planned maintenance configuration to your template from step 1 & 2.