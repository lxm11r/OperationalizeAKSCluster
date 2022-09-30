# Operationalize your AKS Cluster

## Introduction and context

In this MicroHack we are going to cover some basic tasks you are facing when running an AKS cluster.

AKS - Azure Kubernetes Service (AKS) simplifies deploying a managed Kubernetes cluster in Azure by offloading the operational overhead to Azure.
As a hosted Kubernetes service, Azure handles critical tasks, like health monitoring and maintenance.
Since Kubernetes masters are managed by Azure, you only manage and maintain the agent nodes.
Thus, AKS is free; you only pay for the agent nodes within your clusters, not for the masters.

We will take some insights in fundamental operational tasks like scaling and learn how to observe our cluster with Azure Monitor.

## Learning Objectives

In this hack you will learn how to setup an AKS, how to handle daily business tasks, monitor operations and resources via Azure Monitor and access your deployments via an ingress controller.

## Content and Challenges

* Getting started
* Deploy the environment
* Deploy and configure your first pods
* Add Azure Monitor to your environment
* Scale up your services
* Access your services via an ingress controller
* Add service mesh to your environment

## Prerequisites

* VS Code
* Azure CLI
* kubectl
* terraform/bicep extension
* Azure Subscription

## Challenge 0: Getting started

### Introduction

Kubernetes is the de facto standard for big, scalable containerized platforms today and AKS is the solution on Azure. So in this challenge we will take a look into some basic concepts of AKS and K8s in general and what can happen during daily business operating an AKS. 

### Architecture

To have a relateable scenario, we will create a basic AKS cluster setup and use some common service. They will all be pulled directly from upstream sources.

### Components

* Azure resource groups are logical containers for Azure resources. You use a single resource group to structure everything related to this solution in the Azure portal.
* Azure Key Vault is a secret store used in Azure to securly manage your keys, certificates and secrets.
* Azure Kubernetes Service is the native approach from Azure for a kubernetes PaaS solution.
* Azure Container Insights is a feature from Azure monitor to simplify monitoring and logging of AKS resources via the Azure Portal.

### Learning resources

* AKS documentation: https://learn.microsoft.com/en-us/azure/aks/
* AKS best practices: https://learn.microsoft.com/en-us/azure/aks/best-practices

## Challenge 1: Set up the environment

### Introduction

In this challenge we will setup the basic Azure resources we need to fulfill our goal in handling and operating an AKS cluster in daily business. Use whatever you want to setup the resources

### Challenge

* West Europe Region
* Naming conventions for resources
* Key-Vault for storing secrets (optional)
* Go with one node in the beginning
* Default K8s roles 
* Kubenet as network provider
* Basic permission concept for accessing resources

### Success Criteria

* Naming convention defined
* AKS deployed
* Permission concept is created

### Learning resources

* AKS documentation: https://learn.microsoft.com/en-us/azure/aks/
* AKS best practices: https://learn.microsoft.com/en-us/azure/aks/best-practices

## Challenge 2: Deploy and configure your first pod

### Introduction

Now it is time to deploy some pods in our newly deployed cluster. Therefor you will need to apply some yaml-templates to the AKS, after you connected to it. We will use some upstream containers for that!

### Challenges

* Connect to your AKS cluster via Azure CLI
* Deploy a BusyBox Container from upstream sources by creating your own manifest
* Access your BusyBox Container via kubectl

### Success Criteria

* Config for AKS merged in local kubeconfig
* BusyBox container running and viewable via kubectl
* Container was accessed via kubectl

### Learning resources

* Connect to AKS via CLI: https://learn.microsoft.com/en-us/azure/aks/learn/quick-kubernetes-deploy-cli
* Kubernetes Cheatsheet: https://kubernetes.io/docs/reference/kubectl/cheatsheet/
* Busybox upstream source: https://hub.docker.com/_/busybox

## Challenge 3: Add Azure Monitor to your environment

### Introduction

To observe our cluster we now add Azure Monitor to our environment. This is the go-to solution for monitoring Azure services and can directly be connected to our cluster.

### Challenges

* Create Azure Monitor resource in same region and resource group like the other resources
* Configure Container Insights feature for the cluster

### Success Criteria

* Azure Monitor is added to the environment
* Container Insights feature is configured
* Metrics can be viewed in Azure Portal

### Learning Resources

* Azure Monitor Documentation: https://learn.microsoft.com/en-us/azure/azure-monitor/overview
* Container Insights Documentation: https://learn.microsoft.com/en-us/azure/azure-monitor/containers/container-insights-overview

## Challenge 4: Scale up your services

### Introduction

Since kubernetes is a hyperscaler, we need to use it as such and deploy more services. Therefor we will add some common containers from upstream to our cluster and see what happens.

### Challenges

* Connect to your cluster via CLI
* Create a manifest for a basic redis container
* Create a manifest for a basic wordpress container
* Apply your manifests to your cluster
* Scale your redis deployment up and down and try to scale up your whole cluster via your changes
* Check Azure Monitor for some insights on your infrastructure

### Success Criteria

* New containers are running smoothly in your cluster
* Note down your observations from Azure Monitor, what can you observe here and was it expected?

### Learning resources

* Azure Scalesets: https://learn.microsoft.com/en-us/azure/virtual-machine-scale-sets/overview
* Kubernetes Daemonsets: https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/
* Redis upstream source: https://hub.docker.com/_/redis
* Wordpress upstream source: https://hub.docker.com/_/wordpress

## Challenge 5: Access your services via an ingress controller

### Introduction

To now access our services from outside the cluster, we will deploy and configure an ingress controller. One of the core concepts for AKS workloads and use cases.

### Challenges

* Connect to your cluster via CLI
* Create a manifest for your ingress controller with image from NginX
* Apply your ingress manifest to your cluster
* Configure your ingress and wordpress container, so that you are able to access the intial config screen of wordpress. !Traffic must be routed throgh the ingress controller"
* Check Azure Monitor, if everthing works as expected

### Success Criteria

* Ingress Controller is deployed to cluster
* Wordpress can be configured via browser call
* All traffic between Internet and cluster is routed via the ingress controller

### Learning resources

* Ingress controller upstream source: https://docs.nginx.com/nginx-ingress-controller/installation/installation-with-manifests/
* Ingress controller documentation: https://docs.nginx.com/nginx-ingress-controller/
* Ingress in AKS documentation: https://learn.microsoft.com/en-us/azure/aks/ingress-basic?tabs=azure-cli

## Solution guide

### Challenge 1: Set up the environment

You can create your environment using any source you want. The following links describe solutions for using the Portal, the CLI or terraform:

* Portal: https://learn.microsoft.com/en-us/azure/aks/learn/quick-kubernetes-deploy-portal?tabs=azure-cli
* CLI: https://learn.microsoft.com/en-us/azure/aks/learn/quick-kubernetes-deploy-cli
* Terraform: https://learn.hashicorp.com/tutorials/terraform/aks

### Challenge 2: Deploy and configure your first pod

For deploying something on kubernetes, we can use tools like terraform or helm. If you want to use those, take a look here:

* terraform kubernetes provider: https://registry.terraform.io/providers/hashicorp/kubernetes/latest/docs
* helm: https://helm.sh/docs/

If not, you can just create a YAML-Manifest by your own, but be aware of the right intendation!

Here is a sample manifest for the BusyBox-Container: https://github.com/josedom24/kubernetes/blob/master/ejemplos/busybox/busybox.yaml

To apply your manifest, use the following command:

<code>
kubectl apply -f (manifest-filename)
</code>

To access your freshly deployed container, you can use the following command

<code>
kubectl exec -it (pod-name) /bin/bash
</code>

### Challenge 3: Add Azure Monitor to your environment

Here is a solution guide for creating Azure Monitor and configure Container Insights:

* Azure Monitor setup: https://learn.microsoft.com/en-us/windows-server/storage/storage-spaces/configure-azure-monitor
* Configure Container Insights: https://learn.microsoft.com/en-us/azure/azure-monitor/containers/container-insights-onboard

### Challenge 4: Scale up your services

Here you can find ready to go manifests for both instances:

* redis: https://kubernetes.io/docs/tutorials/configuration/configure-redis-using-configmap/
* wordpress: https://kubernetes.io/docs/tutorials/stateful-application/mysql-wordpress-persistent-volume/

Scaling your clusters can be achieved via the scale command of kubectl. Here is a sample:
<code>
kubectl scale [--resource-version=version] [--current-replicas=count] --replicas=COUNT (-f FILENAME | TYPE NAME)
</code>

### Challenge 5: Access your services via an ingress controller

NginX Ingress is one of the go-to resources for purposes like ours. An example manifest and all configuration information can be found here: https://kubernetes.github.io/ingress-nginx/deploy/
