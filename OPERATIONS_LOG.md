## Cluster Creation
**Summary:** Create VMs via Parallels Desktop, create three node cluster via kubeadm, join worker nodes to controlplane node, install Helm

- 2026-01-10 Install Ubuntu Server on all three VMs - configure hostname and IP, validate connectivity to other VMs.
  ![cl1-1](../Cluster/cl1-1.jpg)
- 2026-01-10 Install containerd on node01 and validate running.
  ![cl1-2](../Cluster/cl1-2.jpg)
- 2026-01-10 Install containerd on node02 and validate running.
  ![cl1-3](../Cluster/cl1-3.jpg)
- 2026-01-10 Install containerd on controlplane and validate running.
  ![cl1-4](../Cluster/cl1-4.jpg)
- 2026-01-10 Validate ssh functional from MacBook to ControlPlane node.
  ![cl1-5](../Cluster/cl1-5.jpg)
- 2026-01-10 Validate ssh functional from MacBook to worker node node-01.
  ![cl1-6](../Cluster/cl1-6.jpg)
- 2026-01-10 Validate ssh functional from MacBook to worker node node-02.
  ![cl1-7](../Cluster/cl1-7.jpg)
- 2026-01-10 Successfully initialize the control-plane node.
  ![cl1-8](../Cluster/cl1-8.jpg)
- 2026-01-10 TROUBLESHOOTING: Current containerd version installed is not compatible with control plane - update to 2.2.1.
  ![cl1-9](../Cluster/cl1-9.jpg)
  ![cl1-10](../Cluster/cl1-10.jpg)
- 2026-01-10 Joined both worker nodes to the control-plane node and validate.
  ![cl1-11](../Cluster/cl1-11.jpg)
- 2026-01-10 Install Helm and validate installed version.
  ![cl1-12](../Cluster/cl1-12.jpg)

## Cilium
**Summary:** Installation and validation of Cilium on controlplane node. Installation and configuration of Hubble. Create and apply CNP. Enforce and validate policies.
- 2026-01-11 Remove kubeproxy, install Cilium validate install health, validate all nodes are ready and no kubeproxy pods present in kube-system namespace.
  ![cil1-1](../Cilium/cl1-1.jpg)
- 2026-01-11 TROUBLESHOOTING: Cilium test completed - broken originally - had to force tunneling by pinning to the interface in use by the cluster.
  ![cil1-2](../Cilium/cl1-2.jpg)
- 2026-01-11 Validate Hubble installed - pods ready and up - architecture is correct for ARM silicon then validate installed version.
  ![cil1-3](../Cilium/cl1-3.jpg)
- 2026-01-11 Validate can filter for DROPPED traffic via hubble.
  ![cil1-4](../Cilium/cl1-4.jpg)
- 2026-01-11 Invoke Cilium dbg utility via one of the Cilium pods and inspect endpoint ingress/egress policy enforcement.
  ![cil1-5](../Cilium/cl1-5.jpg)
- 2026-01-11 Edge namespace hubble UX observability validated.
  ![cil1-6](../Cilium/cl1-6.jpg)
- 2026-01-11 Client namespace hubble UX observability validated.
  ![cil1-7](../Cilium/cl1-7.jpg)
- 2026-01-11 Backend namespace hubble UX observability validated.
  ![cil1-8](../Cilium/cl1-8.jpg)
- 2026-01-11 Validate hubble observe functionality and flows are being captured.
  ![cil1-9](../Cilium/cl1-9.jpg)
- 2026-01-11 Validate hubble observe is properly logging traffic.
  ![cil1-10](../Cilium/cl1-10.jpg)
- 2026-01-11 Validate all Cilium Network Policies are active in cluster and properly namespaced-scoped, including zero trust.
  ![cil1-11](../Cilium/cl1-11.jpg)
- 2026-01-11 Validate Cilium Network Policies are being enforced after validating namespace scoping.
  ![cil1-12](../Cilium/cl1-12.jpg)

## Key Lessons Learned
- Cilium CNP enforcement is namespace-scoped and silently fails if misapplied
- Hubble CLI requires relay port-forward even when UI is accessible
- Native vs tunneling routing must match underlying interface reality
- EndpointSelector precision directly impacts enforcement visibility
