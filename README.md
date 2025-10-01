# OpenShift Virtualization GitOps Applications

This repository contains the Virtual Machine definitions and Kustomize configurations for the OpenShift GitOps with OpenShift Virtualization workshop.

## Repository Structure

```
├── base/                           # Base VM templates and resources
│   ├── kustomization.yaml         # Base Kustomize configuration
│   ├── ssh-secret.yaml           # SSH secret template
│   ├── vm-web-01.yaml            # Web server VM 01 definition
│   ├── vm-web-02.yaml            # Web server VM 02 definition
│   └── vm-web-service.yaml       # Service for VM access
└── overlays/                     # Environment-specific customizations
    ├── dev/                      # Development environment
    │   └── kustomization.yaml   # Dev-specific patches
    ├── hml/                      # Homologation environment
    │   └── kustomization.yaml   # HML-specific patches
    └── prd/                      # Production environment
        └── kustomization.yaml   # PRD-specific patches
```

## Branch Strategy

- **vms-dev**: Development virtual machines
- **vms-hml**: Homologation/Staging virtual machines  
- **main**: Production virtual machines

## Virtual Machine Templates

All VMs are based on Fedora templates and include:
- Cloud-init configuration for SSH access
- Web server setup with Apache HTTP Server
- Firewall configuration for HTTP access
- Environment-specific customizations via Kustomize

## Usage

This repository is used by ArgoCD Applications configured in the main OpenShift-Virtualization-GitOps repository. Each environment points to a different branch:

- Development → vms-dev branch
- Homologation → vms-hml branch  
- Production → main branch

Changes should be made in the vms-dev branch and promoted through merge requests to higher environments.
