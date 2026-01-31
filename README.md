# Kubernetes Cilium Zero-Trust Architecture

This repository documents the design, implementation, and validation of a Kubernetes cluster built from scratch using **kubeadm**, with **Cilium** providing CNI, network policy enforcement, and observability via **Hubble**.

The goal of this project was to design and enforce **zero-trust, namespace-scoped networking** and to validate that enforcement at the dataplane level rather than assuming correctness.

This is not a tutorial or a reference architecture copied from documentation. It is an operational record of a real cluster built, debugged, and validated end-to-end.

---

## Repository Structure

```
kubernetes-cilium-zerotrust-architecture/
├── README.md
├── LISCENCE.md
├── architecture/
│   └── kubernetes-cilium-zerotrust-architecture.png
├── manifests/
│   ├── client/
│   │   └── greenDeployment.yaml
│   ├── edge/
│   │   ├── pinkDeployment.yaml
│   │   └── traefikService.yaml
│   ├── backend/
│   │   ├── scarletDeployment.yaml
│   │   ├── yellowDeployment.yaml
│   │   ├── echoService.yaml
│   │   └── nginxService.yaml
│   └── cilium/
│       ├── busyboxCnp.yaml
│       ├── ztpNginxCnp.yaml
│       ├── ztpEchoCnp.yaml
│       └── ztpTraefikCnp.yaml
├── Cluster/
│   └── *.jpg
└── Cilium/
    └── *.jpg
```

## High-Level Overview

Three-node Kubernetes cluster built via kubeadm
Container runtime: containerd
CNI: Cilium (kube-proxy replacement enabled)
Observability: Hubble UI + Hubble CLI
Workload namespaces:
client (busybox)
edge (Traefik)
backend (nginx, http-echo)
Security model:
Identity-based, namespace-scoped Cilium Network Policies
Explicit ingress and egress rules
DNS access explicitly allowed
All other traffic denied by default
The cluster was built on ARM-based virtual machines using Parallels Desktop on macOS

## Architecture

The architecture diagram illustrates namespace boundaries, workload placement, and allowed traffic flows. All application traffic originates from the client workload, and all ingress and egress paths are explicitly controlled via Cilium Network Policies.
Key concepts illustrated:
- Separation between infrastructure (control plane, Cilium, Hubble) and workloads
- Namespace-scoped enforcement boundaries
- Directional traffic flow (ingress vs egress)
- Explicit DNS access
- Policy enforcement validated via Hubble and cilium-dbg

## Zero-Trust Model

This cluster follows a strict zero-trust approach:
- No workload accepts traffic unless explicitly allowed
- No workload can egress traffic unless explicitly allowed
- Policies are applied per-namespace and per-workload identity
- DNS is explicitly permitted to avoid implicit dependencies
- Enforcement is validated through observed drops, not assumptions
Cilium Network Policies were intentionally designed to be minimal, explicit, and readable, favoring correctness and observability over convenience

## Observability and Validation

Observability was treated as a first-class requirement
Validation steps included:
- Verifying policy attachment and enforcement via cilium-dbg
- Confirming allowed and denied flows using hubble observe
- Validating namespace-specific visibility in the Hubble UI
- Confirming enforcement after correcting namespace scoping issues
- Verifying dropped traffic is observable at the dataplane
This ensured that policies were not only syntactically correct, but actually enforced

## Operational Log

A detailed, chronological operational log with screenshots is maintained separately and documents:
- Cluster bootstrap and node joins
- Container runtime and kubeadm issues
- Cilium installation and routing mode troubleshooting
- Hubble setup and validation
- Network policy enforcement verification

## Key Lessons Learned

- Cilium Network Policy enforcement is namespace-scoped and will silently fail if misapplied.
- Hubble CLI access requires an active relay port-forward even when the Hubble UI is accessible.
- Native routing vs tunneling must align with the underlying network interface reality.
- EndpointSelector precision directly determines whether policies are enforced or ignored.
- Observability is required to validate negative cases, not just successful traffic flows.
