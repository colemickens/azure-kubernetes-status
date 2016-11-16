# Status of Kubernetes on Azure

Contents:
 * [Features Status](#features-status)
 * [Recommended Cluster Deployment Mechanism](#recommended-cluster-deployment-mechanism)
 * [Future Work](#future-work)

## Features Status

| Feature | K8s 1.3.x | K8s 1.4.6 | K8s 1.5.x | PRs |
| ------- | --------- | --------- | --------- | --- |
| Native Pod Networking                        |   | ✓ | ✓ | [#28821](https://github.com/kubernetes/kubernetes/pull/28821) |
| Automatic L4 Load Balancers                  |   | ✓ | ✓ | [#28821](https://github.com/kubernetes/kubernetes/pull/28821), [#36256](https://github.com/kubernetes/kubernetes/pull/36256) |
| Source IP White-listing w/ L4 Load Balancers |   |   | ✓ | [#36696](https://github.com/kubernetes/kubernetes/pull/36696) |
| Source IP Preservation w/ L4 Load Balancers  |   |   | ✓ | [#36557](https://github.com/kubernetes/kubernetes/pull/36557) |
| Persistent VHD Disks (AzureDisk)             |   | ✓ | ✓ | [#29836](https://github.com/kubernetes/kubernetes/pull/29836) |
| Dynamically Provisioned Persistent Disks     |   |   | ✓ | [#30091](https://github.com/kubernetes/kubernetes/pull/30091) |

 * Kubernetes 1.4 was released on September 26, 2016.
 * Kubernetes 1.5 [is expected to be released around December 8, 2016](https://github.com/kubernetes/features/blob/master/release-1.5/release-1.5.md).

## Recommended Cluster Deployment Strategy

### [Azure Container Service](https://azure.microsoft.com/en-us/services/container-service/)
  * Azure's first-party Container Service
  * The Azure Container Service supports DC/OS, Swarm and Kubernetes.
  * Documentation: [Azure.com: Azure Container Service Documentation](https://azure.microsoft.com/en-us/documentation/services/container-service/)
  * Walkthrough: `[coming soon]`
  * More Information: [Kubernetes Blog: Bringing Kubernetes Support to Azure](http://blog.kubernetes.io/2016/11/bringing-kubernetes-support-to-azure.html).

### [ACS-Engine](https://github.com/Azure/ACS-Engine)
  * The open-source core of the Azure Container Service
  * Enables easy, up-front modifications of Kubernetes clusters deployed with Azure Resource Manager (ARM) Templates

## Future Work

These are potential projects that anyone could pick up to help Kubernetes on Azure scenarios.

1. **Azure Ingress Controller**
  * **Status**: [Work In Progress](https://github.com/JargoonPard/contrib/commits/master)
  * **Description**:
    * Build an `azure-ingress-controller` using [Azure's ApplicationGateways](https://azure.microsoft.com/en-us/services/application-gateway/)
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
  * **Status**: Fixes coming in 1.5, being looked at internally as well
  * **Description**:
    * See [this issue (and referenced issues)](https://github.com/colemickens/azure-kubernetes-demo/issues/14#issuecomment-250564890) for some background.
