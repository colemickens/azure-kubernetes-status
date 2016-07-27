# Status of Kubernetes on Azure

Contents:
 * Current State of Kubernetes 1.3
 * Planned State for Kubernetes 1.4
 * Future Work

If you'd like to see a complete end-to-end demo of Kubernetes 1.4-**alpha** on 
Azure, checkout [the azure-kubernetes-demo repo](https://github.com/colemickens/azure-kubernetes-demo).

## Stable: Kubernetes 1.3 Status

There is [basic bring-up support (kube-up) for Kubernetes on Azure](http://kubernetes.io/docs/getting-started-guides/azure/)
but there is no CloudProvider implemented. This means that users must run a software-defined
network in order to meet the requirements for Kubernetes. Additionally, users
must do manual work to expose Services out to the Internet, rather than being
able to rely on automatic L4 LoadBalancer provisioning provided by a
CloudProvider implementation.

### Bring-up Options

1. **Bright Pixel**: [Custom Ansible + Terraform (ARM)](https://github.com/edevil/kubernetes-deployment)
  * Ubuntu + Terraform + Ansible
  * Uses the modern Azure Resource Manager stack.

2. **Microsoft**: [kube-up: with custom `azkube` tool (ARM)](http://colemickens.github.io/docs/getting-started-guides/azure/)
  * Documented [here](http://colemickens.github.io/docs/getting-started-guides/azure/), 
    pending [merge into the Kubernetes docs repo](https://github.com/kubernetes/kubernetes.github.io/pull/149)
  * CoreOS + cloud-config + ARM Template + Flannel
  * Uses a custom tool, [`azkube`](https://github.com/colemickens/azkube)
  * The tool automates creation of an ARM Template that uses set of cloud-config
    files to configure a `hyperkube`-powered Kubernetes cluster.
  * The generated template could be re-used later, **without** needing to use
    `azkube` again).
  * Uses the modern Azure Resource Manager stack.

3. **Weaveworks**: [Custom Deployment using Weave (ASM)](http://kubernetes.io/docs/getting-started-guides/coreos/azure/)
  * Contributed to `kubernetes` repo by [Weaveworks](https://www.weave.works/).
  * Ubuntu + azure node api + Weave
  * A `node.js` driven deployment. 
  * Uses the *legacy* Azure Service Management stack


4. **AppFormix**: kube-up: clasic Salt (ASM)
  * Contributed to `kubernetes` repo by [AppFormix](http://www.appformix.com/)
  * Uses [`azure-xplat-cli`](https://github.com/Azure/azure-xplat-cli) with Salt.
  * The [pull request (#21207)](https://github.com/kubernetes/kubernetes/pull/21207) 
    for this is currently open and pending merge.
  * Uses the *legacy* Azure Service Management stack (though there are plans to 
    quickly move it to the modern ARM stack).

## Alpha: Kubernetes 1.4 Status

Kubernetes 1.4 is expected to release as stable in September 2016.

1. **Azure CloudProvider**
    * **Status**: Complete!
    * **Availability**: Available in the `master` branch of `kubernetes`. It should be present in the next alpha release, `v1.4.0-alpha.2`.
    * **Description**:
       * This will enable automatic L4 LoadBalancer provisioning and native Pod 
         Networking in Azure.

2. **Azure Next-Gen Bring-up (`kubernetes-anywhere`)**
    * **Status**: Initial Version Complete
	* **Availability**: Available in the `master` branch of [`kubernetes-anywhere`](https://github.com/kubernetes/kubernetes-anywhere).
    * **Description**: `kube-up` is planned to be deprecated eventually. [`kubernetes-anywhere`](https://github.com/kubernetes/kubernetes-anywhere) will take it's place as the starting point for deploying a basic cluster into various clouds.


## Future Work

These are potential projects that anyone could pick up to help Kubernetes on Azure scenarios.

0. **Azure PersistentVolume Storage Plugin**
    * **Status**: In Progess
    * **Description**:
      * [Huamin Chen (Redhat)](https://github.com/rootfs) is working on a Persistent Volume plugin for Azure VHDs
      * [Work-in-progress pull request](https://github.com/kubernetes/kubernetes/pull/25195)

1. **Additional improvements to `kubernetes-anywhere`**
    * **Status**: Planned, but not started
    * **Description**:
      * allow for multiple side-by-side deployments ([WIP Pull Request](https://github.com/kubernetes/kubernetes-anywhere/pull/157))
      * automate creation of the Azure Active Directory ServicePrincipal ([Issue](https://github.com/kubernetes/kubernetes-anywhere/issues/184))

2. **Continuous Integration Testing for `kubernetes-anywhere`**
    * **Status**: Planned, but not started
    * **Description**:
      * Create Jenkins jobs running deployments of `master` kubernetes and `master` kubernetes-anywhere.
      * Upstream Issue: https://github.com/kubernetes/kubernetes-anywhere/issues/171

3. **Azure Ingress Controller**
    * **Status**: Not started
    * **Description**:
      * Build an `azure-ingress-controller` in the same vein as the [Google Cloud Ingress Controller](https://github.com/kubernetes/contrib/tree/master/ingress/controllers/gce.), but using [Azure's ApplicationGateways](https://azure.microsoft.com/en-us/services/application-gateway/).

4. **Azure Cluster-Autoscale Backend**
    * **Status**: Not started
    * **Description**:
      * Build an Azure backend for [`cluster-autoscale`](https://github.com/kubernetes/contrib/tree/master/cluster-autoscaler).

5. **Azure Federation Support**
    * **Status**: Not started
    * **Description**:
      * Implementation of [dnsprovider](https://github.com/kubernetes/kubernetes/tree/master/federation/pkg/dnsprovider).
      * Likely additional work...
