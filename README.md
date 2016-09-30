# Status of Kubernetes on Azure

Contents:
 * [Stable: Kubernetes 1.4 Status](#stable-kubernetes-14-status)
 * [Future Work](#future-work)

## Stable: Kubernetes 1.4 Status

* Kubernetes 1.4 is available as of September 26, 2016
* It includes native cloudprovider suport for Azure, including:
    * native pod networking enabled without running an overlay/SDN.
    * automatic Azure L4 load balancers provisioned for Services (with `Type=LoadBalancer`)
* It also includes AzureDisk as a storage volume type. This leverages data disks in Azure, similar to how GCEPersistentDisk works.

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
  * **Status**: Work In Progress (**[@JargoonPard](https://github.com/JargoonPard)**)
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
4. **Azure Disk Performance Improvements**
  * **Status**: Not being actively looked at
  * **Description**:
    * See [this issue (and referenced issues)](https://github.com/colemickens/azure-kubernetes-demo/issues/14#issuecomment-250564890) for some background.
    * Some investigation is needed to identify and drive improvements on things that are causing AzureDisk attach/detach performance to be lower than expected.

## Notes

* Kubernetes-Anywhere does not use VMSS due to limitations around VMSS and Disk Attachment and the fact that the Azure CloudProvider does not work with VMSS. There is working on-going to remove these restrictions.
