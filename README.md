# OKE_GPUS
# Nvidia Device Plugin with OCI Container Engine for Kubernetes (OKE)


## Introduction

This tutorial will step through the process of how to use GPUs on OKE using the Nvidia Device Plugin for Kubernetes.

### Available OKE GPU shapes.

Virtual Machine (VM) shapes
VM.GPU2.1
VM.GPU3.1
VM.GPU3.2
VM.GPU3.4
VM.GPU3.A10.1
VM.GPU3.A10.2

Baremetal (BM) shapes
BM.GPU2.2
BM.GPU3.8
VM.GPU4.8
VM.GPU3.A100
VM.GPU3.A10.1
VM.GPU3.A10.2

### Nvidia Device Plugin
The NVIDIA Device Plugin is a tool that helps manage GPU resources in a Kubernetes cluster. It automatically reveals the number of GPUs in each node and makes them allocatable by the Kubernetes scheduler. With the NVIDIA Device Plugin, you don't have to manually set up GPU resources on each node. It also monitors GPU health, which helps Kubernetes maintain reliability and stability for tasks that need GPUs. Moreover, the Plugin allows multiple pods to share GPUs, which is vital when GPUs are scarce and expensive.

### Pre-requisites for 

Ensure that your GPU nodes have the necessary NVIDIA drivers (version ~= 384.81) installed.
 optional Install the nvidia-container-toolkit (version >= 1.7.0) on each GPU node.
Configure the nvidia-container-runtime as the default runtime for Docker or containerd.
Kubernetes version >= 1.10
If using OKE the driver should be installed handled for you by default when using GPU nodes

    nvidia-smi (installed)
    modinfo nvidia | grep ^version
    sudo dmesg | grep 'loading NVIDIA'





## Pre-requisites

- [OCI CLI installed with the required credentials to deploy OKE in your tenancy](https://docs.oracle.com/en-us/iaas/Content/API/SDKDocs/cliinstall.htm)
- [kubectl installed](https://kubernetes.io/docs/tasks/tools/)
- [Terraform Installed](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)


Click [![Deploy to Oracle Cloud](https://oci-resourcemanager-plugin.plugins.oci.oraclecloud.com/latest/deploy-to-oracle-cloud.svg)](https://cloud.oracle.com/resourcemanager/stacks/create?region=home&zipUrl=https://github.com/oracle-devrel/terraform-oci-arch-oke-virtual-node/archive/refs/tags/v1.zip)

## Installation of Terraform stack

**1. Clone or download the contents of this repo** 
     
     git clone https://github.com/oracle-devrel/terraform-oci-arch-oke-virtual-node.git

**2. Change to the directory that holds the Terraform stack** 

      cd ./terraform-oci-arch-oke-virtual-node.git

**3. Populate the varaibles.tf file**


**4. Install the Terraform stack**

     terraform init
     terraform plan
     teraffrom apply
  

**5. Add Kubeconfig of Virtual Node cluster**

     
 run oci command in output of terraform apply

###  Sample Output 
![title](kubeconfig1.png)


**7. To remove Terraform stack**

     terraform destroy
     
 
##  variables.tf specification


| Variables                          | Description                                                         | Type   | Mandatory |
| ---------------------------------- | ------------------------------------------------------------------- | ------ | --------- |
| `compartment_id` | Compartment to deploy OKE Virtual Nodes cluster | string | yes  |
| `tenancy_ocid` | Customer's tenancy ocid| string | yes  |
| `region` | region to deploy the OKE Virtual Nodes Cluster  | string | yes     |
| `pod_shape` | The shape of Virtual Nodes | string | yes       |
| `virtual_node_count` | The number of Virtual Nodes in the node pool  | number | yes       |
| `create_IAM_policy` | To create the policy for for Virtual Node operations set to "true". The IAM policy is created in the root compartment in customer's home region. Customer must have access to create a policy in this compartment.| bool | yes       |
| `deploy_metrics_server` | install metrics server. Set to "true" to create the policy | bool | yes  |
| `deploy_kubernetes_dashboard` | install Kubernetes dashboard. Set to "true" to create the policy | bool | yes  |
| `deploy_ingress_controller` | install ingnx ingress controller. Set to "true" to create the policy | bool | yes  |


## Useful commands 


**1. Check Virtual Nodes status**
     
     kubectl get nodes -o wide

**1. Get IP address of Nginx ingress controller**

     kubectl -n default get svc ingress-nginx

## Additional Resources

- [OKE Virtual Nodes deliver a serverless Kubernetes experience](https://blogs.oracle.com/cloud-infrastructure/post/oke-virtual-nodes-deliver-serverless-experience)
- [Oracle Container Engine for Kubernetes(OKE)](https://www.oracle.com/cloud/cloud-native/container-engine-kubernetes/#:~:text=Oracle%20Cloud%20Infrastructure%20Container%20Engine,complexities%20of%20the%20Kubernetes%20infrastructure.)