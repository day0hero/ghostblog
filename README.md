# ghostblog

This is a repo to quickly build out a ghostblog CMS in kubernetes using
an ingress controller and SSL via cert-manager/lets-encrypt. The /config
directory houses manifests that can be run individually to achieve the goal of installing the ghost CMS.


Assumptions (stuff i had in place prior to build):
- Install an ingress controller:
`kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v0.35.0/deploy/static/provider/cloud/deploy.yaml`

- Install cert-manager:
`kubectl apply --validate=false -f https://github.com/jetstack/cert-manager/releases/download/v1.0.1/cert-manager.yaml`

```
application
├── base
│   ├── clusterIssuer_prod.yaml
│   ├── clusterIssuer_staging.yaml
│   ├── deployment.yaml
│   ├── ingress.yaml
│   ├── kustomization.yaml
│   ├── namespace.yaml
│   ├── pvc.yaml
│   └── service.yaml
└── overlays
    ├── aws_opentlc
    │   ├── deployment.yaml
    │   ├── ingress.yaml
    │   ├── kustomization.yaml
    │   └── pvc.yaml
    ├── digitalocean
    │   └── kustomization.yaml
    └── gke
        ├── clusterIssuer_prod.yaml
        ├── deployment.yaml
        ├── ingress.yaml
        ├── kustomization.yaml
        └── pvc.yaml
```

The overlays are the different cloud providers - which use different default storage-classes. To install 
this in your cluster you will need to follow these procedures:

1. Modify the URL in the deployment.yaml
2. Modify the hosts entry in the ingress.yaml (there will be 3) 
3. Modify the email address in clusterIssuer_prod.yaml. This is required for letsencrypt verification.
4. Change the pvc storageclass type (if you're using something different).

To apply (example: GKE):
`kubectl kustomize ./application/overlays/gke | kubectl apply -f-`
