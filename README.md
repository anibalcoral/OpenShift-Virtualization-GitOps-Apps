# OpenShift Virtualization GitOps Applications

This repository contains the Virtual Machine definitions and Kustomize configurations for the OpenShift GitOps with OpenShift Virtualization workshop.

## Repository Structure

```
├── base/                         # Base VM templates and resources
│   ├── kustomization.yaml        # Base Kustomize configuration
│   ├── vm-web-01.yaml            # Web server VM 01 definition
│   ├── vm-web-02.yaml            # Web server VM 02 definition
│   └── vm-web-service.yaml       # Service for VM access
└── overlays/                     # Environment-specific customizations
    ├── dev/                      # Development environment
    │   └── kustomization.yaml    # Dev-specific patches
    ├── hml/                      # Homologation environment
    │   └── kustomization.yaml    # HML-specific patches
    └── prd/                      # Production environment
        └── kustomization.yaml    # PRD-specific patches
```

## Branch Strategy

The branches are created during the lab provisioning.

- **vms-dev-{guid}**: Development virtual machines
- **vms-hml-{guid}**: Homologation/Staging virtual machines  
- **vms-prd-{guid}**: Production virtual machines

## Virtual Machine Templates

All VMs are based on Fedora templates and include:
- Cloud-init configuration for SSH access
- Web server setup with Apache HTTP Server
- Firewall configuration for HTTP access
- Environment-specific customizations via Kustomize

## Usage

This repository is used by ArgoCD Applications configured in the main OpenShift-Virtualization-GitOps repository. Each environment points to a different branch:

- Development → vms-dev-{guid} branch
- Homologation → vms-hml-{guid} branch  
- Production → vms-prd-{guid} branch

Changes should be made in the vms-dev-{guid} branch and promoted through merge requests to higher environments.
