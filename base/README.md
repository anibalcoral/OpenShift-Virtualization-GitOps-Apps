# Base

This directory contains the base Kubernetes/OpenShift resources and Kustomize configuration used by the OpenShift Virtualization GitOps workshop.

The base layer defines the canonical VirtualMachine and supporting resources without environment-specific overrides. Overlays (dev/hml/prd) apply patches to customize CPU, memory, disk sizes, naming prefixes and routes.

## Contents

- `kustomization.yaml` - Kustomize base configuration that references VM definitions and common resources.
- `ssh-secret.yaml` - Template for the SSH private key secret used by VMs (placeholder, not a real secret).
- `vm-web-01.yaml` - VirtualMachine definition for web VM 01 (common structure and cloud-init configuration).
- `vm-web-02.yaml` - VirtualMachine definition for web VM 02.
- `vm-web-service.yaml` - Service and Route (or Service/Ingress) configuration exposing the VM-based web service.

## Purpose

- Serve as the single source of truth for VM definitions used across environments.
- Keep environment-agnostic values in one place. Do not put environment-specific resource sizes, hostnames or routes here—use overlays.

## Usage

Build or apply the base resources directly (for development/testing only):

```bash
# Render manifests
kustomize build /path/to/OpenShift-Virtualization-GitOps-Apps/base

# Apply to the cluster (not recommended for multi-environment workflows)
oc apply -k /path/to/OpenShift-Virtualization-GitOps-Apps/base
```

In the GitOps workflow, ArgoCD points to environment overlays (not the base) so changes here are propagated through overlays via Kustomize.

## SSH Secret

The `ssh-secret.yaml` is a template. Create a repository/cluster secret containing your private key and reference it from overlays or the ArgoCD Application configuration. Example (create locally then push as needed):

```bash
oc create secret generic workshop-gitops-repo \
  --from-file=sshPrivateKey=$HOME/.ssh/id_rsa \
  --from-literal=url=git@github.com:your-org/your-repo.git \
  --from-literal=type=git -n openshift-gitops

oc label secret workshop-gitops-repo argocd.argoproj.io/secret-type=repository -n openshift-gitops
```

## Notes

- All VMs in base are built from Fedora templates and include cloud-init for SSH configuration and a basic Apache web server.
- Keep changes to the base minimal and generic. Environment-specific tuning belongs in overlays.

## See also

- Top-level Apps README: `../README.md`
