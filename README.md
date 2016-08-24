# Status of Kubernetes on Azure

Contents:
 * [Stable: Kubernetes 1.3 Status](#stable-kubernetes-13-status)
 * [Alpha: Kubernetes 1.4 Status](#alpha-kubernetes-14-status)
 * [Future Work](#future-work)

The Kubernetes 1.4 approach is recommended, despite being pre-release.
It has more features and an easier deployment process.

## Stable: Kubernetes 1.3 Status

* Kubernetes 1.3 does not have native cloudprovider support for Azure.
* The bringup you choose needs to establish a pod network (often via Flannel)
* You will need to manually configure any desired load balancers.

### Recommended Cluster Deployment Strategy

#### [kube-up](http://kubernetes.io/docs/getting-started-guides/azure/)
  * Documentation
    * [Kubernetes on Azure (Flannel based)](https://kubernetes.io/docs/getting-started-guides/azure/)
    * [`azkube` Readme](https://github.com/colemickens/azkube/blob/master/README.md)
  * Features
    * Uses a custom tool to automate everything, [`azkube`](https://github.com/colemickens/azkube)
    * Azure VM Scale Sets + Azure ARM Template
    * CoreOS + cloud-init + Flannel
    * Uses the modern Azure Resource Manager stack.
    * Outputs a deployment directory containing utilities for connecting to the cluster, as well as tools for SSHing to nodes in the cluster.
    * Outputs an ARM template that could be modified and/or re-used.

## Alpha: Kubernetes 1.4 Status

* Kubernetes 1.4 is expected to release as stable in September 2016.
* Kubernetes 1.4 will include native cloudprovider support for Azure.
* Kubernetes 1.4 will include AzureDisk as a storage volume type. This leverages data disks in Azure, similar to how GCEPersistentDisk works.

### Recommended Cluster Deployment Strategy

#### [kubernetes-anywhere](https://github.com/kubernetes/kubernetes-anywhere/tree/master/phase1/azure/README.md)
  * Documentation
    * [General Documentation](https://github.com/kubernetes/kubernetes-anywhere)
    * [Azure Getting Started Guide](https://github.com/kubernetes/kubernetes-anywhere/blob/master/phase1/azure/README.md)
  * Features
    * [Azure Cloudprovider Support](https://github.com/kubernetes/kubernetes/pull/28821)
    * [Azure Peristent Volume Support](https://github.com/kubernetes/kubernetes/pull/29836) (thanks [@rootfs](https://github.com/rootfs)!)
    * [Azure Dynamic Disk Provisioning](https://github.com/kubernetes/kubernetes/pull/30091) (thanks [@rootfs](https://github.com/rootfs)!)
    * Configures and activates the cloudprovider support automatically
    * Automatic Pod network configuration without the overhead of Flannel
    * Automatic L4 LoadBalancers for Services (Type=LoadBalancer)
    * Leverages Terraform to enable manual scaling up/down of the cluster
    * Leverages Ignition for fast, idempotent node bootstrapping
  * Caveats
    * Does NOT use Azure VM Scale Sets (see Notes below)

## Future Work

These are potential projects that anyone could pick up to help Kubernetes on Azure scenarios.

1. **Azure Ingress Controller**
  * **Status**: Not started
  * **Description**:
    * Build an `azure-ingress-controller` using [Azure's ApplicationGateways](https://azure.microsoft.com/en-us/services/application-gateway/)
    * Similar to the [Google Cloud Ingress Controller](https://github.com/kubernetes/contrib/tree/master/ingress/controllers/gce)
2. **Azure Cluster-Autoscale Backend**
  * **Status**: Not started
  * **Description**:
    * Build an Azure backend for [`cluster-autoscale`](https://github.com/kubernetes/contrib/tree/master/cluster-autoscaler).
3. **Azure Federation Support**
  * **Status**: Not started
  * **Description**:
    * Implementation of [dnsprovider](https://github.com/kubernetes/kubernetes/tree/master/federation/pkg/dnsprovider).
    * Likely additional work...

## Notes

* Kubernetes-Anywhere does not use VMSS due to limitations around VMSS and Disk Attachment and the fact that the Azure CloudProvider does not work with VMSS. There is working on-going to remove these restrictions.
