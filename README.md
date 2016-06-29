# Status of Kubernetes on Azure

This repo will hold a `README.md` which will contain information about the state
 of the world in regards to Kubernetes on Azure.

This consists of two primary sections:
 * Current State of Kubernetes 1.2 and 1.3
 * Planned State for Kubernetes 1.4

If you'd like to see a complete end-to-end demo of Kubernetes 1.4-**alpha** on 
Azure, checkout [the azure-kubernetes-demo repo](https://github.com/colemickens/azure-kubernetes-demo).

## Current State of Kubernetes 1.2 and 1.3

There is basic bring-up support for Kubernetes on Azure but there is no
CloudProvider implemented. This means that users must run a software-defined 
network in order to meet the requirements for Kubernetes. Additionally, users 
must do manual work to expose Services out to the Internet, rather than being 
able to rely on automatic L4 LoadBalancer provisioning provided by a 
CloudProvider implementation.

### Bring-up Options

1. **Microsoft**: [Custom Ansible + Terraform (ARM)](https://github.com/edevil/kubernetes-deployment)
  * Created by Microsoft employee
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

## Planned State for Kubernetes 1.4

Kubernetes 1.4 is expected to release as stable in September 2016.

There are 4 efforts which will bring Azure to feature-parity with GCE/AWS. It 
will also utilize the same deployment mechanism for basic cluster deployments as GCE/AWS.

1. **Azure CloudProvider**

  This will enable Azure LoadBalancers (L4 TCP LoadBalancers) and native Pod 
  Networking in Azure. This enables the easy, automatic exposure of Kubernetes 
  Services outside of your cluster.

  ~~**[This is the pull request for `kubernetes`.(file:///dev/null).**~~
  (Expected by end of June.)

2. **Azure Next-Gen Bring-up (`kubernetes-anywhere`)**

  `kube-up` is planned to be deprecate eventually. [`kubernetes-anywhere`](https://github.com/kubernetes/kubernetes-anywhere) 
  will take it's place as the starting point for deploying a basic cluster into 
  various clouds.

  [The Azure implementation of `kubernetes-anywhere` has been merged](https://github.com/kubernetes/kubernetes-anywhere).

  Next steps are to add Azure CI jobs after Google Cloud CI has been added.

3. **Azure PersistentVolume Storage Plugin**

  [Huamin Chen (Redhat)](https://github.com/rootfs) has created an
  Azure PersistentVolume Strorage Plugin for Azure. This will enable users to 
  use VHDs as persistent storage inside the Kubernetes cluster.

  There is **[an open pull request based on @rootfs's `azure` branch](https://github.com/kubernetes/kubernetes/pull/25195)**,
  and [@rootfs's `azure-dev` branch](https://github.com/rootfs/kubernetes/tree/azure-vhd).

4. **Azure Ingress Controller**

  [Larry Guger (Microsoft)](https://github.com/JargoonPard) is soon to begin 
  working on an Azure Ingress Controller implementation that will be enable 
  users to provision [Azure ApplicationGateways](https://azure.microsoft.com/en-us/services/application-gateway/) 
  (L7 HTTP LoadBalancers) for Ingress resources in Kubernetes.

