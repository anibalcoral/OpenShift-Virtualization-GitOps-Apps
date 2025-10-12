# Overlays

This directory contains environment-specific Kustomize overlays that patch the base VM definitions for development, homologation (hml) and production environments.

Each overlay applies JSON patches or strategic-merge patches to adjust resource requests/limits, CPU cores, disk sizes, hostnames and any environment-specific routing or label changes.

## Layout

```
overlays/
├── dev/    # Development overlay (smaller resource footprints, shorter names)
│   └── kustomization.yaml
├── hml/    # Homologation / Staging overlay (medium resources)
│   └── kustomization.yaml
└── prd/    # Production overlay (larger resources, stable hostnames)
    └── kustomization.yaml
```

## Usage

In a GitOps deployment, ArgoCD Applications point to one of these overlays to deploy the appropriate environment. Example ArgoCD application source paths:

- `OpenShift-Virtualization-GitOps-Apps/overlays/dev` (vms-dev-GUID branch)
- `OpenShift-Virtualization-GitOps-Apps/overlays/hml` (vms-hml-GUID branch)
- `OpenShift-Virtualization-GitOps-Apps/overlays/prd` (vms-prd-GUID branch)

Apply an overlay locally for testing:

```bash
# Render the dev overlay
kustomize build /path/to/OpenShift-Virtualization-GitOps-Apps/overlays/dev

# Apply to cluster (for testing)
oc apply -k /path/to/OpenShift-Virtualization-GitOps-Apps/overlays/dev
```

## Best practices

- Keep the base minimal and put only environment-specific changes in overlays.
- Use small, reversible patches (JSON patch) for resource tuning.
- Test changes in `dev` first, promote via pull requests to `hml` and then to `main` for `prd`.

## Troubleshooting

- If an overlay does not produce the expected manifest, run `kustomize build` to inspect the merged output.
- Check ArgoCD application sync status and application logs in the `openshift-gitops` namespace if resources fail to apply.

## See also

- Apps base: `../base/README.md`
- Top-level Apps README: `../README.md`
